# Anotações

# Relacionando Beans

Em programação orientada a objetos, é comum que um objeto tenha que delegar responsabilidades a outros. Isso não é diferente no Spring, e há duas principais maneiras de estabelecer relações no framework:

1. Chamar diretamente o método de construção (wiring).
2. Permitir que o Spring nos entregue um valor usando um parâmetro de método (auto-wiring).

## Implementando relações entre Beans definidos com `@Bean`

Dentro de Beans definidos com `@Bean`, possuímos duas maneiras de estabelecer relações:

1. Usando chamada direta.
2. Usando os parâmetros do método da annotation `@Bean`.

### Chamada direta

Podemos definir uma relação entre dois Beans com uma chamada direta do construtor que queremos relacionar, por exemplo:

```java
@Configuration
public class ProjectConfig {

	@Bean
	public Parrot parrot() {
		Parrot p = new Parrot();
		p.setName("Koko");
		return p;
	}

	@Bean
	public Person person() {
		Person p = new Person();
		p.setName("Ella");
		p.setParrot(parrot());
		return p;
	}
}
```

É comum pensarmos que, ao utilizar a chamada direta, duas instâncias de uma mesma classe são criadas. Entretanto, o Spring é inteligente o suficiente para entender que, caso já tenha uma instância com as mesmas características inseria no contexto, ele apenas atribui a referência para a segunda chamada do construtor.

### Utilizando os parâmetros

Uma alternativa para definir relações de Beans é adicionar um parâmetro ao método da annotation, o tipo do parâmetro deve ser a classe a ser relacionada. Um exemplo seria:

```java
@Configuration
public class ProjectConfig {
	@Bean
	public Parrot parrot() {
		Parrot p = new Parrot();
		p.setName("Koko");
		return p;
	}

	@Bean
	public Person person(Parrot parrot) {
		Person p = new Person();
		p.setName("Ella");
		p.setParrot(parrot);
		return p;
	}
}
```

Dessa maneira, ao realizar a chamada do método `person()`, o Spring sabe que tem de procurar por uma instância de parrot para injetar no parâmetro do método. Ao dizer injetar, me refiro ao conceito de *dependency injection* (DI), cuja ideia é deixar com que o Spring injete o valor dentro de um campo ou referência específica. O DI é uma aplicação do princípio de IoC, que implica em, no tempo de execução, entregar o controle da aplicação ao framework.

## Implementando relações com a *annotation* `@Autowired`

Usando a *annotation* `@Autowired`, marcamos uma propriedade de um objeto onde queremos que o Spring injete um valor do contexto. Essa marcação é utilizada diretamente na classe que definimos que o objeto precisa da dependência. Há três maneiras de se utilizar essa *annotation:*

1. Injetar o valor no campo da classe.
2. Injetar o valor pelos parâmetros do construtor.
3. Injetar o valor através de um setter.

### Injetando o valor pelo campo da classe

Essa é a mais simples das três maneiras de definir uma dependência com `@Autowired`, basta utilizar a *annotation* acima do campo, por exemplo:

```java
@Component
public class Person {
 private String name = "Ella";
 
 @Autowired
 private Parrot parrot;
}
```

Entretanto, essa abordagem é mais utilizada em exemplos, testes de unidades e PoCs, não sendo muito comum em códigos de produção. Isso se dá principalmente por dois motivos:

1. Não é possível definir campos `@Autowired` como `final`.
2. É mais difícil de gerenciar o valor na inicialização.

### Injetando o valor pelo construtor

Definir a dependência através do construtor é a maneira mais recomendada para códigos de produção, já que inibe os dois motivos ruins abordados anteriormente. Um exemplo desse tipo de abordagem seria:

```java
@Component
public class Person {
 private String name = "Ella";
 private final Parrot parrot;
 
 @Autowired
 public Person(Parrot parrot) {
	 this.parrot = parrot;
 }
}
```

Desde o Spring 4.3, caso tenha apenas um construtor definido na classe, é possível omitir a escrita da *annotation* `@Autowired`.

### Injetando o valor pelo Setter

Essa abordagem é a menos recomendada, normalmente sendo encontrada em apps antigos e códigos legados. Ela também não permite definir o campo como `final` e piora a legibilidade do código. Um exemplo seria:

```java
@Component
public class Person {
 private String name = "Ella";
 private Parrot parrot;

 @Autowired
 public void setParrot(Parrot parrot) {
	 this.parrot = parrot;
 }
}
```

## Lidando com dependências circulares

Dependência circular, em Spring, é uma situação em que, para criar o Bean A, é necessário criar o Bean B, que por sua vez também depende do Bean A. Ou seja, para criar A é necessário criar B, que também depende da criação de A, deixando o Spring preso. Um exemplo seria:

```java
@Component
public class Person {
 private final Parrot parrot;
 
 @Autowired
 public Person(Parrot parrot) {
	 this.parrot = parrot; // Person depende de parrot
 }
}

public class Parrot {
 private String name = "Koko";
 
 private final Person person;
 
 @Autowired
 public Parrot(Person person) {
	 this.person = person; // Parrot depende de person
 }
}
```

## Escolhendo dentre múltiplos Beans no Spring context

Caso tenha mais de um Bean do mesmo no contexto do Spring, o framework pode reagir de diferentes maneiras, a depender da sua implementação. Essas maneiras são:

1. Caso o identificador do parâmetro seja o mesmo que o nome de um dos Beans do contexto, o Spring o escolherá.
    
    ```java
    @Configuration
    public class ProjectConfig {
     @Bean
     public Parrot parrot1() {
    	 Parrot p = new Parrot();
    	 p.setName("Koko");
    	 
    	 return p;
     }
     
     @Bean
     public Parrot parrot2() {
    	 Parrot p = new Parrot();
    	 p.setName("Miki");
    	 
    	 return p;
     }
     
     @Bean
     public Person person(Parrot parrot2) {
    	 Person p = new Person();
    	 p.setName("Ella");
    	 p.setParrot(parrot2); // O Bean parrot2 será escolhido
    	 
    	 return p;
     }
    }
    ```
    
2. Caso não tenha um Bean com mesmo nome do identificador do parâmetro, há as seguintes opções:
    1. Caso algum Bean tenha sido marcado com `@Primary`, ele será escolhido;
    2. É possível explicitar a escolha de um Bean através da *annotation* `@Qualifier`.
        
        ```java
        @Component
        public class Person {
         private String name = "Ella";
         private final Parrot parrot;
         
         public Person(@Qualifier("parrot2") Parrot parrot) {
        	 this.parrot = parrot;
         }
        }
        ```
        
    3. Caso nenhuma dessas opções aconteça, o Spring lança uma exceção.