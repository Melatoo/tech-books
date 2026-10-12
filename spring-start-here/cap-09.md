# Anotacoes

# Spring web scopes

Alguns tipos de escopos de beans são relevantes apenas em aplicativos web, eles são chamados de escopos web.

- Escopo de requisição (*request scope*): O Spring cria uma instância de uma classe bean para cada requisição HTTP. Essa instância vive apenas para aquela requisição específica.
- Escopo de sesssão (*session scope*): O Spring cria uma instância e a mantém na memória enquanto a sessão HTTP perdurar. Além disso, linka a instãncia com a sessão do cliente.
- Escopo de aplicação (*application scope*): Uma instância única no contexto, disponível enquando o app estiver executando.

Para exemplificar cada um dos escopos, construiremos uma página de login simples, que possuirá três etapas:

1. Implementar a lógica de login;
2. Manter os detalhes do usuário logado;
3. Contar as requisições de login.

## Utilizando o escopo de requisição

Esse escopo cria uma instância para cada requisição HTTP. Utilizaremos o escopo de requisição para construir a lógica do login, garantindo que as credenciais sensíveis do usuário existam apenas durante a requisição de login.

Para isso, usaremos a página HTML nomeada “login.html” a seguir:

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
	<meta charset="UTF-8">
	<title>Login</title>
</head>
<body>
	<form action="/" method="post">
		Username: <input type="text" name="username" /><br />
		Password: <input type="password" name="password" /><br />
		<button type="submit">Log in</button>
	</form>
	<p th:text="${message}"></p>
</body>
</html>
```

O controller a seguir lida com as requisições `GET` (para exibir a página) e `POST` (para processar o formulário):

```java
@Controller
public class LoginController {
	@GetMapping("/")
	public String loginGet() {
		return "login.html";
	}
	
	@PostMapping("/")
	public String loginPost(
		@RequestParam String username,
		@RequestParam String password,
		Model model
	) {
		boolean loggedIn = false;
		
		if (loggedIn) {
			model.addAttribute("message", "You are now logged in.");
		} else {
			model.addAttribute("message", "Login failed!");
		}
		
		return "login.html";
	}
}
```

Para a lógica de login, criamos um bean `LoginProcessor` com escopo de requisição. Ele armazenará as credenciais e as validará. Por ser `@RequestScope`, cada tentativa de login terá sua própria instância, evitando problemas de concorrência e garantindo que as credenciais sejam descartadas após a requisição.

```java
@Component
@RequestScope
public class LoginProcessor {
    private String username;
    private String password;

    public boolean login() {
        String username = this.getUsername();
        String password = this.getPassword();
        if ("natalie".equals(username) && "password".equals(password)) {
            return true;
        } else {
            return false;
        }
    }
}
```

## Utilizando o escopo de sessão

O escopo de sessão cria uma instância do bean por sessão HTTP de um cliente. Isso nos permite manter informações sobre o usuário logado enquanto ele navega por diferentes páginas da aplicação.

Continuando o exemplo, após um login bem-sucedido, queremos que o usuário permaneça logado. Para isso, criamos um serviço `LoggedUserManagementService` com escopo de sessão (`@SessionScope`).

```java
@Service
@SessionScope
public class LoggedUserManagementService {
    private String username;
}
```

Modificamos o `LoginProcessor` para, em caso de sucesso, armazenar o nome de usuário no bean de sessão.

Java

```java
@Component
@RequestScope
public class LoginProcessor {
    private final LoggedUserManagementService loggedUserManagementService;
    public LoginProcessor(LoggedUserManagementService loggedUserManagementService) {
        this.loggedUserManagementService = loggedUserManagementService;
    }

    public boolean login() {
        boolean loginResult = false;
        if ("natalie".equals(username) && "password".equals(password)) {
            loginResult = true;
            loggedUserManagementService.setUsername(username);
        }
        return loginResult;
    }
}
```

Agora, em outras partes da aplicação, podemos verificar se o usuário está logado injetando `LoggedUserManagementService` e checando se o `username` não é nulo. Se um usuário não logado tentar acessar uma página restrita, podemos redirecioná-lo para a página de login.

```java
@Controller
public class MainController {
    private final LoggedUserManagementService loggedUserManagementService;
    // ...
    @GetMapping("/main")
    public String home(@RequestParam(required = false) String logout, Model model) {
        if (logout != null) {
            loggedUserManagementService.setUsername(null);
        }

        String username = loggedUserManagementService.getUsername();
        if (username == null) {
            return "redirect:/";
        }
        model.addAttribute("username" , username);
        return "main.html";
    }
}
```

Finalmente, o `LoginController` é ajustado para redirecionar para a página principal (`/main`) após um login bem-sucedido.

```java
@PostMapping("/")
public String loginPost(...) {
    boolean loggedIn = loginProcessor.login();
    if (loggedIn) {
        return "redirect:/main";
    }
    model.addAttribute("message", "Login failed!");
    return "login.html";
}
```

## Utilizando o escopo de aplicação

Um bean com escopo de aplicação é, na prática, um singleton para toda a aplicação web. Apenas uma instância do bean existe e é compartilhada por todas as requisições de todos os usuários. É importante ressaltar que seu uso é **desaconselhado para dados mutáveis** devido a problemas de concorrência (race conditions), sendo preferível usar um banco de dados.

Para demonstrar, vamos adicionar um contador de tentativas de login em nossa aplicação. Criaremos um `LoginCountService` com a anotação `@ApplicationScope`.

```java
@Service
@ApplicationScope
public class LoginCountService {
    private int count;

    public void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}
```

Injetamos este serviço no nosso `LoginProcessor` (que é `@RequestScope`) e chamamos o método `increment()` a cada tentativa de login.

```java
@Component
@RequestScope
public class LoginProcessor {
    private final LoginCountService loginCountService;

    public LoginProcessor(..., LoginCountService loginCountService) {
        this.loginCountService = loginCountService;
    }

    public boolean login() {
        loginCountService.increment();
    }
}
```

Por fim, exibimos o contador na página principal, obtendo o valor no `MainController` e passando-o para a view.

```java
// Dentro do MainController
@GetMapping("/main")
public String home() {
    int count = loginCountService.getCount();
    model.addAttribute("loginCount", count);
    return "main.html";
}
```