# Pilares do Java

## Encapsulamento:

Em programação orientada a objetos, o ato de separar o código em partes, se mantendo o menos acoplado possível, é chamado de encapsular. A ideia é fazer o software flexível, onde cada parte depende o menos possível uma das outras. Daí nasce também o conceito de Encapsulamento, que é gerenciar o acesso à uma classe e os seus métodos e atributos. Dentro desse conceito, temos alguns modificadores de acesso diferentes.

### Tipos de Níveis de Acesso:

**Public**: acessível por todas as classes e subclasses, independente do pacote;

**Protected:** acessível apenas pelas classes e subclasses do próprio pacote. Cabe também dizer que não torna acessível para pacotes de uma subclasse que seja diferente da classe mãe;

**Default** **(Package-Private)**: acessível apenas pelas classes do próprio pacote;

**Private:** acessível apenas pela própria classe.

Como visamos sempre manter o menor nível de acoplamento possível, é comum que grande parte dos métodos e atributos de uma classe sejam Private. Sendo assim, é normal a implementação de métodos Get e Set, o nome desses métodos são bem sugestivos, já que suas responsabilidades são, respectivamente, obter e definir atributos de uma classe. Também possível definir regras de negócio e validações nesse método.

## Herança:

Herança é um mecanismo de POO que permite criar classes com base em classes já existentes, diminuindo a redundância. A classe já existente é chamada de superclasse, já a classe derivada é chamada de subclasse.
Essas classes criadas a partir de outras além de herdarem todas as características de seus supertipos, também podem adicionar mais características, seja na forma de variáveis e/ou métodos adicionais, bem como reescrever métodos já existentes na superclasse, o que chamamos de polimorfismo.

Ao definirmos uma herança, caso o construtor seja definido pelo programador, é necessário que a definição do construtor da superclasse seja a primeira declaração do construtor da subclasse, através da chamada de super().

### **O Problema da Herança Múltipla e a Solução com Interfaces**

O Java não permite herança múltipla (uma subclasse herdar de mais de uma superclasse). Isso acontece para evitar ambiguidades, como o "Deadly Diamond of Death", onde o compilador não saberia qual método de superclasse executar se ambos tivessem a mesma assinatura.
Para contornar esse problema, o Java utiliza interfaces. Uma classe pode estender (`extends`) apenas uma superclasse, mas pode implementar (`implements`) múltiplas interfaces, permitindo que um objeto "desempenhe múltiplos papéis".

### Classes Abstratas e Interfaces

**Classes Abstratas**: São classes que não podem ser instanciadas. Elas servem como um "modelo" para suas subclasses. Uma classe deve ser declarada `abstract` se possuir pelo menos um método abstrato. Métodos abstratos não têm corpo (`{}`) e devem ser implementados pela primeira subclasse concreta na hierarquia.

**Interfaces**: São como classes 100% abstratas. Todos os seus métodos são implicitamente `public` e `abstract`. Uma classe não estende (`extends`) uma interface, ela a implementa (`implements`). Interfaces são a chave para um polimorfismo mais flexível, pois classes de diferentes árvores de herança podem implementar a mesma interface.

### A Superclasse `Object`

Toda classe do Java é uma subclasse da superclasse Object. Sendo assim, todas as classes do Java herdam alguns métodos diretamente na sua criação, métodos da classe Object, como `toString()`, `equals()`, `hashCode()`, etc.
Essa superclasse possui dois principais propósitos: agir como um tipo polimórfico para métodos que precisam funcionar em classes que qualquer um faça e provisionar código de métodos reais que o próprio Java precisa em tempo de execução.

### Execução de Métodos da Superclasse

É possível invocar um método pela subclasse da maneira como ele é implementado na superclasse. Para isso, usamos a keyword super, como:

```java
abstract class Report {
	void runReport() {
		// Faz algo
	}
}

class NationalJournal extends Report {
	void runReport() {
		super.runReport();
		// Faz algo mais
 }
}
```

## Polimorfismo:

Em POO, polimorfismo permite que objetos de diferentes classes respondam à mesma mensagem (chamada de método) de maneiras específicas para cada classe. Isso geralmente é alcançado através da Herança e Interfaces.

### **O Poder e o "Preço" do Polimorfismo**

O verdadeiro poder do polimorfismo está em usar uma referência de uma superclasse para apontar para um objeto de uma subclasse.

**Poder:** Isso permite criar **argumentos e arrays polimórficos**. Um método que aceita `Animal` pode receber um `Dog`, e um array `Animal[]` pode conter `Dog`s e `Cat`s.
**O "Preço":** Ao usar uma referência de uma superclasse (como `Object` ou `Animal`), o compilador só permite chamar os métodos que existem na classe de referência. Mesmo que a referência `animal` aponte para um objeto `Dog`, você não pode chamar `animal.latir()`, pois o método `latir()` não existe na classe `Animal`.

### **Casting e `instanceof`**

Para resolver a limitação acima, você pode "recuperar" o tipo completo do objeto:

**Casting:** Você pode forçar o compilador a tratar a referência como seu tipo real através de um cast. Isso permite o acesso a todos os seus métodos.

```java
// animalRef aponta para um objeto Dog
Dog realDog = (Dog) animalRef;
realDog.latir(); // Funciona
```

**Segurança com `instanceof`:** Fazer um cast é arriscado, se o objeto não for do tipo esperado, ocorrerá uma `ClassCastException`. Para evitar isso, usamos o operador `instanceof` para verificar o tipo do objeto antes de fazer o cast.

```java
if (animalRef instanceof Dog) {
  Dog realDog = (Dog) animalRef;
}
```

### Formas de Polimorfismo em Java

Em Java, o polimorfismo se manifesta, principalmente, de duas formas:

**Overriding:** Ocorre quando uma subclasse fornece uma implementação específica para um método que já é definido em sua superclasse, através da anotação `@Override`. Métodos overridden devem ter o mesmo nível de acesso, ou níveis mais “amigáveis”, e a mesma assinatura que o método implementado na superclasse.

**Overloading:** Ocorre quando temos vários métodos com o mesmo nome, mas com parâmetros diferentes (seja na quantidade ou no tipo) dentro da mesma classe. Não é permitido que apenas o tipo de retorno do método overloaded seja trocado, mesmo que seja uma subclasse da classe retornada no método original. **É necessário que a lista de argumentos seja trocada para que o compilador defina que é um Overload** e não uma tentativa de Override.

Existem discussões se o Overloading é considerado polimorfismo ou não, pra mim isso é irrelevante, afinal, polimorfismo é um conceito abstrato e não algo exato. Sendo assim, vale a pena uma leve referência para os tipos de polimorfismo: 

**Polimorfismo estático:** Esse tipo de polimorfismo é resolvido em tempo de compilação, ou seja, quando o método é chamado, a JVM não precisa decidir qual de suas versões deve ser executada, isso já foi definido pelo próprio compilador.

**Polimorfismo dinâmico:** Esse tipo de polimorfismo é resolvido durante o tempo de execução, ou seja, quando um método é chamado a própria JVM decide qual de suas versões devem ser executadas.