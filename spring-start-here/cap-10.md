# Implementando serviços REST

## Usando serviços REST para trocar dados entre apps

No Spring, um endpoint REST é apenas uma ação do controller mapeada para um método HTTP e um caminho, cuja resposta é diretamente o que o controller retorna.

Esse tipo de comunicação possui alguns pontos de atenção:

- Caso a ação do controller demore muito para completar, a chamada HTTP pode expirar e quebrar a comunicação
- Enviar grandes quantidades de dados também pode fazer a chamada HTTP pode expirar e quebrar a comunicação
- Muitas chamadas concorrentes em um endpoint pode colocar muita pressão no aplicativo e fazer com que falhe
- A conexão nunca é 100% confiável. Sempre existe a chance que um endpoint REST falhe devido à conexão

Quando implementamos uma comunicação usando REST, devemos sempre levar em conta o que nosso aplicativo fará em caso de falha de alguma chamada.

## Implementando um endpoint REST

Para implementar um endpoint REST em Spring, é necessário apenas definir que a ação do controller não retorne uma view, mas sim um dado. Para isso, basta utilizarmos a *annotation* `@RequestBody`. Um exemplo:

```java
@Controller
public class HelloController {
	GetMapping("/hello")
	@ResponseBody
	public String hello() {
		return "Hello!";
	}
}
```

Entretanto, escrever a anotação toda vez que formos implementar um endpoint não é interessante, para isso, podemos apenas definir que o controller é um controller REST, através da anotação `@RestController`. Ficaria da seguinte maneira:

```java
@RestController
public class HelloController {
	@GetMapping("/hello")
	public String hello() {
		return "Hello!";
	}
	@GetMapping("/ciao")
	public String ciao() {
		return "Ciao!";
	}
	
}
```

## Gerenciando a resposta HTTP

A resposta HTTP é como o backend envia dados de volta ao cliente. Ela contém: status da resposta, cabeçalhos (headers) e o corpo da resposta (body).

### Enviando objetos como corpo da resposta

Para enviar um objeto na resposta, basta que a ação do controller o retorne. Por padrão, o Spring converte o objeto para o formato JSON.

```java
@RestController
public class CountryController {
    @GetMapping("/france")
    public Country france() {
        Country c = Country.of("France", 67);
        return c; // Retorna um objeto Country
    }
}
```

A resposta JSON para o código acima seria:

```java
{
	 "name": "France",
	 "population": 67
}
```

### Definindo o status e os cabeçalhos da resposta

Para ter mais controle sobre a resposta, como definir um status HTTP específico ou adicionar cabeçalhos, utilizamos a classe `ResponseEntity`.

```java
@RestController
public class CountryController {
    @GetMapping("/france")
    public ResponseEntity<Country> france() {
        Country c = Country.of("France", 67);
        return ResponseEntity
            .status(HttpStatus.ACCEPTED) // Define o status para 202 Accepted
            .header("continent", "Europe") // Adiciona um cabeçalho customizado
            .header("capital", "Paris")
            .body(c); // Define o corpo da resposta
    }
}
```

### Gerenciando exceções no endpoint

Para lidar com erros, podemos separar a lógica de tratamento de exceções do controller. Uma forma limpa de fazer isso é usando um `@RestControllerAdvice`, que funciona como um *aspect* que intercepta exceções lançadas pelos controllers.

Primeiro, definimos uma classe com a anotação `@RestControllerAdvice`. Dentro dela, criamos métodos para tratar exceções específicas, anotados com `@ExceptionHandler`.

```java
@RestControllerAdvice
public class ExceptionControllerAdvice {

    @ExceptionHandler(NotEnoughMoneyException.class)
    public ResponseEntity<ErrorDetails> exceptionNotEnoughMoneyHandler() {
        ErrorDetails errorDetails = new ErrorDetails();
        errorDetails.setMessage("Not enough money to make the payment.");
        return ResponseEntity
            .badRequest() // Retorna status 400 Bad Request
            .body(errorDetails);
    }
}
```

Com isso, o controller fica mais simples, focando apenas no fluxo de sucesso, pois a exceção será capturada e tratada pelo `advice`.

```java
@RestController
public class PaymentController {
    // ...
    @PostMapping("/payment")
    public ResponseEntity<PaymentDetails> makePayment() {
        // Se processPayment() lançar NotEnoughMoneyException,
        // o @RestControllerAdvice irá tratar.
        PaymentDetails paymentDetails = paymentService.processPayment();
        return ResponseEntity
            .status(HttpStatus.ACCEPTED)
            .body(paymentDetails);
    }
}
```

## Usando o corpo da requisição para obter dados do cliente

Quando o cliente precisa enviar uma quantidade maior de dados (como um objeto complexo), utilizamos o corpo da requisição HTTP. No Spring, usamos a anotação `@RequestBody` no parâmetro da ação do controller para receber esses dados. O Spring automaticamente converte o JSON do corpo da requisição para um objeto Java.

```java
@RestController
public class PaymentController {
    @PostMapping("/payment")
    public ResponseEntity<PaymentDetails> makePayment(
        @RequestBody PaymentDetails paymentDetails) { // Recebe os detalhes do pagamento do corpo da requisição
        logger.info("Received payment " + paymentDetails.getAmount());
        return ResponseEntity
            .status(HttpStatus.ACCEPTED)
            .body(paymentDetails);
    }
}
```