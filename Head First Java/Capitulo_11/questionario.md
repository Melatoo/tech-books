Questionário de Java: Estruturas de Dados e Coleções
Pergunta 1: De acordo com o capítulo, qual é a principal diferença entre as interfaces List e Set da API de Coleções do Java?

List não pode conter valores nulos, enquanto Set pode.

List mantém a ordem de inserção e permite elementos duplicados, enquanto Set não permite duplicatas.

Set é sempre ordenado, enquanto List não é.

É mais rápido adicionar elementos a uma List do que a um Set.

Dica: Pense em uma lista de compras versus um conjunto de ingredientes únicos necessários para uma receita.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 2. List mantém a ordem de inserção e permite elementos duplicados, enquanto Set não permite duplicatas. 



<strong>Justificativa:</strong> Correto. List é sobre sequência e índice, permitindo duplicatas. Set é sobre unicidade e não permite elementos duplicados.
</details>

Pergunta 2: Por que a chamada Collections.sort(songList) falha quando songList é uma List<Song> mas funciona quando é uma List<String>?

Porque o método sort() só funciona com tipos primitivos e Strings.

Porque a classe Song não implementa a interface Comparable.

Porque objetos personalizados como Song não podem ser colocados em uma List.

Porque a classe Song precisa ter um método chamado sort().

Dica: Como o método sort() sabe se uma música vem antes ou depois de outra?

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 2. Porque a classe Song não implementa a interface Comparable. 



<strong>Justificativa:</strong> Correto. Para que Collections.sort() possa ordenar uma lista, os elementos da lista precisam ter uma 'ordem natural'. Isso é alcançado implementando a interface Comparable e seu método compareTo(). A classe String já implementa Comparable.
</details>





Pergunta 3: Se você precisa ordenar uma lista de objetos Song de várias maneiras diferentes (por artista, por BPM, etc.), qual é a abordagem recomendada no capítulo?

Criar múltiplas versões da classe Song, cada uma com um método compareTo() diferente.

Adicionar uma variável na classe Song para controlar como o método compareTo() deve se comportar.

Usar o método sort() que aceita uma Comparator, e criar uma classe Comparator separada (ou um lambda) para cada critério de ordenação.

Converter todos os objetos para String e depois ordenar a lista de Strings.

Dica: O capítulo apresenta uma solução para quando a 'ordem natural' de um objeto não é suficiente.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 3. Usar o método sort() que aceita uma Comparator, e criar uma classe Comparator separada (ou um lambda) para cada critério de ordenação. 



<strong>Justificativa:</strong> Correto. O Comparator é uma classe externa que define uma lógica de comparação. Você pode criar vários Comparators para a mesma classe e passá-los para o método sort(), permitindo múltiplas ordens de classificação.
</details>

Pergunta 4: Para que um HashSet considere dois objetos diferentes como 'duplicatas' e evite adicionar o segundo, o que deve ser verdade sobre esses objetos?

Ambos devem ter o mesmo valor retornado pelo método toString().

Ambos devem implementar a interface Comparable e compareTo() deve retornar 0.

O método equals() deve retornar true para eles, e ambos devem retornar o mesmo valor de hashCode().

As variáveis de referência para os dois objetos devem ser iguais (usando ==).

Dica: O HashSet precisa de duas 'confirmações' para ter certeza de que encontrou um item duplicado.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 3. O método equals() deve retornar true para eles, e ambos devem retornar o mesmo valor de hashCode(). 



<strong>Justificativa:</strong> Correto. O HashSet primeiro usa hashCode() para encontrar o 'balde' onde o objeto deveria estar e depois usa equals() para confirmar se algum objeto naquele balde é realmente uma duplicata.
</details>

Pergunta 5: O que acontece em tempo de execução se você tentar adicionar objetos de uma classe que NÃO implementa Comparable a um TreeSet (sem fornecer um Comparator ao construtor)?

Nada, o TreeSet aceitará os objetos e os manterá na ordem em que foram adicionados.

Ocorre um erro de compilação, pois o compilador sabe que a classe não é Comparable.

Ocorre uma ClassCastException em tempo de execução quando você tenta adicionar o segundo elemento.

O TreeSet usará os hashCodes dos objetos para ordená-los.

Dica: O TreeSet tem uma única função: manter as coisas ordenadas. O que ele precisa para cumprir essa função?

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 3. Ocorre uma ClassCastException em tempo de execução quando você tenta adicionar o segundo elemento. 



<strong>Justificativa:</strong> Correto. O TreeSet tenta fazer um 'cast' do objeto para Comparable para poder chamar o método compareTo(). Como a classe não implementa a interface, essa conversão falha, resultando em uma exceção.
</details>

Pergunta 6: O capítulo explica que um método com um parâmetro List<Animal> não aceitará um argumento List<Dog>. Por que o compilador Java proíbe isso?

Porque um List<Dog> só pode conter objetos Dog, enquanto um List<Animal> pode conter qualquer animal.

Porque List<Dog> não é uma subclasse de List<Animal>.

Para evitar que o método adicione um objeto não-Dog (como um Cat) à lista que deveria conter apenas objetos Dog.

Porque métodos com parâmetros genéricos só podem aceitar o tipo exato declarado, sem polimorfismo.

Dica: O que aconteceria se o método que recebe a lista tentasse adicionar um new Cat() a ela?

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 3. Para evitar que o método adicione um objeto não-Dog (como um Cat) à lista que deveria conter apenas objetos Dog. 



<strong>Justificativa:</strong> Exatamente. Se fosse permitido, o método poderia tratar a List&lt;Dog&gt; como uma List&lt;Animal&gt; e adicionar um new Cat() a ela, quebrando a segurança de tipo da lista original.
</details>