# Anotacoes

# Aspects

*Aspects* são maneiras do framework interceptar uma chamada de um método e possivelmente alterando sua execução.

## Como *aspects* funcionam no Spring

Um *aspect* é apenas uma lógica que o framework executa quando chamamos um método específico. Quando projetamos um *aspect*, estamos defindo:

- **Aspect**: O código a ser executado na chamada de um método específico.
- **Advice**: Quando o app deveria executar a lógica do aspect, por exemplo: antes ou depois da execução do método.
- **Pointcut**: Quais métodos serão interceptados pelo framework.

Também existem os *join points*, que definem o evento que deslanchará a execução do *aspect*. Entretanto, no Spring os *join points* são sempre métodos.

Para que o spring seja capaz de interceptar um método, o objeto deve ser um bean. Quando um bean que é um alvo de *aspect* é refernciado, o Spring retorna um intermediador, um *proxy*, ao invés do bean real. Essa abordagem se chama *weaving*.

Nessa lógica, ao chamar um método interceptado, o *proxy* executa a lógica implementada no *aspect*, e depois delega a chamada do método real.

## Implementando a*spects* com AOP do Spring

Iremos utilizar como exemplo

### Implementando um *aspect* simples

Para criar um *aspect*, são necessário quatro passos:

1. Habilitar o mecanismo do *aspect* adicionando a *annotation* `@EnableAspectJAutoProxy`.
    
    ```java
    @Configuration
    @ComponentScan(basePackages = "services")
    @EnableAspectJAutoProxy
    public class ProjectConfig {}
    ```
    
2. Criar uma nova classe, definir como bean e utilizar a *annotation* `@Aspect`.
    
    Um *aspect* de log seria:
    
    ```java
    @Aspect
    public class LoggingAspect {
    	public void log() {
    		// ainda será implementado
    	}
    }
    
    // definir como bean de qualquer maneira
    ```
    
3. Definir o método que será implementado a lógica e dizer ao Spring quais métodos e quando interceptá-los.
    
    Para isso, usamos as *advice annotations*, um exemplo:
    
    ```java
    @Aspect
    public class LoggingAspect {
    	@Around("execution(* services.*.*(..))")
    	public void log(ProceedingJoinPoint joinPoint) {
    		joinPoint.proceed(); // delega ao execução ao método normal
    	}
    }
    ```
    
    A *annotation* `@Around` permite performar uma lógica antes e depois do método. O parâmetro dessa *annotation* define quais métodos esse *aspect* interceptará, nesse caso, todos os métodos do pacote services, sejam quaisquer seus tipos de retorno, classes, nomes e parâmetros.
    
4. Implementar a lógica do *aspect*.
    
    Por fim, basta implementar a lógica necessária. Como:
    
    ```java
    @Aspect
    public class LoggingAspect {
    	private Logger logger = Logger.getLogger(LoggingAspect.class.getName());
    	
    	@Around("execution(* services.*.*(..))")
    	public void log(ProceedingJoinPoint joinPoint) throws Throwable {
    		logger.info("Method will execute");
    		joinPoint.proceed();
    		logger.info("Method executed");
    	}
    }
    ```
    
    O método `joinPoint.proceed()` delega a execução ao método real. Caso esse método não seja utilizado, o método real nunca é executado. Essa abordagem pode ser útil caso realmente não queiramos que o método execute, dentro de uma validação, por exemplo.
    

### Alterando parâmetros e valor retornado de métodos interceptados

O parâmetro de tipo `ProceedingJoinPoint` representa o método interceptado, com ele é possível alterar os parâmetros e o valor retornado de um método interceptado por um *aspect*. Um exemplo seria: 

```java
@Aspect
public class LoggingAspect {
	private Logger logger = Logger.getLogger(LoggingAspect.class.getName());
	
	@Around("execution(* services.*.*(..))")
	public Object log(ProceedingJoinPoint joinPoint) throws Throwable {
		String methodName = joinPoint.getSignature().getName();
		Object [] arguments = joinPoint.getArgs();
		
		logger.info("Method " + methodName +
								" with parameters " + Arrays.asList(arguments) +
								" will execute");
		
		Object returnedByMethod = joinPoint.proceed();
		logger.info("Method executed and returned " + returnedByMethod);
		return returnedByMethod;
	}
}
```

Onde `joinPoint.getSignature().getName()` retorna o nome do método, `joinPoint.getArgs()` retorna os argumentos e `joinPoint.proceed()` delega ao método real e retorna o valo retornado pelo método real.

Além disso, o valor retornado pela chamada original do método é o valor definido no *aspect*.

Por fim, é possível alterar os parâmetros da chamada original do método através do `proceed()`, onde seus parâmetros serão os parâmetros da execução do método real e, caso estejam vazios, se mantem os mesmos parâmetros da chamada original.

### Interceptando métodos *annotated*

É possível criar uma *annotation* customizada para definir quais métodos queremos interceptar. Para isso, são necessários dois passos:

1. Definir uma *annotation* customizada e torná-la acessível em tempo de execução.
    
    ```java
    @Retention(RetentionPolicy.RUNTIME) // permite que a annotation seja interceptada em tempo de execução
    @Target(ElementType.METHOD) // restringe para ser utilizada apenas com métodos
    public @interface ToLog {}
    ```
    
2. Utilizar uma *pointcut expression* para que o método do *aspect* conte a ele quais métodos interceptar.
    
    ```java
    @Aspect
    public class LoggingAspect {
    	@Around("@annotation(ToLog)")
    	public Object log(ProceedingJoinPoint jp) {}
    }
    ```
    
    Dessa maneira, a *pointcut expression* `"@annotation(ToLog)"` define que o método `log(ProceedingJoinPoint jp)` será executado ao interceptar métodos com a *annotation* `@ToLog`.
    

### Algumas outras *advice annotations*

Utilizar a *annotation* `@Around` permite flexibilizar quando a lógica do *aspect* será executada ao interceptar o método, alterando a lógica como necessário.

Entretanto, caso não seja necessário essa flexibilidade, algumas *advice annotations* que podem ser úteis são:

- `@Before`: Chama a lógica do *aspect* antes da execução do método.
- `@AfterReturning`: Chama a lógica do *aspect* após o método retornar um valor, e esse valor é definido como parâmetro para o método do *aspect*. Não roda caso o método original lance uma exceção.
- `@AfterThrowing`: Chama a lógica do *aspect* após o método lançar uma exceção, e passa como parâmetro para o método do *aspect*.
- `@After`: Chama a lógica do *aspect* após a execução do método.

Nessas *annotations*, não existe o parâmetro `ProceedingJoinPoint`, e nem é possível decidir quando delegar para o método real.

## A cadeia de execução do *aspect*

Muitas vezes, um método pode ser interceptado por mais de um aspect. Quando isso acontece, eles precisam executar um após o outro, criando uma cadeia de execução. A ordem de execução é importante, pois ordens diferentes podem ter resultados diferentes. 

Por padrão, o Spring não garante a ordem de execução dos aspects. Para definir a ordem, pode-se utilizar a *annotation* `@Order`. Essa *annotation* recebe um valor numérico que representa a ordem na cadeia de execução. Quanto menor o número, mais cedo o aspect é executado (maior prioridade). Se dois *aspects* tiverem o mesmo valor, a ordem entre eles não é definida.

```java
@Aspect
@Order(1) // Executará primeiro
public class SecurityAspect {
    // ...
}

@Aspect
@Order(2) // Executará depois do SecurityAspect
public class LoggingAspect {
    // ...
}
```

Um `SecurityAspect` pode não delegar a execução em todos os casos. Se ele for executado primeiro, o `LoggingAspect` pode nunca ser executado, e a chamada não seria registrada no log.