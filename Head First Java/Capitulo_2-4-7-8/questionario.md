Pergunta 1: De acordo com o Capítulo 2, qual é a principal diferença entre uma classe e um objeto em Java?
Uma classe é um tipo de dado primitivo, enquanto um objeto é um tipo de dado de referência.

Uma classe é um modelo ou projeto para criar objetos, enquanto um objeto é uma instância de uma classe.

Uma classe executa o código no método main(), enquanto um objeto não pode.

Objetos herdam de classes, mas classes não podem herdar de objetos.

Dica: Pense na analogia de uma planta de uma casa e a própria casa.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 2. Uma classe é um modelo ou projeto para criar objetos, enquanto um objeto é uma instância de uma classe. 





<strong>Justificativa:</strong> Esta é a definição fundamental. A classe é o projeto , e os objetos são as instâncias reais criadas a partir desse projeto. 



</details>

Pergunta 2: O Capítulo 2 explica que um objeto tem estado e comportamento. O que representa o estado e o comportamento de um objeto, respectivamente?
Métodos e variáveis de instância.

Construtores e pacotes.

Variáveis de instância e métodos.

Variáveis locais e argumentos.

Dica: O que um objeto 'sabe' (dados) versus o que um objeto 'faz' (ações)?

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 3. Variáveis de instância e métodos. 




<strong>Justificativa:</strong> As variáveis de instância contêm os dados que definem o estado de um objeto , e os métodos definem as ações que ele pode realizar, ou seja, seu comportamento. 






</details>

Pergunta 3: Qual é o principal benefício da encapsulação, conforme descrito no Capítulo 4, ao marcar as variáveis de instância como private e fornecer métodos public (getters e setters)?
Garante que as variáveis de instância nunca possam ser alteradas após a criação do objeto.

Permite o controle sobre os dados, protegendo-os de alterações inválidas e ocultando a implementação interna.

Melhora o desempenho do programa ao permitir acesso direto à memória.

Torna as variáveis de instância acessíveis a qualquer outra classe no programa.

Dica: Pense em proteger os dados de um objeto contra 'maus usos'.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 2. Permite o controle sobre os dados, protegendo-os de alterações inválidas e ocultando a implementação interna. 



<strong>Justificativa:</strong> A encapsulação protege os dados , permitindo que a classe valide os valores através dos setters e mude sua implementação interna sem quebrar o código externo. 




</details>

Pergunta 4: O Capítulo 4 diferencia variáveis de instância e variáveis locais. Qual afirmação descreve corretamente as variáveis locais?
Elas são declaradas dentro de uma classe, mas fora de qualquer método, e vivem enquanto o objeto existir.

Elas devem sempre ser declaradas como public para serem usadas.

Elas recebem um valor padrão (0, false ou null) se não forem inicializadas explicitamente.

Elas são declaradas dentro de um método e só existem enquanto o método está em execução (na pilha).

Dica: Onde a variável é declarada e por quanto tempo ela 'vive'?

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 4. Elas são declaradas dentro de um método e só existem enquanto o método está em execução (na pilha). 


<strong>Justificativa:</strong> Variáveis locais são temporárias e seu escopo e tempo de vida estão confinados ao método em que são declaradas. 


</details>

Pergunta 5: De acordo com o Capítulo 9, se você não fornecer nenhum construtor para sua classe, o que o compilador Java faz?
Gera um erro de compilação, pois toda classe precisa de um construtor explícito.

Cria um construtor padrão, público e sem argumentos (no-arg).

Impede que a classe seja instanciada, tornando-a efetivamente abstrata.

Cria um construtor private para impedir a criação de objetos fora da classe.

Dica: O compilador tenta ajudar quando você esquece de algo essencial para a criação de objetos.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 2. Cria um construtor padrão, público e sem argumentos (

no-arg). 



<strong>Justificativa:</strong> Se nenhum construtor for definido, o compilador fornece um construtor padrão público e sem argumentos para garantir que a classe possa ser instanciada. 



</details>

Pergunta 6: O Capítulo 9 discute o ciclo de vida dos objetos. Um objeto torna-se elegível para a coleta de lixo (garbage collection) quando:
Seu método finalize() é chamado explicitamente pelo programa.

O objeto é explicitamente definido como null.

O objeto não pode mais ser alcançado por nenhuma referência 'viva' (ativa).

O objeto fica sem memória no heap.

Dica: Pense no que aconteceria se você perdesse o controle remoto de um objeto.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 3. O objeto não pode mais ser alcançado por nenhuma referência 'viva' (ativa). 





<strong>Justificativa:</strong> A elegibilidade para a coleta de lixo é determinada pela alcançabilidade. Se um objeto não pode ser acessado a partir de nenhuma parte do programa em execução, ele pode ser coletado. 



</details>

Pergunta 7: O Capítulo 8 introduz classes abstratas. Qual das seguintes afirmações sobre uma classe abstrata é VERDADEIRA?
Uma classe abstrata não pode ter um construtor.

Uma classe abstrata não pode ser instanciada diretamente usando a palavra-chave new.

Todos os métodos em uma classe abstrata devem ser abstratos.

Uma classe abstrata só pode ser estendida por uma outra classe abstrata.

Dica: O que a palavra 'abstrato' sugere sobre a capacidade de criar um objeto 'genérico'?

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 2. Uma classe abstrata não pode ser instanciada diretamente usando a palavra-chave 

new. 




<strong>Justificativa:</strong> Marcar uma classe como 

abstract impede que qualquer código crie uma instância dela diretamente.  Você só pode instanciar suas subclasses concretas.


</details>

Pergunta 8: De acordo com o Capítulo 8, o que uma classe deve fazer quando implementa uma interface?
Deve herdar os construtores da interface.

Deve implementar todos os métodos abstratos definidos na interface.

Pode escolher quais métodos da interface implementar.

Deve usar a palavra-chave extends em vez de implements.

Dica: Implementar uma interface é como assinar um contrato. O que o contrato exige?

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 2. Deve implementar todos os métodos abstratos definidos na interface. 



<strong>Justificativa:</strong> Uma classe que implementa uma interface assina um contrato para fornecer uma implementação (um corpo de método) para todos os métodos abstratos declarados nessa interface. 


</details>