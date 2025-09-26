# Implementando web apps com Spring Boot e Spring MVC

## Apps com views dinâmicas

A grande maioria dos apps web modernos precisam mostrar dados dinâmicos para seus usuários. Com isso, precisamos que a página mude de acordo com os dados de resposta, chamamos isso de view dinâmica.

Para isso, além de usar a *annotation* `@RequestMapping` no método do controller, também utilizaremos um parâmetro do tipo `Model`, que armazena os dados que o controller enviará para a view. Um exemplo seria:

```java
@Controller
public class MainController {
	@RequestMapping("/home")
	public String home(Model page) {
		page.addAttribute("username", "Katy");
		page.addAttribute("color", "red");
		return "home.html";
	}
}
```

Onde o método `addAtribute()` adiciona um valor a ser passado para a view, sendo o primeiro parâmetro a key e o segundo o valor a ser passado.

### Obtendo dados na requisição HTTP

É possível enviar dados pela requisição HTTP das seguintes maneiras:

- Parâmetro de requisição HTTP (*request parameter)*: uma maneira simples de enviar pequenas quantidades de dados que seguem o formato de pares chave-valor. Para enviar dados dessa maneira, basta acrescê-los ao URI em uma expressão de requisição de busca. Também são chamados de parâmetros de query.
- Cabeçalho de requisição HTTP (*request header*): uma maneira parecida com os parâmetros da requisição, entretanto, não aparecem no URI, e sim no cabeçalho da requisição.
- Variável de caminho (*path variable*): envia dados pelo próprio caminho da requisição, utilizada para valores obrigatórios.
- Corpo de requisição HTTP (*request body*): utilizada para enviar grandes quantidades de dados, normalmente em string.

### Utilizando parâmetros de requisição para enviar dados do cliente para servidor

Essa maneira de se enviar dados é normalmente utilizada quando os dados contemplam dois critérios: 

- A quantidade de dados enviada não é larga, já que é limitada em cerca de 2000 caracteres.
- São dados opcionais.

Um caso de uso bem comum é enviar dados para definir buscas e critérios de filtragem.

Para utilizar essa maneira, basta acrescentar a expressão de requisição de busca, definida por `?`, e seguir com o formato de pares chave-valor, um exemplo seria: [`http://example.com/products?brand=honda`](http://example.com/products?brand=honda).

Para lidar esses dados dentro do controller, criamos um parâmetro com a *annotation* `@RequestParam`, um exemplo é:

```java
@Controller
public class MainController {
	@RequestMapping("/home")
	public String home(@RequestParam String color, Model page) {
		page.addAttribute("username", "Katy");
		page.addAttribute("color", color);
		return "home.html";
	}
}
```

Um parâmetro de requisição é obrigatório por padrão, para defini-lo como opcional, é necessário utilizar o parâmetro `optional=true` na *annotation*.

### Utilizando variáveis de caminho para enviar dados do cliente ao servidor

Essa maneira de enviar dados é mais fácil ser lida do que parâmetros de busca, a tornando mais amigável para mecanismos de buscas. Entretanto, não deve ser utilizada para dados opcionais é uma boa prática limitar o caminho em até duas variáveis. 

Para utilizar variáveis de caminho, definimos os dados diretamente no caminho do aplicativo, por exemplo: `http://localhost:8080/home/blue`. E, para serem lidos dentro do controller, basta utilizarmos um parâmetro com a *annotation* `@PathVariable`, um exemplo seria:

```java
@RequestMapping("/home/{color}")
public String home(@PathVariable String color, Model page) {
	page.addAttribute("username", "Katy");
	page.addAttribute("color", color);
	return "home.html";
}
```

## Utilizando os métodos HTTP GET e POST

Para expressar a intenção do cliente, uma requisição HTTP utiliza um verbo, que é o método HTTP. Se a intenção é apenas recuperar dados, por exemplo, o método GET é utilizado. Se a intenção for de alterar, adicionar ou deletar dados, outros verbos devem ser utilizados para representar essa intenção. É crucial que o método HTTP não seja utilizado de maneira contrária ao seu propósito.

Os métodos HTTP mais comuns são:

- **GET**: A requisição do cliente apenas recupera dados.
- **POST**: A requisição do cliente envia novos dados para serem adicionados pelo servidor.
- **PUT**: A requisição do cliente altera um registro de dados no lado do servidor.
- **PATCH**: A requisição do cliente altera parcialmente um registro de dados no lado do servidor.
- **DELETE**: A requisição do cliente deleta dados no lado do servidor.

É possível mapear múltiplas ações de um controller para o mesmo caminho, desde que utilizem métodos HTTP diferentes. Por padrão, a anotação `@RequestMapping` utiliza o método GET, mas isso pode ser alterado através do atributo `method`. 

```java
@RequestMapping(path = "/products", method = RequestMethod.POST)
public String addProduct() {}
```

No entanto, para manter o código mais claro e direto, geralmente são utilizadas anotações dedicadas para cada método HTTP, como `@GetMapping` e `@PostMapping`.

```java
@Controller
public class ProductsController {
    @GetMapping("/products")
    public String viewProducts(Model model) {
        return "products.html";
    }

    @PostMapping("/products")
    public String addProduct(
        @RequestParam String name,
        @RequestParam double price,
        Model model
    ) {
        return "products.html";
    }
}
```

Para enviar dados através de um método POST a partir do navegador, geralmente se utiliza um formulário HTML. A tag `<form>` é configurada com o `action` apontando para o caminho do endpoint e o `method` definido como `"post"`. Os campos de entrada (`<input>`) dentro do formulário terão seus valores enviados como parâmetros de requisição.

```html
<h2>Adicionar um produto</h2>
<form action="/products" method="post">
    Nome: <input type="text" name="name"><br/>
    Preço: <input type="number" step="any" name="price"><br/>
    <button type="submit">Adicionar produto</button>
</form>
```