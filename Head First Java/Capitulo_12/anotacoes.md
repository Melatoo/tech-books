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