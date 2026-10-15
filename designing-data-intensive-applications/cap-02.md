# Defining Nonfunctional Requirements

### Case Study

#### Functional Requirementes

Quando definimos requisitos de telas ou operações que uma aplicação deve ter, estamos definindo requisitos funcionais.

#### Nonfunctional Requirements

Quando definimos quando o app deve estar disponível, perfomance, tempo de resposta, etc, estamos definindo requisitos não-funcionais.

### Describing Performance

#### Response time

Tempo que passou entre o momento que o usuário fez a requisição e quando recebeu a resposta. Tempo de medida é segundos.

#### Throughput

Quantidade de requisições por segundo. Tempo de medida é algo por segundo.

#### Retry Storm

Quando um sistema está sobrecarregado de requisições, fazendo que o tempo de resposta seja muito alto e expire, esse cliente pode fazer a requisição novamente e aumentar ainda mais o número de requisições.

Um sistema é escalável se o throughput máximo aumenta se aumentarmos o hardware

### Latency and Response time

O tempo de resposta é o que o cliente vê, inclui todo o delay do sistema.

Tempo de serviço é o tempo que um serviço está processando uma requisição.

Delay de fila é o tempo que ficamos ociosos esperando alguma coisa, como um CPU liberar.

Lateência é o termo utilizado quando o sistema não está ativamente processando algo, como o tempo de uma resposta trafegar a internet.