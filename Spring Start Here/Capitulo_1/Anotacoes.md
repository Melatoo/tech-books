# Anotações

# Spring no mundo real

## O que são frameworks?

No começo da história do desenvolvimento de software, cada aplicação era feita do completo zero, os tornando únicos. Com o passar do tempo, foi possível perceber um certo padrão de funcionalidades que uma aplicação utilizava, como:

- Logar mensagens de erro/info/warn;
- Usar transações para mudanças em dados;
- Mecanismos de segurança comuns;
- Maneiras de comunicação de aplicação com aplicação;

entre outros.

Sendo assim, também perceberam que as regras de negócio se tornaram a menor parte do código, que era ocupado, em sua maior parte, por aquilo que podia se considerar o “motor” da aplicação.

Com isso, vieram os frameworks, que prometem diminuir substancialmente o gasto de tempo/esforço com requisitos genéricos e essenciais de uma aplicação, permitindo maior gasto nas regras de negócio.

## O que é o Spring

Normalmente se refere ao Spring como um Framework, mas é um pouco mais complexo que isso. Spring é um ecossistema de frameworks, dividido em: Spring Core, Spring MVC, Spring Data Access e Spring testing.

### Spring Core

O Spring core é a parte que provê os mecanismos fundamentais de integração em apps. O funcionamento do Spring se baseia na Inversion of Control (IoC). Ou seja, o controle da aplicação não é do próprio código, e sim do framework (Spring). Nós apenas dizemos para o Spring como o app deve ser controlado, daí vem a inversão.

### Spring Data Access

No Spring, é o Spring Data Access quem permite a persistência de dados. Ele inclui, através do uso do JDBC, a integração com frameworks ORM como o Hibernate e o gerenciamento de transações.

### Spring MVC

As aplicações que usam Spring normalmente são aplicativos web e, dentro do ecossistema Spring é possível encontrar diversas ferramentas para lidar com esses tipos de aplicação. Uma delas é o Spring MVC, que permite o desenvolvimento de apps web da maneira servlet padrão.

### Spring testing

O Spring testing oferece uma gama de ferramentas para escrever testes de unidade e testes de integração.

### Projetos do ecossistema Spring

O Spring também possui projetos independentes, como Spring Data, Spring Security, Spring Cloud, Spring Batch, Spring Boot, etc. Ao desenvolver um app, é possível utilizar mais de um projeto em conjunto. Alguns deles são:

- Spring Data: o Spring Data permite a conexão rápida com databases e o uso de uma camada de persistência com uma mínima quantidade de linhas escritas. O projeto se refere tanto a SQL quanto NoSQL.
- Spring Boot: o Spring Boot introduz o conceito de “convenção sobre configuração”. A ideia principal do projeto é oferecer configurações padrões do framework, permitindo a customização conforme necessário. Ou seja, ao invés de escrever todas as configurações de um app qualquer, é mais eficiente utilizar convenções famosas e já estabilizadas e apenas alterar o que for necessário.

## Spring no Mundo Real

Por mais que o Spring apareça majoritariamente para desenvolvimento de aplicativos back-end, ele também se faz presente em alguns outros tipos de desenvolvimento:

### Aplicação de testes automatizados

É possível que um aplicativo de testes utilize o Spring IoC Container para gerenciar componentes, bem como utilizar o Spring Data para conectar em databases ou simplesmente usar o Spring para chamadas de endpoints REST.

### Aplicativos de Desktop

Também é possível que um app para desktop use o Spring IoC Container para gerenciar suas instâncias de objetos, facilitando a implementação e manutenabilidade. Também é possível utilizar as ferramentas do Spring para outras soluções.

### Aplicativos Mobile

É raro, mas o spring-android provê um cliente REST para android, além de suporte de autenticação para acessar APIs seguras.

## Quando não utilizar um Framework

Em algumas situações específicas, pode ser que utilizar um framework pode não ser o cenário ideal, como por exemplo:

- Aplicativos leves: quando se há a necessidade de diminuir ao máximo o tamanho (pegada) de um app, adicionar dependências pesadas como frameworks é indesejado. Um caso seria funções server-less.
- Aplicativos de segurança extrema: quando se há a necessidade de fazer aplicações cuja segurança deve ser levada ao máximo, utilizar frameworks não é a opção ideal, já que podem possuir vulnerabilidades específicas que fogem do controle dos desenvolvedores.
- Custo-benefício baixo: quando já se possui um app que não foi feito utilizando um framework, pode ser que o gasto para utilizar um seja maior que os benefícios gerados.