# Focus sur "l'anatomie" de `useEffect` et lazy state initialisation 
a.k.a. Réponse à la question de Christophe

Tous le hooks utilisés dans un component React sont appelés à CHAQUE render de ce component. Les hooks doivent être dans le body du component, jamais derrière une condition, justement pour assurer qu'ils soient exécutés à chaque render.

Quand une fonction est exécuté, son paramètre est évalué peu importe s'il est ensuite utilisé ou pas :

```js
const functionWithUselessParameter = (p) => null
const somethingToEvaluate = () => {
  console.log("If you read this, I did run.")
}

functionWithUselessParameter(somethingToEvaluate())
// "If you read this, I did run."
```

https://codepen.io/alyra/pen/OJWGBPW


L'exemple suivant est plus proche à `useState` où le paramètre n'est utilisé qu'une seule fois :

```js
let initialRender = true
const functionWithMostlyUselessParameter = (p) => {
  if (initialRender) {
    initialRender = false
    return p
  }
  return null
}

// somethingToEvaluate runs et est utilisé
functionWithMostlyUselessParameter(somethingToEvaluate())
// somethingToEvaluate runs mais n'est pas utilisé
functionWithMostlyUselessParameter(somethingToEvaluate())
```

https://codepen.io/alyra/pen/ZELZqYM


## Comment pouvons nous optimiser ça ?

En permettant de passer une fonction en tant que paramètre. Exactement comme c'est fait dans `useState` - avec le _lazy state initialisation._

```js
let initialRender = true
const somethingToEvaluate = () => {
  console.log("If you read this, I did run.")
}

const betterFunctionWithMostlyUselessParameter = (p) => {
  if (initialRender) {
    initialRender = false
    if (typeof p === "function") {
      return p()
    }
    return p
  }
  return null
}

// somethingToEvaluate runs et est utilisé
betterFunctionWithMostlyUselessParameter(somethingToEvaluate)
// somethingToEvaluate does not run
betterFunctionWithMostlyUselessParameter(somethingToEvaluate)
```

https://codepen.io/alyra/pen/oNBOPEV

## Passons à `useEffect`.

```js
useEffect( () => {
  /* some side effects code */
  return () => {
  /* some clean up code */
  }
}, [dependecies])
```

`useEffect` est également évalué à CHAQUE render. Si `/* some side effects code */` n'est pas enveloppé dans une fonction, il va être directement exécuté et nous perdons tout le contrôle sur les side effects. 

Pareil pour la fonction de "clean up". Si `/* some clean up code */`  n'est pas enveloppé dans une fonction, `/* some clean up code */` va être exécuté au moment ou sideEffect est évalué.

En ce qui concerne les paramètres à passer dans ces fonctions ? Que pourrait on vouloir y passer ? `useEffect` a pour la tâche de synchroniser "state of the world" avec "state of the component". Les fonction "callback" et "clean up" ont déjà accès à toutes les valeurs de state du component (c'est dans leur global scope).

Aussi, c'est naturel que useEffect lui même ne retourne rien. Il est là pour des side effects donc retourner quelque-chose n'a pas beaucoup de sens.

J'espère que ça fait sens.