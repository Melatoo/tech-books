Questionário de Java: Lambdas e Streams
Pergunta 1: De acordo com o Capítulo 12, qual é a principal filosofia por trás da API de Streams em Java?

Dizer ao computador exatamente como executar uma tarefa, passo a passo, usando loops for.

Dizer ao computador o que você quer que seja feito, em vez de como fazê-lo, delegando a iteração para a biblioteca.

Substituir todas as coleções, como ArrayList, por uma estrutura de dados mais eficiente chamada Stream.

Garantir que todas as operações em uma coleção sejam executadas em threads paralelas para melhor desempenho.

Dica: Pense na diferença entre dar a alguém um mapa (como fazer) e apenas dizer o destino (o que você quer).

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 2. Dizer ao computador o que você quer que seja feito, em vez de como fazê-lo, delegando a iteração para a biblioteca. 



<strong>Justificativa:</strong> Correto. A API de Streams promove um estilo declarativo, onde você descreve o resultado ('o que'), e a API cuida da implementação da iteração e do processamento ('o como').
</details>

Pergunta 2: Uma 'pipeline de stream' (stream pipeline) consiste em três partes. Qual é a ordem correta dessas partes?

Operação terminal, fonte, operação intermediária.

Operação intermediária, fonte, operação terminal.

Fonte, uma ou mais operações intermediárias, uma operação terminal.

Fonte, uma operação terminal, uma ou mais operações intermediárias.

Dica: Pense nisso como uma linha de montagem: de onde vem a matéria-prima, o que acontece no meio e o que sai no final?

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 3. Fonte, uma ou mais operações intermediárias, uma operação terminal. 



<strong>Justificativa:</strong> Correto. Você sempre começa com uma fonte (ex: uma coleção), aplica uma série de transformações ou filtros (operações intermediárias) e, finalmente, executa uma operação terminal para obter um resultado.
</details>

Pergunta 3: Qual das seguintes opções descreve melhor a diferença entre operações intermediárias e operações terminais em um Stream?

Operações intermediárias alteram a coleção original, enquanto operações terminais não.

Operações intermediárias são 'preguiçosas' (lazy) e retornam um novo stream, enquanto operações terminais são 'ansiosas' (eager) e produzem um resultado final.

Operações intermediárias só podem ser usadas uma vez em uma pipeline, enquanto operações terminais podem ser usadas várias vezes.

Operações intermediárias são para ordenação e filtragem, enquanto operações terminais são apenas para coletar resultados.

Dica: Pense nas operações intermediárias como planejar uma receita e na operação terminal como o ato de cozinhar.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 2. Operações intermediárias são 'preguiçosas' (lazy) e retornam um novo stream, enquanto operações terminais são 'ansiosas' (eager) e produzem um resultado final. 



<strong>Justificativa:</strong> Correto. As operações intermediárias apenas definem os passos a serem executados, mas não fazem nada até que uma operação terminal seja chamada para iniciar o processamento e gerar o resultado final.
</details>

Pergunta 4: Qual operação intermediária você usaria para transformar cada elemento em um Stream de um tipo para outro (por exemplo, de um Stream<Song> para um Stream<String> contendo apenas os títulos das músicas)?

filter()

distinct()

collect()

map()

Dica: Qual operação funciona como um 'transformador' ou 'conversor' para cada item na linha de montagem?

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 4. map() 



<strong>Justificativa:</strong> Correto. A operação map() é projetada para aplicar uma função a cada elemento do stream, 'mapeando' cada elemento para um novo valor, que pode ser de um tipo diferente.
</details>

Pergunta 5: Uma expressão lambda, como song -> song.getGenre().equals("Rock"), passada para um método filter(), é uma implementação de qual tipo de interface?

Uma interface Comparator, porque está comparando algo.

Uma interface funcional (SAM) que possui um método que recebe um objeto e retorna um boolean (como Predicate).

Uma classe ActionListener, porque está respondendo a uma ação.

Uma interface Function, porque está aplicando uma função ao objeto.

Dica: O método filter precisa tomar uma decisão de 'sim' ou 'não' para cada elemento.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 2. Uma interface funcional (SAM) que possui um método que recebe um objeto e retorna um boolean (como Predicate). 



<strong>Justificativa:</strong> Correto. O método filter() espera um Predicate, que tem um único método abstrato test() que retorna true ou false, determinando se o elemento deve ser mantido no stream.
</details>

Pergunta 6: O que a sintaxe Song::getGenre representa no contexto da API de Streams?

Uma chamada direta ao método getGenre() em um objeto Song.

Uma referência de método (method reference), que é uma forma compacta de uma expressão lambda que apenas chama um método existente.

Um construtor da classe Song que aceita um gênero como argumento.

Uma variável estática chamada getGenre na classe Song.

Dica: É uma forma de 'apontar' para um método sem realmente chamá-lo naquele momento.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 2. Uma referência de método (method reference), que é uma forma compacta de uma expressão lambda que apenas chama um método existente. 



<strong>Justificativa:</strong> Correto. Song::getGenre é uma referência a um método que pode ser usada no lugar de uma lambda como song -&gt; song.getGenre(), tornando o código mais conciso.
</details>