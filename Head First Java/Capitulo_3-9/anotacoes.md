# Ciclo de Vida de um Objeto

## Tipos de variáveis:

### Instance Variables

Variáveis de instância são variáveis declaradas **dentro da uma classe**, mas não dentro de um método. Essas variáveis são os atributos que cada objeto individual possui.

### Local Variables

Variáveis locais são variáveis definidas **dentro de um método**, incluindo os parâmetros desse método. Essas variáveis possuem o mesmo tempo de vida do local em que foram definidas, ou seja, **apenas enquanto o método esteja na Pilha** (Stack).

## Onde as Coisas Vivem: A Pilha (Stack) e o Monte (Heap)

### Stack

A Pilha **é onde as invocações de métodos e suas variáveis locais vivem**. Quando um método é chamado, é criado um novo stack frame no topo da Pilha. Esses stack frames possuem o estado do método, incluindo a linha sendo executada no momento e suas variáveis locais.

O método no topo da Pilha **é sempre o método executando no momento**, e o stack frame é removido assim que o método termina, e todas as suas variáveis locais desaparacem.

### Heap (Garbage Collector Heap)

A heap **é a estrutura em que todos os objetos vivem**. Também é conhecida como Garbage Collector Heap, já que **é o GC quem gerencia essa estrutura**. Se uma variável de instância é uma referência para um objeto, ela também vive na heap. A vida de um objeto no Heap não está ligada a um método específico, mas sim à sua acessibilidade.

## Construtores

É o código que é executado quando você usa a palavra-chave `new` para instanciar um objeto. Sua principal função **é inicializar o estado do novo objeto** (ou seja, definir os valores iniciais de suas variáveis de instância). Um construtor **deve ter** **o mesmo nome da classe e não pode ter um tipo de retorno**.

Se você não escrever um construtor para sua classe, o compilador fornecerá um construtor padrão, sem argumentos. No entanto, **se você escrever qualquer construtor, o compilador não criará mais o padrão para você.**

Uma classe pode ter múltiplos construtores, desde que suas listas de argumentos sejam diferentes. Isso oferece flexibilidade para criar objetos de maneiras distintas.

## Encadeamento de construtores

Quando um objeto de uma subclasse é criado, o **construtor de sua superclasse também deve ser executado**. Essa chamada acontece em cadeia até o construtor da classe Object.

A chamada para o construtor da superclasse é feita usando `super()`. Se você não a colocar explicitamente, o compilador insere uma chamada para `super()` sem argumentos como a primeira instrução do seu construtor.

A chamada para `super()` (ou `this()`, que chama outro construtor na mesma classe) **deve ser obrigatoriamente a primeira instrução em um construtor**.

Se a superclasse tiver um construtor com argumentos, a subclasse deve fazer uma chamada explícita a `super()` com os argumentos apropriados.

## **A Vida: Escopo e Acessibilidade**

A longevidade de um objeto é determinada pela vida das referências que apontam para ele.

### **Vida da Referência**

Se a referência é uma **variável local**, ela vive na Pilha e morre quando o método termina. Se é uma **variável de instância**, ela vive no Heap, dentro de outro objeto, e morre quando esse objeto morre.

### **Vida do Objeto**

Um objeto no Heap permanece vivo enquanto houver pelo menos uma referência "viva" apontando para ele. Se a última referência viva a um objeto for destruída (por exemplo, saindo de escopo), o objeto se torna órfão e inacessível.

## A Morte: Garbage Collection

Quando um objeto se torna inacessível, ele fica elegível para a coleta de lixo. Em Java, não se destrói objetos diretamente, simplesmente os abandona. O Garbage Collector é o processo da JVM que, de tempos em tempos, varre o Heap em busca de objetos órfãos para liberar a memória que eles ocupavam.

Existem três principais maneiras de um objeto se tornar não-acessível:

1. **A referência sai de escopo permanentemente:** A variável que apontava para o objeto deixa de existir (ex: o método terminou).
2. **A referência é atribuída a outro objeto:** A variável que apontava para o objeto A agora aponta para o objeto B, abandonando A.
3. **A referência é definida como `null`:** A variável é explicitamente desconectada de qualquer objeto.