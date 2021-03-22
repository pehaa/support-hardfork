# Destructuring Assignement

_(fr. affectation par décomposition)_

Nous allons découvrir quelques secrets de la syntaxe des objets et tableaux qui rendent notre code plus court et parfois plus maintenable.

## objects

```javascript
const name = "Deej"
const age = 300
const alien = {
  name,
  age,
}
/* équivalent à
alien = {
  name: name,
  age: age
}
alien = {
  name (clé "name"): name (valeur de la variable name),
  age (clé "age"): age (valeur de la variable age)
}
*/
```

Ça marche aussi dans le "sens inverse" :

```javascript
const alien = {
  name: "Deej",
  age: 300,
}
const { name } = alien

/* équivalent à
const name = alien.name // 'Deej'
*/

const { age } = alien

/* équivalent à
const age = alien.age // 300
*/
```

Définir plusieurs variables à la fois est aussi possible est souvent utilisé :

```javascript
const alien = {
  name: "Deej",
  age: 300,
}
const { age, name } = alien

/* équivalent à
const name = alien.name // 'Deej'
const age = alien.age // 300
*/
```

S'il n'y a pas de clé correspendant au nom de la variable, sa valeur est `undefined`:

```javascript
const alien = {
  name: "Deej",
  age: 300,
}

const { age, id, name } = alien
/* équivalent à
const age = alien.age // 300
const id = alien.id // undefined
const name = alien.name // "Deej"
*/
```

Nous pouvons aussi prévoir un _fallback_, la valeur par défaut :

```javascript
const alien = {
  name: "Deej",
  id: "_deej",
}

const { age = 100, id, name } = alien
/* équivalent à
const age = typeof alien.age !== "undefined" ? alien.age : 100 // 100
const id = alien.id // "_deej"
const name = alien.name // "Deej"
*/
```

On peut aussi récupérer une partie qui "reste" d'objet dans une variable, souvent on utilisera le nom `rest` pour cette variable (ceci est une convention). Pour cela on utilise les "..."

```javascript
const alien = {
  name: "Deej",
  id: "_deej",
  age: 200,
}

const { name, ...rest } = alien
console.log(name) // 'Deej'
console.log(rest)
/*
{
  age: 200,
  id: "_deej"
}
*/
```

## arrays

Dans arrays les valeurs sont indexées et ne possèdent pas des noms des clés, mais la décomposition est possible :

```javascript
const spices = ["chili", "poivre", "sel"]
const [a, b] = spices
// a - premier élément de l'array spices -> 'chili'
// b - 2e élément de l'array spices -> 'poivre'
```

```javascript
const spices = ["chili", "poivre", "sel"]
const [a, , b] = spices
// a - premier élément de l'array spices -> 'chili'
// b - 3e élément de l'array spices -> 'sel'
```

```javascript
const spices = ["chili", "poivre", "sel"]
const [a, ...rest] = spices
// a - premier élément de l'array spices -> 'chili'
// rest - ['poivre', 'sel']
```

```javascript
const spices = ["chili", "poivre", "sel"]
const [a, b, c, d] = spices
// a - premier élément de l'array spices -> 'chili'
// b - 2e élément de l'array spices -> 'poivre'
// c - 3e élément de l'array spices -> 'sel'
// d - 4e élément de l'array spices -> undefined
```

---

## Exercices :

[js tours - destructuring](https://jstours-destructure.netlify.app/)
