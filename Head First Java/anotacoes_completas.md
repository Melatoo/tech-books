# Pilares do Java

## Encapsulamento:

Em programação orientada a objetos, o ato de separar o código em partes, se mantendo o menos acoplado possível, é chamado de encapsular. A ideia é fazer o software flexível, onde cada parte depende o menos possível uma das outras. Daí nasce também o conceito de Encapsulamento, que é gerenciar o acesso à uma classe e os seus métodos e atributos. Dentro desse conceito, temos alguns modificadores de acesso diferentes.

### Tipos de Níveis de Acesso:

**Public**: acessível por todas as classes e subclasses, independente do pacote;

**Protected:** acessível apenas pelas classes e subclasses do próprio pacote. Cabe também dizer que não torna acessível para pacotes de uma subclasse que seja diferente da classe mãe;

**Default** **(Package-Private)**: acessível apenas pelas classes do próprio pacote;

**Private:** acessível apenas pela própria classe.

Como visamos sempre manter o menor nível de acoplamento possível, é comum que grande parte dos métodos e atributos de uma classe sejam Private. Sendo assim, é normal a implementação de métodos Get e Set, o nome desses métodos são bem sugestivos, já que suas responsabilidades são, respectivamente, obter e definir atributos de uma classe. Também possível definir regras de negócio e validações nesse método.

## Herança:

Herança é um mecanismo de POO que permite criar classes com base em classes já existentes, diminuindo a redundância. A classe já existente é chamada de superclasse, já a classe derivada é chamada de subclasse.
Essas classes criadas a partir de outras além de herdarem todas as características de seus supertipos, também podem adicionar mais características, seja na forma de variáveis e/ou métodos adicionais, bem como reescrever métodos já existentes na superclasse, o que chamamos de polimorfismo.

Ao definirmos uma herança, caso o construtor seja definido pelo programador, é necessário que a definição do construtor da superclasse seja a primeira declaração do construtor da subclasse, através da chamada de super().

### **O Problema da Herança Múltipla e a Solução com Interfaces**

O Java não permite herança múltipla (uma subclasse herdar de mais de uma superclasse). Isso acontece para evitar ambiguidades, como o "Deadly Diamond of Death", onde o compilador não saberia qual método de superclasse executar se ambos tivessem a mesma assinatura.
Para contornar esse problema, o Java utiliza interfaces. Uma classe pode estender (`extends`) apenas uma superclasse, mas pode implementar (`implements`) múltiplas interfaces, permitindo que um objeto "desempenhe múltiplos papéis".

### Classes Abstratas e Interfaces

**Classes Abstratas**: São classes que não podem ser instanciadas. Elas servem como um "modelo" para suas subclasses. Uma classe deve ser declarada `abstract` se possuir pelo menos um método abstrato. Métodos abstratos não têm corpo (`{}`) e devem ser implementados pela primeira subclasse concreta na hierarquia.

**Interfaces**: São como classes 100% abstratas. Todos os seus métodos são implicitamente `public` e `abstract`. Uma classe não estende (`extends`) uma interface, ela a implementa (`implements`). Interfaces são a chave para um polimorfismo mais flexível, pois classes de diferentes árvores de herança podem implementar a mesma interface.

### A Superclasse `Object`

Toda classe do Java é uma subclasse da superclasse Object. Sendo assim, todas as classes do Java herdam alguns métodos diretamente na sua criação, métodos da classe Object, como `toString()`, `equals()`, `hashCode()`, etc.
Essa superclasse possui dois principais propósitos: agir como um tipo polimórfico para métodos que precisam funcionar em classes que qualquer um faça e provisionar código de métodos reais que o próprio Java precisa em tempo de execução.

### Execução de Métodos da Superclasse

É possível invocar um método pela subclasse da maneira como ele é implementado na superclasse. Para isso, usamos a keyword super, como:

```java
abstract class Report {
	void runReport() {
		// Faz algo
	}
}

class NationalJournal extends Report {
	void runReport() {
		super.runReport();
		// Faz algo mais
 }
}
```

## Polimorfismo:

Em POO, polimorfismo permite que objetos de diferentes classes respondam à mesma mensagem (chamada de método) de maneiras específicas para cada classe. Isso geralmente é alcançado através da Herança e Interfaces.

### **O Poder e o "Preço" do Polimorfismo**

O verdadeiro poder do polimorfismo está em usar uma referência de uma superclasse para apontar para um objeto de uma subclasse.

**Poder:** Isso permite criar **argumentos e arrays polimórficos**. Um método que aceita `Animal` pode receber um `Dog`, e um array `Animal[]` pode conter `Dog`s e `Cat`s.
**O "Preço":** Ao usar uma referência de uma superclasse (como `Object` ou `Animal`), o compilador só permite chamar os métodos que existem na classe de referência. Mesmo que a referência `animal` aponte para um objeto `Dog`, você não pode chamar `animal.latir()`, pois o método `latir()` não existe na classe `Animal`.

### **Casting e `instanceof`**

Para resolver a limitação acima, você pode "recuperar" o tipo completo do objeto:

**Casting:** Você pode forçar o compilador a tratar a referência como seu tipo real através de um cast. Isso permite o acesso a todos os seus métodos.

```java
// animalRef aponta para um objeto Dog
Dog realDog = (Dog) animalRef;
realDog.latir(); // Funciona
```

**Segurança com `instanceof`:** Fazer um cast é arriscado, se o objeto não for do tipo esperado, ocorrerá uma `ClassCastException`. Para evitar isso, usamos o operador `instanceof` para verificar o tipo do objeto antes de fazer o cast.

```java
if (animalRef instanceof Dog) {
  Dog realDog = (Dog) animalRef;
}
```

### Formas de Polimorfismo em Java

Em Java, o polimorfismo se manifesta, principalmente, de duas formas:

**Overriding:** Ocorre quando uma subclasse fornece uma implementação específica para um método que já é definido em sua superclasse, através da anotação `@Override`. Métodos overridden devem ter o mesmo nível de acesso, ou níveis mais “amigáveis”, e a mesma assinatura que o método implementado na superclasse.

**Overloading:** Ocorre quando temos vários métodos com o mesmo nome, mas com parâmetros diferentes (seja na quantidade ou no tipo) dentro da mesma classe. Não é permitido que apenas o tipo de retorno do método overloaded seja trocado, mesmo que seja uma subclasse da classe retornada no método original. **É necessário que a lista de argumentos seja trocada para que o compilador defina que é um Overload** e não uma tentativa de Override.

Existem discussões se o Overloading é considerado polimorfismo ou não, pra mim isso é irrelevante, afinal, polimorfismo é um conceito abstrato e não algo exato. Sendo assim, vale a pena uma leve referência para os tipos de polimorfismo: 

**Polimorfismo estático:** Esse tipo de polimorfismo é resolvido em tempo de compilação, ou seja, quando o método é chamado, a JVM não precisa decidir qual de suas versões deve ser executada, isso já foi definido pelo próprio compilador.

**Polimorfismo dinâmico:** Esse tipo de polimorfismo é resolvido durante o tempo de execução, ou seja, quando um método é chamado a própria JVM decide qual de suas versões devem ser executadas.

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

# Estrutura de Dados: Collections e Generics

O Java oferece o **Java Collections Framework**, um conjunto de classes e interfaces no pacote `java.util` projetado para praticamente qualquer necessidade de armazenamento e manipulação de dados.

## A Interface `List` e a Classe `ArrayList`

Para listas de itens onde a sequência de inserção é importante e duplicatas são permitidas, a interface **`List`** é a escolha ideal. A **`ArrayList`** é sua implementação mais comum.

Para ordenar uma `List` de tipos que possuem uma "ordem natural", como `String` ou números, pode-se usar o método estático **`Collections.sort()`**. Ele ordena os elementos em ordem crescente (alfabética para Strings, numérica para números).

## O Problema com Tipos Personalizados: A Interface `Comparable`

Quando tentamos ordenar uma lista de objetos personalizados (como uma `List<Song>`), o compilador gera um erro. A razão é simples: o Java não sabe como comparar dois objetos `Song`. Ele não sabe se deve ordenar por título, artista ou ano.

A solução para definir uma "ordem natural" para uma classe é fazer com que ela implemente a interface **`Comparable`**. Esta interface obriga a implementação de um único método:

- **`int compareTo(T obj)`**: Este método compara o objeto atual (`this`) com o objeto recebido como parâmetro. Ele deve retornar:
    - Um número **negativo** se `this` for "menor" que `obj`.
    - **Zero** se `this` for "igual" a `obj`.
    - Um número **positivo** se `this` for "maior" que `obj`.

A classe `String` já implementa `Comparable`, e por isso `Collections.sort()` funciona perfeitamente com listas de Strings. Já uma classe genérica `Song`, pode-se simplesmente delegar a comparação para o `compareTo()` da `String` do título.

```java
class Song implements Comparable<Song> {
    String title;
    // ... outros atributos e construtor
    public int compareTo(Song other) {
        return this.title.compareTo(other.title);
    }
}
```

## Ordenação Múltipla

A interface `Comparable` define apenas uma forma de ordenação. Se é necessário múltiplos critérios (por exemplo, ordenar músicas por artista ou por BPM), é utilizada a interface **`Comparator`**.

Um `Comparator` é uma classe separada cuja única finalidade é definir uma lógica de comparação. Ele implementa o método **`int compare(T obj1, T obj2)`**. O método `sort()` da `List` (ou de `Collections`) possui uma versão sobrecarregada que aceita um `Comparator`.

```java
class ArtistCompare implements Comparator<Song> {
    public int compare(Song one, Song two) {
        return one.getArtist().compareTo(two.getArtist());
    }
}

// No código principal:
songList.sort(new ArtistCompare());
```

## Simplificando com Expressões Lambda

Criar uma classe separada para cada critério de ordenação é verboso. Como `Comparator` é uma **interface funcional** (possui um único método abstrato, ou SAM - Single Abstract Method), pode-se usar **expressões lambda** para fornecer a implementação do método `compare()` de forma concisa e direta.

```java
// Ordenando por artista usando uma expressão lambda
songList.sort((one, two) -> one.getArtist().compareTo(two.getArtist()));
```

## Lidando com Duplicatas

Se o objetivo é ter uma coleção que não permite elementos duplicados, a interface **`Set`** é a solução. A implementação mais comum é a **`HashSet`**, que é otimizada para buscas rápidas, mas não garante a ordem dos elementos.

## Igualdade de Objetos

Para que um `HashSet` (ou qualquer estrutura baseada em hash) funcione corretamente, a classe dos objetos armazenados deve sobrescrever os métodos **`equals()`** e **`hashCode()`** da classe `Object`.

O `HashSet` utiliza o `hashCode()` para determinar onde armazenar um objeto. Se dois objetos têm hash codes diferentes, o `Set` assume que são diferentes. Se os hash codes são iguais, o `Set` então chama o método `equals()` para confirmar se os objetos são realmente equivalentes.

### **O Contrato `equals()`/`hashCode()`**:

1. Se dois objetos são iguais de acordo com `equals()`, eles **DEVEM** ter o mesmo `hashCode()`.
2. Se dois objetos têm o mesmo `hashCode()`, eles **NÃO** são necessariamente iguais.

## Mantendo a Ordem e a Unicidade: `TreeSet`

Caso seja necessário uma coleção que seja ao mesmo tempo **única** e **ordenada**, a implementação a ser usada é a **`TreeSet`**. Para que funcione, os elementos adicionados devem:

1. Implementar a interface **`Comparable`**.
2. Ou, um **`Comparator`** deve ser fornecido ao construtor do `TreeSet`.

## Genéricos e Type-Safety

**Genéricos**, representados pelos colchetes angulares (`<>`), permitem criar coleções com segurança de tipos. Eles informam ao compilador o tipo de objeto que a coleção pode conter.

- **Sem Genéricos**: `ArrayList list = new ArrayList();` pode conter qualquer tipo de `Object`, o que exige `casts` e pode levar a erros em tempo de execução.
- **Com Genéricos**: `ArrayList<Song> songList = new ArrayList<>();` garante que apenas objetos `Song` (ou subclasses) podem ser adicionados, pegando erros em tempo de compilação.

## **Polimorfismo e Genéricos (com Wildcards)**

Uma peculiaridade importante é que a herança não se aplica diretamente aos tipos genéricos. Um método `processList(List<Animal> list)` não pode aceitar uma `List<Dog>`. Para permitir esse tipo de polimorfismo, é necessário utilizar **wildcards**:

- **`List<? extends Animal>`**: Significa "uma lista de qualquer tipo que seja uma subclasse de Animal". Esse tipo de lista se torna efetivamente "somente leitura" dentro do método, garantindo a segurança

# Streams e Lambdas

A JDK trás algumas bibliotecas que podem ser bastante úteis, principalmente para diminuir o tempo levado por tarefas comuns, já que precisamos dizer apenas o que queremos, sem ter de dizer o como. Uma dessas bibliotecas é a Streams API, que processa collections de objetos. Essa biblioteca é ainda mais poderosa se unida com expressões Lambdas.

## Streams

Uma Stream nada mais é que uma sequência de elementos que suporta processamentos através de um pipeline de operações.

### Começando uma Stream

Collections normais, como List, Map, Set, etc, não implementam Streams diretamente, ou seja, caso seja necessário utilizar métodos da Stream em alguma delas, é preciso primeiramente obter um objeto Stream daquela coleção. Para isso, a interface Collection possui o método `stream`:

```java
List<String> strings = List.of("I", "am", "a", "list", "of", "Strings");
Stream<String> stream = strings.stream(); // Retorna um objeto Stream
```

Com o objeto stream, é possível criar um pipeline de operações, como por exemplo:

```java
stream.limit(4).sorted().skip(2);
```

### Operações da Stream

Os métodos que uma Stream suporta para processar seus dados são chamados de operações intermediárias. Essas operações são a parte principal de uma Stream, entretanto, elas não processam os dados de fato, apenas sinalizam que aquela tarefa terá de ser feita. Sendo assim, as operações intermediárias são *lazy evaluated*, ou seja apenas definem a instrução do trabalho, e não realizam o trabalho em si.

### Operações Terminais

Para que uma Stream realize seu trabalho definido pelas operações intermediárias, é necessário realizar uma chamada de uma operação terminal, como:

```java
List<String> result = stream.collect(Collectors.toList());
```

Existem diversas operações terminais, como count(), min(), max(), etc. Mas o importante é que, ao chamá-las, a Stream entende que é hora de realizar suas operações e tenta fazer da maneira mais performática possível, ou seja, percorrendo seus dados a menor quantidade de vezes.

As operações de stream **não modificam a coleção original,** elas produzem um novo resultado a partir dela.

### Streams Paralelas

Para coleções muito grandes e operações complexas, a Streams API permite processar os dados em paralelo, utilizando múltiplos núcleos de CPU. Isso pode ser ativado simplesmente trocando `.stream()` por `.parallelStream()`.

```java
List<Song> rockSongs = songs.parallelStream() // Inicia uma stream paralela
    .filter(song -> song.getGenre().contains("Rock"))
    .collect(Collectors.toList());
```

## Expressões Lambda

Expressões Lambda são uma forma concisa de implementar **Interfaces Funcionais**, que são interfaces com um único método abstrato (SAM - *Single Abstract Method*). Elas permitem "passar comportamento" como argumento para um método.

### Estrutura

A anatomia de uma lambda é:`(parametros) -> { corpo da implementação }`

- Se o corpo tiver apenas uma linha, as chaves
    
    `{}` e o `return` são opcionais.
    
- Se houver apenas um parâmetro, os parênteses
    
    `()` são opcionais.
    

As interfaces funcionais mais comuns usadas com streams são:

- `Predicate<T>`: Recebe `T`, retorna `boolean`. Usado em `filter()`.
- `Function<T, R>`: Recebe `T`, retorna `R`. Usado em `map()`.
- `Consumer<T>`: Recebe `T`, retorna `void`. Usado em `forEach()`.
- `Supplier<T>`: Não recebe nada, retorna `T`.

### Referências de Métodos

Quando uma expressão lambda simplesmente chama um método já existente, você pode usar uma

**referência de método** como um atalho mais limpo.

```java
// Em vez desta lambda:
.map(song -> song.getGenre())

// Você pode usar esta referência de método:
.map(Song::getGenre)
```

### Operações Terminais e o `Optional`

Algumas operações terminais, como `findFirst()`, podem não encontrar um resultado. Em vez de retornar `null` (o que pode causar `NullPointerException`), esses métodos retornam um objeto `Optional`. `Optional` é um "contêiner" que pode ou não ter um valor. Para usá-lo com segurança, você deve primeiro verificar se um valor está presente com `isPresent()` antes de obtê-lo com `get()`.

```java
Optional<Song> result = songs.stream()
    .filter(s -> s.getYear() == 1995)
    .findFirst();

if (result.isPresent()) {
    Song foundSong = result.get(); // Seguro para chamar get() aqui
    System.out.println(foundSong.getTitle());
} else {
    System.out.println("Nenhuma música de 1995 encontrada.");
}
```