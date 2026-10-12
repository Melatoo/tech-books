# Usando *data sources* em aplicativos Spring

## O que é um *data source*

O *data source* é um objeto cuja responsabilidade é fazer conexões com o servidor do banco de dados. Ele garante que seu aplicativo faça requisições de conexão com o banco de dados de maneira eficiente, aumentando a eficiência da camada de persistência.

Para isso, o *data source* utiliza o JDBC (Java Database Connectivity), cuja responsabilidade é apenas se conectar com o servidor do banco de dados. Entretanto, o JDBC não provê implementações específicas para cada gerenciador de banco de dados, para isso é necessário um JDBC driver. É responsabilidade do gerenciador de banco de dados prover um JDBC driver, sendo assim uma dependência externa.

Existem múltiplas alternativas para implementações de *data sources*, mas a mais utilizada e recomendada pelo Spring é o HikariCP (Hikari connection pool).