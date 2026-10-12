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
