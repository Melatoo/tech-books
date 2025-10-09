Pergunta 1: De acordo com o Capítulo 3, o que uma variável de referência a um objeto realmente contém?

O próprio objeto, com todas as suas variáveis de instância e métodos.

Um valor primitivo que representa o tamanho do objeto na memória.

Um valor que representa uma forma de acessar um objeto no heap.

O código-fonte da classe do objeto.

Dica: Pense na variável como um controle remoto e no objeto como a TV.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 3. Um valor que representa uma forma de acessar um objeto no heap. 






<strong>Justificativa:</strong> Correto. Uma variável de referência funciona como um 'controle remoto' ou um ponteiro para o objeto real, que reside na memória heap.
</details>




Pergunta 2: No Capítulo 3, ao criar um array de objetos (por exemplo, Dog[] myDogs = new Dog[3];), o que é imediatamente criado na memória heap?

Um objeto array e três objetos Dog.

Apenas três objetos Dog.

Um objeto array contendo três referências null.

Três variáveis de referência na pilha, mas nada no heap.

Dica: A criação de um 'recipiente' é diferente da criação do 'conteúdo'.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 3. Um objeto array contendo três referências null. 


<strong>Justificativa:</strong> A instrução new Dog[3] cria um objeto array, cujos elementos são referências. Como nenhum objeto Dog foi atribuído ainda, essas referências são inicializadas como null.
</details>



Pergunta 3: Qual das seguintes atribuições de variáveis primitivas resultaria em um erro de compilação, de acordo com as regras do Capítulo 3?

int x = 10; byte b = x;

byte b = 10; int x = b;

long y = 100;

float f = 2.5f;

Dica: O compilador Java é muito cauteloso em relação à perda de precisão ao mover valores de 'copos grandes' para 'copos pequenos'.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 1. int x = 10; byte b = x; 


<strong>Justificativa:</strong> Isso causa um erro porque o compilador não pode garantir que o valor de um int caberá em um byte sem perda de informação, mesmo que o valor atual (10) seja pequeno o suficiente.
</details>

Pergunta 4: No Capítulo 3, o que acontece com um objeto no heap quando a única variável de referência que aponta para ele sai de escopo?

O objeto é imediatamente destruído e sua memória é liberada.

O objeto se torna elegível para a coleta de lixo (garbage collection).

O objeto é movido da heap para a pilha para ser destruído mais rapidamente.

O objeto permanece no heap para sempre, causando um vazamento de memória.

Dica: A perda da última referência não significa destruição instantânea, mas sim uma mudança de status.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 2. O objeto se torna elegível para a coleta de lixo (garbage collection). 






<strong>Justificativa:</strong> Quando um objeto se torna 'inalcançável' (nenhuma referência aponta para ele), ele é marcado como elegível para ser removido da memória pelo coletor de lixo em um momento futuro.
</details>




Pergunta 5: De acordo com o Capítulo 9, qual é a diferença fundamental entre onde as variáveis de instância e as variáveis locais vivem na memória?

Ambas vivem no heap, dentro do objeto.

Variáveis de instância vivem na pilha (stack) e variáveis locais vivem no heap.

Ambas vivem na pilha (stack), mas em frames diferentes.

Variáveis de instância vivem no heap (dentro do objeto) e variáveis locais vivem na pilha (stack).

Dica: O estado de um objeto precisa durar tanto quanto o objeto, mas as variáveis de um método são temporárias.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 4. Variáveis de instância vivem no heap (dentro do objeto) e variáveis locais vivem na pilha (stack). 



<strong>Justificativa:</strong> Correto. O estado de um objeto (variáveis de instância) reside com o objeto no heap, enquanto as variáveis temporárias de um método (variáveis locais) residem na pilha.
</details>

Pergunta 6: No Capítulo 9, se uma classe tem um construtor que aceita um argumento (ex: public MinhaClasse(int x)), e você não escreve um construtor sem argumentos, o que acontece se você tentar instanciar a classe com new MinhaClasse()?

O programa compila e o construtor com argumento é chamado com um valor padrão para x.

O programa não compila, porque o compilador não encontra um construtor correspondente.

O programa compila, e o compilador cria automaticamente um construtor sem argumentos para você.

Ocorre uma exceção em tempo de execução (RuntimeException).

Dica: O compilador só ajuda a criar um construtor se você não criar nenhum.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 2. O programa não compila, porque o compilador não encontra um construtor correspondente. 


<strong>Justificativa:</strong> Uma vez que você define qualquer construtor, o compilador não fornece mais o construtor padrão. A chamada new MinhaClasse() procura por um construtor sem argumentos, que não existe.
</details>

Pergunta 7: Qual é a primeira instrução que é chamada implicitamente dentro de qualquer construtor, conforme explicado no Capítulo 9?

Uma chamada ao construtor da classe Object.

Uma chamada ao construtor sem argumentos da superclasse (super()).

Uma chamada para inicializar todas as variáveis de instância com valores padrão.

Uma chamada a outro construtor sobrecarregado na mesma classe (this()).

Dica: Um objeto de uma subclasse não pode existir completamente até que sua parte da superclasse seja construída.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 2. Uma chamada ao construtor sem argumentos da superclasse (super()). 



<strong>Justificativa:</strong> Todo construtor, por padrão, invoca o construtor sem argumentos de sua superclasse como sua primeira ação, a menos que outra chamada (this() ou uma chamada super() diferente) seja feita explicitamente.
</details>



Pergunta 8: O Capítulo 9 descreve a 'cadeia de construtores' (constructor chaining). Se uma classe Hippo estende Animal, e Animal estende Object, em que ordem os construtores são concluídos quando você cria um new Hippo()?

Hippo, depois Animal, depois Object.

Object, depois Hippo, depois Animal.

Os construtores são executados simultaneamente.

Object, depois Animal, depois Hippo.

Dica: Pense em uma pilha: o primeiro a entrar é o último a sair, mas a execução de um construtor só termina depois que seu superconstrutor termina.

<details>
<summary>Ver Resposta</summary>
<strong>Resposta Correta:</strong> 4. Object, depois Animal, depois Hippo. 


<strong>Justificativa:</strong> A chamada sobe na hierarquia até Object(), que é o primeiro a ser concluído. Em seguida, a execução retorna para Animal(), que é concluído, e finalmente, o construtor de Hippo() é concluído.
</details>