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

Esse escopo cria uma instância para cada requisição HTTP. Utilizaremos o escopo de requisição para construir a lógica do login. Para isso, usaremos a página HTML nomeada “login.html” a seguir:

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

Quando o usuário entrar na página raiz da aplicação, uma requisição get será enviada ao servidor, o servidor deve retornar a página de login acima. Após isso, ao preencher e enviar o formulário, uma requisição POST será enviada, que deve utilizar a lógica de login para verificar se o usuário logará ou não. Para isso, basta o controller exemplificado a seguir:

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

Entretanto, esse controller é um singleton padrão do Spring