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