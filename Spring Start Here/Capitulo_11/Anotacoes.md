# Consumindo endpoints REST

É possível utilizar três ferramentas para consumir endpoints REST:

1. OpenFeign: Ferramenta oferecida pelo projeto Spring Cloud. A solução recomendada.
2. RestTemplate: Uma ferramenta bem conhecida pelos desenvolvedores. Era a maneira mais comum de se consumir endpoints REST, mas foi descontinuada a partir do Spring 5 e será eventualmente deprecada.
3. WebClient: Um recurso do Spring apresentado como um substituto do RestTemplate. Utiliza uma abordagem de programação diferente, chamada *reactive programming*.

## Utilizando o OpenFeign

Para utilizar o OpenFeign para consumir endpoints REST, é necessário apenas definir uma interface, cujos métodos consomem um endpoint REST, e a ferramenta provê a implementação. Sendo assim, é necessário apenas anotar o método, para definir o caminho, método HTTP e eventuais parâmetros, cabeçalhos e o corpo da requisição. Um exemplo seria:

```java
@FeignClient(name = "payments", url = "${name.service.url}")
public interface PaymentsProxy {
	@PostMapping("/payment")
	Payment createPayment(@RequestHeader String requestId, @RequestBody Payment payment);
}
```

Onde na anotação `@FeignClient` nós definimos o nome do proxy e a URI base da requisição. Já no método da interface, utilizamos as mesmas anotações de definição de um endpoint REST normais do Spring para definir o caminho, método HTTP, etc. Sendo assim, para utilizarmos o proxy, basta inserirmos a ferramenta na configuração:

```java
@Configuration
@EnableFeignClients(basePackages = "com.example.proxy")
public class ProjectConfig {}
```

E chamar o proxy no controller:

```java
@RestController
public class PaymentsController {
	private final PaymentsProxy paymentsProxy;
	
	public PaymentsController(PaymentsProxy paymentsProxy) {
		this.paymentsProxy = paymentsProxy;
	}
	
	@PostMapping("/payment")
	public Payment createPayment(@RequestBody Payment payment) {
		String requestId = UUID.randomUUID().toString();
		return paymentsProxy.createPayment(requestId, payment);
	}
}
```

## Utilizando o RestTemplate

Para começo de conversa, o RestTemplate está sendo descontinuado não por ser uma ferramenta ruim, mas por não apresentar funcionalidades que os desenvolvedores frequentemente precisam:

- Chamar endpoints de maneira tanto síncrona quanto assíncrona
- Escrever menos código e tratar menos exceções
- Retentar execuções de chamadas e implementar operações de fallback

Sendo assim, para utilizar o RestTemplate são necessários três passos:

1. Definir um cabeçalho HTTP criando e configurando uma instância de `HttpHeaders`.
2. Criar uma instância `HttpEntity` que representa os dados da requisição.
3. Enviar a chamada HTTP utilizando `exchange()` e obter a resposta HTTP.

Seguindo esses passos, um exemplo de criação de um proxy utilizando essa ferramenta seria:

```java
@Component
public class PaymentsProxy {
	private final RestTemplate rest;
	
	@Value("${name.service.url}")
	private String paymentsServiceUrl;
	
	public PaymentsProxy(RestTemplate rest) {
		this.rest = rest; // Injeta o RestTemplate por constructor DI
	}
	
	public Payment createPayment(Payment payment) {
		String uri = paymentsServiceUrl + "/payment";
		
		HttpHeaders headers = new HttpHeaders(); // Cria os headers HTTP
		headers.add("requestId", UUID.randomUUID().toString()); // Insere os valores do header
		
		HttpEntity<Payment> httpEntity = new HttpEntity<>(payment, headers); // Cria a instância HTTP
		
		ResponseEntity<Payment> response = rest.exchange(uri, HttpMethod.POST, httpEntity, Payment.class); // Utiliza o método exchange para consumir o endpoint
		
		return response.getBody();
	}
}
```

Por fim, basta definir um endpoint como no exemplo da ferramenta anterior.

## Utilizando WebClient

O `WebClient` é uma ferramenta utilizada em apps que seguem a metodologia **reativa**. Embora a documentação do Spring o recomende como substituto do `RestTemplate`, essa recomendação é mais válida para aplicações reativas. Para aplicações não reativas (tradicionais), o `OpenFeign` é a melhor alternativa.

A abordagem reativa muda a forma como as threads são gerenciadas. Em vez de uma thread ficar bloqueada esperando por uma operação de I/O (como uma chamada de rede), ela é liberada para executar outras tarefas. Quando a operação de I/O termina, o fluxo é retomado, otimizando o uso de recursos. A comunicação se dá por meio de *producers* (geralmente `Mono` ou `Flux`) e *subscribers*.

Para usar o `WebClient`, a dependência `spring-boot-starter-webflux` é necessária em vez da `spring-boot-starter-web`. Primeiro, criamos um bean de `WebClient`:

```java
@Configuration
public class ProjectConfig {
    @Bean
    public WebClient webClient() {
        return WebClient.builder().build();
    }
}
```

A implementação do proxy usa uma API fluente para construir a requisição. Note que o método não retorna o objeto `Payment` diretamente, mas sim um `Mono<Payment>`, que é o *producer* do resultado.

```java
@Component
public class PaymentsProxy {
    private final WebClient webClient;

    @Value("${name.service.url}")
    private String url;

    public PaymentsProxy(WebClient webClient) {
        this.webClient = webClient;
    }

    public Mono<Payment> createPayment(String requestId, Payment payment) {
        return webClient.post() // Define o método HTTP
            .uri(url + "/payment") // Define a URI
            .header("requestId", requestId) // Adiciona o header
            .body(Mono.just(payment), Payment.class) // Define o corpo da requisição
            .retrieve() // Envia a requisição
            .bodyToMono(Payment.class); // Obtém o corpo da resposta como um Mono
    }
}
```

O controller que utiliza este proxy também será reativo, recebendo e retornando objetos `Mono`, permitindo que o framework gerencie o fluxo de forma não bloqueante.

```java
@RestController
public class PaymentsController {
    private final PaymentsProxy paymentsProxy;

    public PaymentsController(PaymentsProxy paymentsProxy) {
        this.paymentsProxy = paymentsProxy;
    }

    @PostMapping("/payment")
    public Mono<Payment> createPayment(@RequestBody Payment payment) {
        String requestId = UUID.randomUUID().toString();
        return paymentsProxy.createPayment(requestId, payment);
    }
}
```