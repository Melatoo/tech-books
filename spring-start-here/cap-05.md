# Anotações

# Bean scopes e ciclo de vida

O Spring possui múltiplas abordagens para criar beans e gerenciar seu ciclo de vida, chamamos essas abordagens de escopo. Os dois escopos mais utilizados são: *singleton* e *prototype*.

## Escopo Singleton

O escopo *singleton* é a abordagem padrão do Spring para gerenciar beans no seu contexto.

### Como beans *singleton* funcionam

Ao contrário do padrão de design com mesmo nome, o escopo *singleton* permite mais de uma instância para um mesmo tipo, sendo único apenas por nome e permitindo definições como:

```java
@Configuration
public class ProjectConfig {

	@Bean
	public CommentService commentService1() {
		return new CommentService();
	}
  
	@Bean
	public CommentService commentService2() {
		return new CommentService();
	}
}
```

Sendo assim, nesse escopo, `commentService1` terá apenas um bean no contexto do spring, assim como `commentService2` terá outro bean diferente, mas também único para o seu nome. Para exemplificar essa abordagem, vamos definir um Bean utilizando a annotation `@Bean`:

```java
@Configuration
public class ProjectConfig {
	@Bean
	public CommentService commentService() {
		return new CommentService();
	}
}
```

Sendo que, ao atribuir à duas variáveis diferentes a referência para CommentService, elas possuirão a mesma referência:

```java
public class Main {
	public static void main(String[] args) {
		var c = new AnnotationConfigApplicationContext(ProjectConfig.class);
		
		var cs1 = c.getBean("commentService", CommentService.class);
		var cs2 = c.getBean("commentService", CommentService.class);
		
		boolean b1 = cs1 == cs2;
	
		System.out.println(b1); // true
	}
}
```

### Beans *singleton* no mundo real

Aplicativos no mundo real podem ser executados em múltiplas threads, onde a mesma instância de um objeto é partilhado entre elas. Caso essas threads resolvam alterar a instância, podemos encontrar um cenário de *race-condition*, onde mais de uma thread tenta alterar a mesma instância. Esse cenário pode ser tratado utilizando sincronia de threads, mas pode afetar drasticamente a performance do app.

Sendo assim, beans *singleton* não são recomendados para beans mutáveis. Esse é um dos motivos que tornam a injeção por construtor (definida no capítulo 3) como a mais recomendada nos casos gerais, já que permite a definição dessas dependências como `final`, tornando-as imutáveis.

### *Eager* e *lazy instatiantions*

Por padrão, o Spring cria todas as suas instâncias quando é inicializado, isso é chamado de *eager instatiation*. Entretanto, é possível definir que o Spring apenas crie instâncias quando é necessário, ou seja, quando alguém se refere àquele bean. Essa segunda abordagem é chamada *lazy instatiation*.

No geral, é mais recomendado utilizar a instanciação padrão *Eager*, já que o custo performático de sempre checar se o objeto existe para saber se deve instanciá-lo normalmente não justifica o ganho de memória. Além de que, caso algum Bean tenha problemas para inicializar, é bom que vejamos diretamente quando o Spring inicia. Entretanto, em alguns casos específicos onde parte do app não será utilizado em certos clientes ou versões, é justificável o uso da *annotation* `@Lazy`, que define que aquele bean utilizará a instanciação *lazy*, como descrito no exemplo a seguir:

```java
@Service
@Lazy
public class CommentService {
	public CommentService() {
		System.out.println("CommentService instance created!");
	}
}
```

## Escopo *prototype*

### Como beans *prototype* funcionam

Ao contrário do escopo *singleton*, no escopo *prototype* sempre que for pedido ao Spring uma nova referência a um bean, uma nova instância será criada. Para definir um bean como *prototype*, basta usar a *annotation* `@Scope`, e passar como parâmetro a definição `SCOPE_PROTOTYPE()`, como por exemplo:

```java
@Configuration
public class ProjectConfig {
	@Bean
	@Scope(BeanDefinition.SCOPE_PROTOTYPE)
	public CommentService commentService() {
		return new CommentService();
	}
}
```

### Beans *prototype* no mundo real

Seguindo a mesma ideia de múltiplas threads apresentada no *singleton*, utilizamos beans *prototype* quando alguma classe que use o bean como dependência precisa mudar algum de seus atributos, evitando a *race-condition*. Um exemplo seria:

```java
@Component
@Scope(BeanDefinition.SCOPE_PROTOTYPE)
public class CommentProcessor {
	@Autowired
	private CommentRepository commentRepository;
}
```

E sua chamada dentro do `CommentService` seria:

```java
@Service
public class CommentService {
	@Autowired
	private ApplicationContext context;
	
	public void sendComment(Comment c) {
		CommentProcessor p = context.getBean(CommentProcessor.class);
		
		p.setComment(c);
		p.processComment(c);
		p.validateComment(c);
		c = p.getComment();
	}
}
```

É necessário manter-se atento para não injetar o bean *prototype* diretamente em uma classe *singleton.* Isso faria com que o Spring injetasse a dependência apenas uma vez, quebrando o conceito de se usar o escopo *prototype*. Um exemplo desse caso seria:

```java
@Service
public class CommentService {
	@Autowired
	private CommentProcessor p; // Dessa maneira o Spring injeteria CommentProcessor na sua criação, mas CommentService só é criado 1x
	
	public void sendComment(Comment c) {
		p.setComment(c);
		p.processComment(c);
		p.validateComment(c);
		c = p.getComment();
	}
}
```