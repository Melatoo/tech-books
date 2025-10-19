# Sumário — Capítulos 3 e 9

## Capítulo 3 — Conheça Suas Variáveis
- Tipos de variáveis
  - Primitivas: armazenam o valor (ex.: int, boolean).
  - Referência: armazenam o endereço de um objeto no heap.
- Variáveis de referência
  - Funcionam como um "controle remoto" para um objeto; não contêm o objeto em si.
- Arrays
  - São sempre objetos; new Dog[5] cria um array de referências inicializadas como null (não cria os objetos Dog).

## Capítulo 9 — Vida e Morte de um Objeto
- Memória: Stack vs Heap
  - Stack: chamadas de método e variáveis locais (vida curta).
  - Heap: área onde objetos são criados e residem.
- Variáveis de instância vs locais
  - Instância: declarada na classe, vive no objeto no heap.
  - Local: declarada no método, vive na stack enquanto o método executa.
- Construtores
  - Executados ao criar um objeto com new; o compilador fornece um construtor padrão se não houver.
  - Podem ser sobrecarregados (assinaturas diferentes).
- Cadeia de construtores
  - Cada construtor chama o da superclasse (super()) como primeira instrução — até Object.
- Coleta de lixo (Garbage Collection)
  - Um objeto vira elegível para coleta quando não é mais alcançável por referências ativas (referência sai de escopo, é reatribuída ou definida como null).