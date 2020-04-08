# Tipos de Valor e Tipos de Referência



O JS possui __5__ tipos de dados **passados por valor**: ```Boolean, null, undefined, String e Number```. Podemos chamá-los de **tipos primitivos**.
Também possui __3__ tipos de dados **passados por referência**: ```Array, Function e Object```. Todos esses são tecnicamente objetos, então nos referiremos a eles como **Objetos**.

## Primitivos

Se um tipo primitivo é atribuído a uma variável, podemos pensar que essa variável está guardando o valor primitivo.

```js
var x = 10;
var y = "abc";
var z = null;
```

Quando nós atribuímos essas variáveis a outras utilizando ```=```, nos copiamos o valor para a nova variável. São **cópias pelo valor**.

```js
var x = 10;
var y = "abc";
var a = x;
var b = y;
console.log(x, y, a, b); // -> 10, 'abc', 10, 'abc'
```

Agora ```a``` e ```x``` contém o valor **10**. ```b``` e ```y``` o valor **'abc'**, separadamente, pois os próprios valores foram copiados. Alterar uma não altera a outra. Pense como se as variáveis não possuem **"relacionamento"** com a outra, a qual copiou o valor, e vice-versa.

## Objetos

Às variáveis que são atribuídas um valor não primitivo **recebem a referência a esse valor**.  Essa referência aponta para a localização do objeto na memória. **Essas variaveis não possuem realmente o valor do objeto, somente a localização**.

Objetos são criados em algum lugar na memória do seu computador. Quando declaramos:
```js 
arr = []
```
Estamos criando um **array** na memória do computador. O que a variável ```arr``` realmente recebe é o endereço, a localização, do array.

Vamos fingir que esse endereço é um novo tipo de dados passado por valor, tal como um ```number``` ou uma ```string```.
Um endereço aponta para a localização na memória, de um valor que é passado por referência.
Assim como uma string é indicada entre aspas, um endereço será indicado entre setas (< >). Quando atribuímos e usamos variáveis do tipo passadas por referência, o que escrevemos e vemos é:

```js
var arr = [];
```
| Variáveis | Valores | Endereço na memória | Objetos |
| :-------: | :-----: | :-----------------: | :-----: |
|    arr    | <#001>  |        #001         |   [ ]   |


```js
arr.push(1);
```

| Variáveis | Valores | Endereço na memória | Objetos |
| :-------: | :-----: | :-----------------: | :-----: |
|    arr    | <#001>  |        #001         |   [1]   |


Perceba que o valor, o endereço, contido na variável ```arr``` é estático. O array na memória é o que muda. Quando usamos ```arr``` para faze alguma operação, tal como adicionar um valor, a engine do **JS** acessa o local do arr na memória e trabalha com as informações armazenadas lá.

## Atribuindo por referência

Quando um valor do tipo passado por referência, um objeto, é copiado para outra variável utilizando ```=```, é o endereço dessa valor que é realmente copiado, como se fosse um tipo primitivo. Objetos são copiados por referência, em vez de por valor.

```js
var reference = [1];
var refCopy = reference;
```

O código acima parece assim na memória:

| Variáveis | Valores | Endereço na memória | Objetos |
| :-------: | :-----: | :-----------------: | :-----: |
| reference | <#001>  |        #001         |   [1]   |
|  refCopy  | <#001>  |          -          |    -    |

Agora, cada variável contém a referência para o mesmo **array**. Isso significa que se alterarmos ```reference```, ```refCopy``` **sofrerá as mesmas mudanças**.

```js
reference.push(2);
console.log(reference, refCopy); // -> [1, 2], [1, 2]
```

| Variáveis | Valores | Endereço na memória | Objetos |
| :-------: | :-----: | :-----------------: | :-----: |
| reference | <#001>  |        #001         |  [1,2]  |
|  refCopy  | <#001>  |          -          |    -    |


## Reatribuindo uma referência

A reatribuição  de uma variável de referência substitui a referência antiga.

```js
var obj = { first: "reference" };
```
| Variáveis | Valores | Endereço na memória |       Objetos        |
| :-------: | :-----: | :-----------------: | :------------------: |
|    obj    | <#234>  |        #234         | {first: "reference"} |


Quando temos uma segunda linha:

```js
var obj = { first: "reference" };
obj = { second: "ref2" };
```

O endereço gravado em obj muda. O primeiro objeto continua na memória,
e o próximo objeto:

| Variáveis | Valores | Endereço na memória |       Objetos        |
| :-------: | :-----: | :-----------------: | :------------------: |
|    obj    | <#678>  |        #234         | {first: "reference"} |
|     -     |    -    |        #678         |   {second: "ref2"}   |

Quando não há referência a um objeto remanescente, como vemos no endereço **#234** o **JS** pode executar o __Garbage Collector__. Isso significa que o programador perdeu todas as referências ao objeto, e não pode utilizá-lo novamente, para que o **JS** possa excluí-lo da memória com segurança. Nesse caso, o objeto ```{first: "reference"}``` não está mais acessivel e está disponível para o __Garbage Collector__.

## Operadores ```==``` e ```===```  

Quandos os operadores de igualdade ```==``` e ```===``` são utilizados em variáveis do tipo passadas por referência, eles verificam a referência. Se as variáveis contiverem um referência ao mesmo item, a comparação resultará em **true**.

```js
var reference = [1];
var refCopy = reference;

reference == refCopy; // true
reference === refCopy; // true
```

Se são objetos distintos, mesmos se possuem propriedades e valores idênticos, a comparação retornará falso.

```js
var arr1 = ["Hi"];
var arr2 = ["Hi"];

arr1 == arr2; // false
arr1 === arr2; // false
```


Se temos dois objetos distintos e queeremos ver se possuem as mesmas
propriedades, o método mais fácil é transformá-los em string e após compará-las. Quando o operador de igualdade compara primitivos ele pode simplesmente verificar se os valores são idênticos.

```js
var arr1str = JSON.stringify(arra1);
var arr2str = JSON.stringify(arr2);

arr1.str === arr2str; // true
```

Outra opção seria um loop recursivo pelos objetos verificando se cada propriedade é identica.


## Passando parâmetros através de funções

Quando passamos valores primitivos para uma função, a função copia os valores em seus parâmetros. É o equivalente a usar ``` = ``` para atribuir um valor.

```js
var hundred = 100;
var two = 2;
function multiply(x, y) {
  // PAUSE
  return x * y;
}
var twoHundred = multiply(hundred, two);
```

No exemplo acima, atribuímos o valor **100** a variável ```hundred```. Quando passamos ```hundred``` como parâmetro de ```multiply``` a varíavel ```x``` obtém o valor **100**. O valor é copiado como se tivessemos utilizando uma atribuição ```=```. Novamente o valor de ```hundred``` não é modificado. Abaixo vai uma cópia dos valores em memória no momento do comentário:

```// PAUSE na função multiply.```

| Variáveis | Valores | Endereço na memória |       Objetos       |
| :-------: | :-----: | :-----------------: | :-----------------: |
|  hundred  |   100   |        #333         | ```function(x,y)``` |
|    two    |    2    |          -          |          -          |
| multiply  | <#333>  |          -          |          -          |
|     x     |   100   |          -          |          -          |
|     y     |    2    |          -          |          -          |


## Funções puras

São aquelas que não afetam nada no escopo externo a sí. Desde que a função user apemas valores primitivos como parâmetros e não use variáveis em seu escopo circuncidante, ela é uma função pura, pois não podem afetar nada no escopo externo a sí. Todas as variáveis criadas dentro dela são removidas pelo __Garbage Collector__ assim que a função retorne.

Uma função que recebe um objeto, no entanto, pode alterar o estado do seu escopo circuncidante. Se uma função pega a referência de um __array__ e altera a __matriz__ para qual este aponta, as variaveis no escopo circuncidante que fazem referência a essa matriz enxergam essa alteração. Após o retorno da função, as mudanças persistem no escopo externo, podendo causar efeitos colaterais difíceis de rastrear.

Muitas funções de array nativas, incluindo ```Array.map``` e ```Array.filter```, são, portanto, gravadas como funções puras. Elas recebem a referência do array e internamente trabalham com uma cópia do mesmo, ao invés do original.

```js
// Função Impura
function changeAgeImpure(person) {
  person.age = 25;
  return person;
}
var alex = {
  name: "Alex",
  age: 30,
};
var changedAlex = changeAgeImpure(alex);
console.log(alex); // -> { name: 'Alex', age: 25 }
console.log(changedAlex); // -> { name: 'Alex', age: 25 }
```

A função impura recebe um objeto e altera a propriedade **idade** nesse objeto para o valor **25**. Como age na referência que foi dada, altera direamente o objeto ```alex```.

Observe que, quando retorna o objeto ```person```, ele está retornando exatamente o mesmo objeto que foi passado. ```Alex``` e ```changedAlex``` possuem a mesma referência. É redundante retorna a variável ```person``` e armazenar a referência em uma nova variável.

```js
// Função Pura
function changeAgePure(person) {
  var newPersonObj = JSON.parse(JSON.stringify(person));
  newPersonObj.age = 25;
  return newPersonObj;
}
var alex = {
  name: "Alex",
  age: 30,
};
var alexChanged = changeAgePure(alex);
console.log(alex); // -> { name: 'Alex', age: 30 }
console.log(alexChanged); // -> { name: 'Alex', age: 25 }
```


Nessa função, usamos JSON.stringify para convertermos o objeto
que passamos em uma string, e em seguida convertermos novamente
em um objeto com JSON.parse. Ao realizar essa conversão e armazenar
o resultado em uma nova variável, criamos um novo objeto.  
Existem outras maneiras de fazer isso, como percorrer o objeto original e atribuir cada uma de suas propriedades a um novo objeto, porém essa é a maneira mais simples.   
O novo objeto possui as mesmas propriedades que o original, mas está em um outro, e distinto, espaço de memória. Quando alteramos a propriedade age neste novo objeto, o orignal não é afetado, essa
função agora é pura. Ela não pode afetar nenhum outro objeto fora de seu próprio escopo, nem mesmo o objeto que foi passado como parâmetro.
O novo objeto precisa ser retornado e armazenado em uma nova variável, senão será coletado pelo Garbage Collector assim que a função retornar.



### Referência -  [Explaining Value vs. Reference in Javascript](https://codeburst.io/explaining-value-vs-reference-in-javascript-647a975e12a0)