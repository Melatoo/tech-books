# Capítulo 1: Trade-offs in Data Systems Architecture

O capítulo começa explicando que aplicações com uso intensivo de dados são aquelas cujo gerenciamento de dados são desafios importantes na construção do app. Nesse tipo de aplicação, nos preocupamos mais em como armazenar e processar os dados (consistência, falha, concorrência, disponibilidade).

Database -> onde armazenamos os dados
Cache -> memória de resultado de operações complexas
Índice -> encontrar dados comn base em palavras ou filtros
Stream processing -> lidar com eventos de alteração de dados assim que ocorrem
Processamento em lote -> dividir uma grande parte de dados acumulados

Também é explicado que o difícil de lidar com dados é que pessoas diferentes precisam lidar com eles de maneiras diferentes, e cada maneira tem um jeito "melhor" de lidar.

## Operational Versus Analytical Systems

Sistemas operacionais são aqueles ligados a serviços back-end e onde os dados são gerados.
Sistemas analíticos possuem cópias dos dados gerados pelo sistema operacional e são otimizados para análises.

Esses dois tipos de sistemas são frequentemente mantidos de maneira separada.

### Characterizing Transaction Processing and Analytics

Sistemas operacionais possuem sua leitura, escrita, criação e deleção com base na entrada do usuário. Por ser interativo, esse padrão de acesso com base no usuário se tornou conhecido por online transaction processing, ou OLTP.

Já sistemas analíticos precisam percorrer grandes quantidades de dados e realizar operações de agregação dentre eles, como soma, média, mediana, etc. Esse tipo de acesso se chama online analytical processing (OLAP).

Sistemas operacionais têm padrão individual para leituras e escritas, são de uso externo, possuem muitas querias normalmente simples e seus dados representam a verdade (isto é, o ponto atual no tempo).

Já sistemas analíticos possuem padrão em lote para leitura e escritas, são de uso interno para tomada de decisão ou detecção de padrões, possuem poucas querias complexas e seus dados são histórico de eventos que já aconteceram.

### Data warehouse

No passado, sistemas analíticos e operacionais atuavam encima do mesmo banco de dados, mas isso é contra-indicado por alguns motivos: os dados que interessam para análise podem estar dispersos, tornando difícil uma combinação entre eles (data silos), queries analíticas são custosas e podem impactar a performance de usuários e razões de compliance.

Para isso, foram criadas as data warehouses, onde os dados são armazenados especificamente para OLAP (online analytics processing) e suas operações não afetam o banco de dados do OLTP (online transactional processing). As data warehouses são constituídas de versões read-only de dados de sistemas OLTP e transformados em uma versão "analysis-friendly" para sistemas OLAP. Esse processo de construção de data warehouses são conhecidos por extract-transform-load (ETL). Em casos específicos, a transformação e carregamento podem ser invertidos, resultando em extract-load-transform (ELT).

Em alguns casos, os dados de data warehouses podem ser extraídos de sistemas externos, fazendo com que dados externos também possam ser analisados.

### Data Lake

Data lakes são um repositório de dados centralizados que possuem cópias de dados úteis para cientista de dados. A diferença para a Data Warehouse é que nos Data Lakes os dados não armazenados sem formato, modelo ou schema. São comumente utilizados como intermediários às Data Warehouses, como uma fonte de dados crus que podem ser trasnformados confome necessidade.

### Data as Streams

Dados para analytics também podem se tornar streams de eventos. Enquanto dados armazenados em imagem, textos, etc podem ser processados diariamente, por exemplo, dados em Streams permitem respostas imediatas à um evento, como uma fraude ou tentativa de quebra de segurança.

### Systems of Record

System of record armazenam a fonte da verdade dos dados. Quando um novo dado chega, ele é armazenado aqui primeiro e cada fato, normalmente normalizado, é representado apenas uma única vez.

### Derivered data systems

Dados de um Derivered data system são derivados de outros dados existentes, onde já foram aplicados alguma transformação. Se esses dados derivados forem perdidos, eles podem ser recriados através da fonte original. Um exemplo é a cache, quando estão presentes, os dados podem ser entregues por eles, mas caso não existam, basta consultar o banco de dados original. Outros exemplos são: índices, valores desnormalizados, views materializadas.

Esses dados podem ser reduntantes, mas são principalmente úteis em consultas de leitura. Na SYDLE, o system of record é o MongoDB e o Derivered é o ElasticSearch.

Quando esses sistemas são utilizados em conjunto, há a necessidade de atualizar o sistema derivado toda vez que o sistema de registro é atualizado, o ElasticSearch faz isso com suas indexações que custam ~1s.

## Cloud vs Self-Hosting

Uma regra clara para quando escolher entre comprar ou construir é: caso traga vantagem competitiva à empresa, deve-se desenvolver internamente.


### Pros and Cons of Cloud Services

A principal vantagem de utilizar serviços de núvem é a praticidade. Quando não se sabe como implantar nem operar, é mais fácil contratar um serviço de núvem do que treinar ou contratar um time.

Além disso, esses serviços são particularmente úteis quando se há a necessidade de elasticidade no serviço, ou seja, em um momento precisa-se de muito poder computacional, mas na maior parte do tempo o sistema fica ocioso. Nesses casos, uma máquina seria pouco eficiente, mas serviços de núvem podem alocar e desalocar os recursos conforme a necessidade.

Entretanto, para um sistema cuja carga é previsível ou que demanda personalização de configuração mais aprofundada para melhor desempenho, pode ser melhor executar esse serviço internamente.

A maior desvantagem de serviços em núvem é a falta de controle: depende-se do fornecedor para criação de features, resolução de bugs, resolução em casos de indisponibilidade, além disso, conflito político entre o seu país e o país do fornecedor pode acarretar perda de dados.

### Cloud-native

A arquitetura cloud-native é projetada para tomar vantagem de sistemas cloud. Alguns exemplos são: AWS Aurora, Google BigQuery, DynamoDB, etc.

### Layering of cloud services

A ideia principal de serviços cloud-native não é apenas o gerenciamento de hardware (CPU, RAM, GPU, etc), mas também a construção de outros serviços de nível mais alto com base em serviços de nível mais baixo. Por exemplo: serviços de gerenciamento de arquivos, como S3, são mais limitados quando comparado à um sistema de arquivo comum, mas possuem a vantagem de dispersar os dados em várias máquinas. Outros serviços são construídos encima de serviços de gerenciamento de arquivos em núvem, como o Snowflake, uma data warehouse em núvem que depende do S3 para armazenar dados.

Serviços de nível mais alto são utilizados em casos específicos. Entretanto, quando se vê em um caso que é contemplado por um serviço de nível mais alto, normalmente é mais fácil contratá-lo do que construir por si mesmo.

### Separation of storage and compute

Na computação tradicional, temos o RAID (redundant array of independent disks), que é utilizado normalmente para se manter o mesmo arquivo em diversos discos conectados à mesma máquina.

Essa técninca também pode ser utilizada por sistemas em núvem. No entando, sem sistemas cloud-native, os discos são tratados mais como um cache, isso pois um disco local pode ser inutilizado caso uma instância falhe ou seja substituída. Como alternativa à discos físicos, foi criado o conceito de disco virtual, que podem ser acoplados e desacoplados de qualquer máquina. Entretanto, esse conceito é suscetível à problemas de rede, já que todo I/O é uma chamada de rede, e acarreta problemas que não acontecem em serviços cloud-nativa, como sobrecarga. Por esses problemas, sistemas de núvem evitam utilizar discos virtuais.

Em uma arquitetura tradicional, o sistema de arquivos e a computação são gerenciados pelo mesmo computador, diferente de sistemas núvens que possuem serviços específicos para cada um, em máquinas separadas. Além disso, esses sistemas são multitenant, ou seja, múltiplos clientes participam de um mesmo hardware compartilhado ao invés de terem sua máquina individual. Isso permite um escalonamento mais fácil de hardware.

### Operations in the Cloud Era

Hoje em dia, responsabilidades de operations estão dispersas entre serviços backend e infraestrutura. A filosofia DevOps tem orientado essa tendência. Também estamos trocando gastos fixos (espaço determinado em um disco) por gastos métricos (quanto de espaço estamos realmente ocupando). 

Essa filosofia enfatiza alguns pontos: transformar processos repetitivos em automações, utilizar serviçõs efêmeros (VMs) ao invés de serviços de longa-duração, permitir atualizações frequentes das aplicações, aprender com incidentes e preservar conhecimento.

Além disso, a responsabilidade da operação foi bifurcada entre garantir disponibilidade ao cliente final e diminuir os custos com infraestrutura. Por mais que adotar um serviço de núvem tenha diminuído o esforço manual, ainda é necessário entender todos os serviços para implantá-los, contornar possíveis faltas de features e garantir a segurança da aplicação em pontos que o provedor não garante.

## Distributed vs Single-node Systems

Sistemas dispersos em mais de uma máquina que se comunicam por rede são chamados de distribuídos. Cada processo desse sistema é chamado de nó. Usamos esse tipo de sistema por alguns motivos:

Distribuição inerente: Se a aplicação tiver mais de um usuário interagindo entre si, com seu próprio dispositivo, o sistema é inevitavelmente distribuído.

Solicitações entre serviços de núvem: Se os dados são armazenados em um local e processados em outro, precisam ser comunicados através de rede. Logo, arquiteturas cloud-native e microsserviços são distribuídos.

Tolerância à falhas: Se a aplicação precisa ter uma alta disponibilidade, podemos dispersar esse sistema em mais de uma máquina para fornecer redundância.

Escalabilidade: Se o volume de dados ou computação superam o que uma máquina individual oferece, podemos utilizar outra e distribuir a carga entre elas.

Latência: Se houver clientes espalhados pelo mundo, podemos distribuir o sistema em espaços geográficos específicos mais próximos dos clientes, assim eliminando a necessidade de esperar o dado percorrer grandes distâncias geográficas para comunicar com o cliente final.

Elasticidade: Se a aplicação tem picos de atividade e ociosidade, podemos utilizar os serviços de núvem para aumentar ou reduzir a escala.

Hardware especializado: Diferentes partes do sistema podem requerer um uso maior de partes específicos da máquina, como memória RAM, GPU, CPU.

Conformidade Legal: Alguns países podem requerer que alguns tipos de dados pessoais sejam armazenados dentro do seu território.

Sustentabilidade: Pode ser possível executar as cargas de trabalho em locais com energia renovável, diminuindo a emissão de carbono.

### Problems with distributed systems

Problemas de rede e performance: as comunicações entre sistemas distribuídos são necessariamente por rede, que podem falhar, serviço pode estar sobrecarregado, expirar, etc, e essa comunicação acaba perdida sem sabermos o que foi realizado. Além disso, por mais que as redes de serviços de núvem sejam muito rápidas, nada é mais rápido que chamar uma função do próprio processo. Por vezes, sistemas single-threaded é significativamente mais rápido que um cluster com 100 núcleos de CPU.

Diagnóstico de problemas: como descobrir onde está um problema se o sistema está disperso em várias máquinas? Para isso, foi criado o conceito de observabilidade, que envolve a coleta e uso de dados sobre a execução de um sistema.

Consistência de dados: por mais que bancos de dados disponibilizem maneiras de garantir a consistência de dados, quando cada serviço possui seu próprio banco de dados isso passa a ser um problema da aplicação.

Por esses motivos, executar um processo em um único nó costuma ser mais barato do que em sistemas distribuídos.

### Microservices and Serverless

A maneira mais comum de dividir serviços entre máquinas é utilizando o conceito de cliente e servidor, e deixar os clientes fazerem requisições aos servidores. O mesmo serviço pode ser tanto cliente quanto servidor.

Essa maneira de criar aplicativos é chamada de service-orientede architecture (SOA), recentemente foi refinada à microservices architecture. Nesse tipo de arquitetura, cada microsserviço tem um propósito bem-definido, expõe APIs para comunicação com clientes e é mantido por um time.

Dividir os serviçoes pode ter algumas vantagens: cada serviço pode ser atualizado independentemente, os recursos podem ser alocados de maneira específica para aquele serviço e manter o código escondido por uma API significa alterar a implementação sem afetar clientes. Normalmente os bancos de dados de cada serviço é separado entre si.

Em outro sentido, pode também trazer algumas disvantagens: testar um serviço pode ser complicado, já que depende de outro serviço. Além disso, o que antes era configurável uma vez por aplicação, se repete em cada serviço, como alocar recursos, coletar logs, monitorar processos, etc. Por fim, APIs de microsserviços podem ser difíceis de serem evoluídas, já que o cliente espera campos específicos nessa API, e mudanças que causam falhas não discobertas provavelmente só vão ser encontratas já direto em produção.

Microsserviço são, fundamentalmente, a solução para um problema relacionado à pessoas, para que times diferentes possam trabalhar independentemente entre si.

### Serverless

Serverless, ou function as a service (FaaS), é uma maneira de implantar serviços terceirizando a sua configuração para o provedor da núvem.

Em um serverless, o provedor decide quando inicializar, quanto recurso alocar e quando desligar uma instância. Dessa maneira, você paga apenas pelo tempo em que sua aplicação está de fato rodando.

Desse modo, provedores de FaaS costumam ter tempo limite de execução de funções, restringem os ambientes de execução e tempos para iniciar uma instância quando a função é chamada pode ser mais demorado.

Por mais que o nome diga o contrário, serverless roda sim em um servidor, mas cada execução pode acontecer em um servidor diferente.

### Supercomputação

A computação de alto desempenho pode ser uma alternativa à computação em núvem para sistemas em larga escala. 

A supercomputação tem algumas características que diferem da computação em núvem: são normalmente utilizadas em tarefas científicas intensivas. Normalmente executados em lote, que salvam checkpoints no disco. Nós se comunicam por memória compartilhada ou RDMA, possuindo baixa latência mas também baixa confiabilidade e comumente são distribuídos muito próximos uns aos outros.

