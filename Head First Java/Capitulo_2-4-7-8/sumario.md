# Sumário — Capítulos 2, 4, 7 e 8

## Capítulo 2 — Um Passeio por Objectville (Classes e Objetos)
Introduz os conceitos fundamentais de classes e objetos.

- Classes vs. Objetos  
  - Classe: projeto/modelo para criar objetos.  
  - Objeto: instância concreta de uma classe.

- Estado e comportamento  
  - Estado: variáveis de instância (o que o objeto "sabe").  
  - Comportamento: métodos (o que o objeto "faz").

- Criação e uso  
  - Criação: `new`.  
  - Acesso a membros: operador ponto `.` em uma variável de referência.

## Capítulo 4 — Como os Objetos se Comportam
Aprofunda a relação entre estado e comportamento.

- Métodos usam o estado  
  - O comportamento de métodos depende das variáveis de instância (ex.: `bark()` pode variar com `size`).

- Parâmetros e retornos  
  - Métodos recebem parâmetros e retornam valores.  
  - Java usa passagem por valor — cópia do valor (primitivos) ou da referência (objetos).

- Encapsulamento  
  - Proteger dados marcando variáveis como `private`.  
  - Fornecer acesso controlado via getters e setters.

## Capítulo 7 — Vivendo Melhor em Objectville (Herança e Polimorfismo)
Explora herança como pilar da OO.

- Herança (`extends`)  
  - Subclasses herdam membros da superclasse, promovendo reuso e hierarquia.

- Sobrescrita de métodos (override)  
  - Subclasse pode fornecer implementação específica; a versão mais específica é executada.

- Relação "É-UM" (IS-A)  
  - Use o teste "É-UM" para validar a hierarquia — funciona apenas em uma direção.

## Capítulo 8 — Polimorfismo a Sério (Classes Abstratas e Interfaces)
Expande polimorfismo com ferramentas avançadas.

- Classes abstratas  
  - `abstract` não pode ser instanciada; pode conter métodos concretos e abstratos.  
  - Subclasse concreta deve implementar métodos abstratos.

- Interfaces  
  - Contrato semelhante a uma classe 100% abstrata.  
  - Use `implements`; a classe deve implementar todos os métodos (a não ser que seja `abstract`).

- Polimorfismo com interfaces  
  - Interfaces permitem múltipla abstração sem herança múltipla, possibilitando tratar objetos polimorficamente por um tipo comum.