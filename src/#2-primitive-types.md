# Tipos primitivos

Objetos são agregações de propriedades. Um propriedade pode referenciar um **objeto** ou um **primitivo**. **Primitivos** são __valores__, não possuem __propriedades__.

No __Javascript__ há cinco tipos primitivos: ```undefined,js null, boolean, string e number ```, todo o resto é um objeto. Os tipos primitivos ```boolean, string e number``` podem ser agrupados por suas contrapartes de objeto.
Esses objetos são instâncias dos construtores de ```boolean, String e Number```, respectivamente.

```js
typeof true; //"boolean"
typeof Boolean(true); //"boolean"
typeof new Boolean(true); //"object"
typeof new Boolean(true).valueOf(); //"boolean"

typeof "abc"; //"string"
typeof String("abc"); //"string"
typeof new String("abc"); //"object"
typeof new String("abc").valueOf(); //"string"

typeof 123; //"number"
typeof Number(123); //"number"
typeof new Number(123); //"object"
typeof new Number(123).valueOf(); //"number"
```

## Mas, se primitivos não possuem propriedades, por qual motivo __"abc".length__ retorna um valor?

Pois o __Javascript__ irá forçar a conversão entre **primitivos** e **objetos**. Nesse caso a ```string``` é convertida em um objeto de ```string``` para que se possa ter acesso a todas as propriedades definidas pelo seu construtor.
O objeto de ``` string```  é usado somente por uma fração de segundos.

```js
String.prototype.returnMe = function () {
  return this;
};
var a = "abc";
var b = a.returnMe();

a; //"abc"
typeof a; //"string" (still a primitive)
b; //"abc"
typeof b; //"object"
```
## E os objetos também poder ser forçados para valores?


Sim, mas maioria das vezes. Objetos desse tipo são meramente invólucros, seu valor e o primitivo que eles envolvem e geralmente forçarão esse valor conforme necessário.

```js
// String
var a = new String("john");
typeof a; // "object"
var b = a + " doe"; // result => john doe
typeof b; // string

// Number
var number = new Number(3);
typeof number; // "object"
var sum = number + 3;
typeof sum; // number
```

Infelizmente, objeto ```booleanos``` não se forçam tãofacilmente. Um objeto ```booleano``` é tido como verdadeiro a menos que seu valor  seja nulo ou indefinido.  
Tente o seguinte:

```js
if (new Boolean(false)) {
  alert("true???");
}
```
Geralmente você precisa explicitar os valores dos objetos ```booleanos```. O seguite pode ser útil para inferir se o valor é verdadeiro ou falso:

```js
var a = "";
new Boolean(a).valueOf(); //false
```

Na prática é mais fácil fazer isso:

```js
var a = Boolean("");
a; //false
```

Ou mesmo isso: 
```js
var a = ""
!!a; //false
```
## Consigo forçar valores em tipos primitivos?

A resposta é **NÃO!**

```js
var primitivo = "carro";
primitivo.rodas = 4;
primitivo.rodas; // undefined;
```


Se o Javascript detectar uma tentiva de atribuir uma propriedade a um primitive ele irá força o primitivo para um objeto. Mas, como no exemplo anterior, esse recém criado objeto não possuir referências ele será consumido pelo garbage collector.   
Aqui está uma representação para ilustrar o querealmente acontece:
 
 ```js
var primitivo = "carro";
primitivo.rodas = 4;
// novo objeto criado para definir a propriedade
new String("carro").rodas = 4;
primitivo.rodas;
//outro novo objeto criado para retirar a propriedade
new String("carro").rodas; //undefined
```

## Wrapper

Acontece que a capacidade de atribuir propriedades é 
quase a única vantagem dos objetos sobre suas contrapartes primitivas, mas na prática mesmo essa
é uma qualidade duvidosa. ``` Strings, booleanos e
numbers ``` têm propósitos específicos e bem definidos, e redefini-los com detentores de estados provavelmente vai confundir as pessoas. Além disso como os primitivos são imutáveis, você não pode modificá-las, ajustando as propriedades do wrapper de objeto:

```js 
var eu = new String("Vyctor");
eu.length = 2; //(error in strict mode)
eu.length; //5
eu.valueOf(); // Vyctor
```


### Referência: [The Secret Life of Javascript Primitives](https://javascriptweblog.wordpress.com/2010/09/27/the-secret-life-of-javascript-primitives/)