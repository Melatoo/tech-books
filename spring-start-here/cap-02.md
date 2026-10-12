# Anotações

## Definindo beans

Beans são instâncias de objetos que são controlados pelo Spring.

### Adicionando Beans com a annotation `@Bean`

Para adicionar Beans com a anotação `@Bean`, é necessário três passos:

1. Criar uma classe de configuração para o projeto, usando a annotation `@Configuration` .
2. Adicionar um método à classe de configuração que retorne a instância de um objeto da classe que queremos definir como Bean, por exemplo:

```java
@Configuration
public class ProjectConfig {
	@Bean // Instrui o Spring a chamar esse método para inicialização e adicionar ao spring-context
	Parrot parrot() { // O nome do método se torna o nome do Bean
		var p = new Parrot();
		p.setName("Koko");
		return p;
	}
}
```

3. Fazer o spring utilizar a classe de configuração.

    ```java
    public class Main {
     public static void main(String[] args) {
    	 // Ao criar uma instância do Spring Context, passa a classe de configuração
    	 var context = new AnnotationConfigApplicationContext(ProjectConfig.class);

    	 Parrot p = context.getBean(Parrot.class); // Obtém a referência pelo context

    	 System.out.println(p.getName());
     }
    }
    ```

É possível definir mais de um tipo de Bean para uma mesma classe:

```java
@Configuration
public class ProjectConfig {
	@Bean
	@Primary // Define que, se não for passada uma instância específica na criação, a instância a ser criada é parrot1
	Parrot parrot1() {
		var p = new Parrot();
		p.setName("Koko");
		return p;
	}

	@Bean
	Parrot parrot2() {
		var p = new Parrot();
		p.setName("Miki");
		return p;
	}

	@Bean
	Parrot parrot3() {
		var p = new Parrot();
		p.setName("Riki");
		return p;
	}
}
```

Entretanto, precisaremos especificar qual instância queremos utilizar quando chamamos a criação de um Bean. Para isso, é necessário explicitar o nome da instância, como:

```java
Parrot p = context.getBean("parrot2", Parrot.class);
```

Assim o Spring consegue definir de qual instância estamos falando. Para alterar o nome da instância, é possível utilizar um argumento na anotação do Bean, como:

```java
@Bean(name = "miki")
@Bean(value = "miki")
@Bean("miki")
```

### Adicionando Beans com _stereotype annotations_

Com anotações de estereótipo, é possível definir Beans com menos menos código, é necessário dois passos:

1. Usar a annotation `@Component` na classe que queremos adicionar suas instâncias ao contexto do Spring.
```java
@Component
public class Parrot {
	private String name;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}
}
```

2. Usar a annotation `@ComponentScan` na classe de configuração.
```java
public class Main {
	public static void main(String[] args) {
		var context = new AnnotationConfigApplicationContext(ProjectConfig.class);
		Parrot p = context.getBean(Parrot.class);
		System.out.println(p);
	}
}
```

Nos casos do mundo real é bem mais comum de utilizarmos esse tipo de definição de beans, já que é necessário escrever menos código. Entretanto, utilizando a annotation @Bean você possui mais controle sobre a criação da instância que é adicionada ao Spring-context (além de outros benefícios, como definição de mais um Bean e Beans de tipos primitivos). Apesar de que, em stereotype annotations é possível utilizar a annotation `@PostConstruct` para isso.

### Adicionando Beans programaticamente

A partir do Spring 5, é possível adicionar Beans programaticamente, que permite adicionarmos novas instâncias ao contexto diretamente. Para isson é necessário chamar o método `registerBean()`, cuja assinatura é:

```java
<T> void registerBean(
 String beanName,
 Class<T> beanClass,
 Supplier<T> supplier,
 BeanDefinitionCustomizer... customizers);
```

Um exemplo seria:

```java
public class Main {
	public static void main(String[] args) {
		var context = new AnnotationConfigApplicationContext(ProjectConfig.class);

		Parrot x = new Parrot(); // Cria a instância a ser adicionada ao contexto
		x.setName("Kiki");

		Supplier<Parrot> parrotSupplier = () -> x; // Define um supplier para retornar a instância

		context.registerBean("parrot1", Parrot.class, parrotSupplier); // Chama o método com seus parâmetetros

		Parrot p = context.getBean(Parrot.class);
		System.out.println(p.getName());
	}
}
```
