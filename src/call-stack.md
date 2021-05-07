# Pilha de chamadas


 A pilha de chamadas é maneira como o compilador do __Javascript__ organiza as múltiplas funções que serão executadas em um programa, sempre que uma função é chamada ela é adicionada ao topo da pilha e executada na ordem __LIFO(Last In, First Out)__ que no português signifca último a chegar primeiro a sair, uma vez que a função é totalmente executada ele é removida da pilha e o código continua a ser executado de onde parou antes de chamar a função.

De maneira resumida começamos com uma pilha vazia, e as funções do programa são adicionadas a pilha de chamadas conforme elas são chamadas no código, a execução acontece na ordem LIFO ( Last In, First Out), ou seja a última função adiciona a pilha é sempre a primeira a ser executada.

## Exemplo

```js
function funcao1() {
  funcao2();
  console.log("Executou função 1");
}

function funcao2() {
  funcao3();
  console.log("Executou função 2");
}

function funcao3() {
  console.log("Executou função 3");
}

funcao1();

// Ordem de execução:
funcao3();
funcao2();
funcao1();

// Resultado da execução da funcao1()
"Executou função 3"
"Executou função 2"
"Executou função 1"
```

### Referência -  [Entenda de uma vez por todas a pilha de chamadas (Callstack) do JavaScript](https://js.pro.br/callstack-entenda-pilha-chamadas-javascript)