**Visão Geral das Coleções**  
- O capítulo introduz o Java Collections Framework (principalmente em `java.util`) para armazenar e manipular grupos de objetos.  
- Três interfaces principais: `List`, `Set` e `Map`.

**List e Ordenação**  
- `List` (ex.: `ArrayList`) é uma coleção ordenada que permite elementos duplicados.  
- Use `Collections.sort()` para ordenar uma `List`.  
- Objetos customizados devem implementar `Comparable` e o método `compareTo()` para definir a ordem natural (ex.: título).

**Comparator para Ordenação Múltipla**  
- Use `Comparator` quando precisar de ordenações alternativas (ex.: por artista, por ano).  
- `Comparator` é uma classe/objeto separado passado para a sobrecarga de `sort()` (ex.: `songList.sort(artistCompare)`).  
- Expressões lambda são uma forma concisa de criar `Comparator` sem classes separadas.

**Generics (`<>`)**  
- Generics fornecem segurança de tipo em coleções (ex.: `ArrayList<Song>`).  
- Operador diamante `<>` permite inferência de tipo: `new ArrayList<>()`.  
- Polimorfismo com generics é restrito: `List<Dog>` não é `List<Animal>`; use wildcards (`? extends Animal`) se necessário.

**Set e Unicidade**  
- `Set` representa coleções sem elementos duplicados.  
- `HashSet` identifica duplicados com `hashCode()` e `equals()`.  
- Regra: se `a.equals(b)` então `a.hashCode() == b.hashCode()`.

**TreeSet para Conjuntos Ordenados**  
- `TreeSet` mantém elementos em ordem.  
- Elementos devem implementar `Comparable` ou fornecer um `Comparator` ao construtor.

**Map para Pares Chave-Valor**  
- `Map` armazena pares chave–valor; chaves devem ser únicas. Implementação comum: `HashMap`.  
- Use `put(key, value)` para inserir e `get(key)` para recuperar.  
- `Map` não estende `Collection`, mas faz parte do Collections Framework.

**Métodos de Fábrica (Java 9+)**  
- `List.of()`, `Set.of()`, `Map.of()` criam coleções imutáveis e pré‑populadas.