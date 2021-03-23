# JavaScript - premiers pas avec types et expressions

Bienvenue dans le monde de JavaScript :) 

Nous allons commencer notre aventure par un aperçu des différents types des données. Nous allons apprendre comment travailler avec eux. Dans JavaScript il existe plusieurs types de données simples (nommés primitifs) ainsi que des objets et des fonctions (que nous allons découvrir un peu plus tard).

_Qu'est que ça veut dire type de données ? Pourquoi devons-nous en parler ?_ Voici un exemple. Avec JavaScript nous pouvons exécuter des opérations arithmétiques, par exemple `1 + 2`. Comme vous pouvez le deviner, `1 + 2` nous donnera... `3`. 

Nous pouvons aussi effectuer des opérations sur les chaines de caractères. Par exemple, `"Java" + "Script"` sera évalué en tant que `"JavaScript"`. De la même façon `"1" + "2"` donnera `"12"`. Vous voyez la différence entre `"1" + "2"` et `1 + 2` ? 

Mais quel sera alors le résultat de `"1" + 2` 🧐 ? Vous saurez répondre à cette question suite à ce premier cours de JavaScript !

## Types de données, opérateur `typeof` :

Nous allons utiliser JavaScript avec Node.js que vous avez installé et qui est disponible dans votre terminal. Vous pouvez alors ouvrir votre terminal et taper `node`. Pour sortir de mode "node", tapez `.exit` 

Pour vérifier le "type" nous disposons de l'opérateur `typeof` qui retourne le type.

### (primitif) **number**

Voici quelques exemples de type primitif `"number"`, testez chacun d'eux avec `typeof` dans votre terminal :

```javascript
2
33.6
-10
1e5
Infinity
-Infinity
NaN

typeof(5)
// 'number'
```

Comme vous pouvez l'observer, pour chacun de nos exemples `typeof(...)` nous donne le résultat `"number"`.
Pareil, en effectuant des opérations arithmétiques sur les données de type "number" nous obtenons des résultats de type `"number"`, par exemple :

```javascript
typeof(2 + 4) // 'number'
typeof(3 * 2) // 'number'
typeof(0 / 0) // 'number'
typeof(1 / 0) // 'number'
typeof(-1 / 0) // 'number'
```

### (primitif) **string**

La chaine de caractères (`"string"`) est entourée par des guillemets, comme dans les exemples suivants :

```javascript
"Bonjour tout le monde !"
"Hello world!"
"a"
'a'
`a`
"123"
'123'
`123`


"Je m'appelle Paulina"
`Je m'appelle Paulina`
//'Je m'appelle Paulina' <- ceci génère une erreur, mais nous pouvons le fixer avec le symbole "\"
'Je m\'appelle Paulina'

"Je pense que 320px donnera " + 320 / 16 + "rems"

`Je pense que 320px donnera ${320 / 16}rems`


// le string avec un saut de ligne est possible uniquement avec des ``
`Bonjour et
Bonsoir`

// ou nous sommes obligés d'ajouter le symbole "\" 

"Bonjour et \
Bonsoir"
```

```javascript
typeof("Bonjour tout le monde !")
// 'string'
typeof("123")
// 'string'
```

### (primitif) **boolean**,

Le troisième type primitif, particulièrement important est `"boolean".` Il prend une des deux valeurs possibles : `true` (vrai)  ou `false` (faux).  
Ceci permet de conditionner le comportement de notre script. 

Par exemple, si l'utilisateur est connecté - l'interface affiche son tableau de bord, dans le cas contraire l'interface affiche le formulaire de connexion.

Quels sont des exemples du type `"boolean"` ? Ce seront tous les résultats de comparaison, tels que `10 > 2` ou `-3 < -4`. 


```javascript
typeof(true) // 'boolean'
typeof(false) // 'boolean'
typeof(10 > 2) // 'boolean'
10 > 2 // true
typeof (10 == 2) // 'boolean'
10 == 2 // false
typeof (10 != 2) // 'boolean'
10 != 2 // true
```

### (primitif) **undefined**

Quand JavaScript rencontre une chaine de caractères qui n'est pas entourée par des guillemets, il cherche la fonction ou la variable qui est identifiée avec cette chaine de caractère. (Nous allons parler des variables et des fonctions très bientôt). Si JavaScript ne trouve pas de valeur qui correspond à notre chaine, son type est `"undefined"`.

```javascript
typeof a
// 'undefined'
typeof paulina
// 'undefined'
```

<hr/>

### D'autres types

Dont nous ne parlons pas encore aujourd'hui

- (primitif) **null**, `null` est un type primitif et pourtant `typeof(null)` donne `"object"` - ceci est en fait une erreur du langage, `null` n'est pas un `object`.

- (primitif) **BigInt**

- (primitif) **Symbol**

- **Object**

- **Function**

## Expressions

Nous parlons d'expression JavaScript quand nous demandons à JavaScript de nous **donner un résultat**. Par exemple, `1 + 2` est une expression, son résultat est 3. Regardons ensemble d'autres exemples des expressions JavaScript.

### Numbers

Plusieurs expressions JavaScript peuvent être composées avec des opérations arithmétiques, telles que :

- addition, opérateur `+`  
- soustraction, opérateur `-`  
- multiplication, opérateur `*`  
- division, opérateur `/`  
- modulo, opérateur `%` - le reste de division  
- puissance, opérateur `**`

```javascript
6 % 3 // 0
6 % 2 // 0
3 % 2 // 1
-3 % 2 // -1
```

```javascript
6 ** 3 // 216 puisque 6*6*6
5 ** 4 // 625 puisque 5*5*5*5
```

### Strings

Dans le contexte des `"string"` l'opérateur qui est souvent utilisé est le `+` qui correspond à la concaténation. 

```javascript
"Hello" + " " + "World" + "!"
// Hello World!
```

### Booleans

Dans le contexte de valeurs du type `"boolean"`, nous utilisons souvent les opérateurs `!`, `||` est `&&`

**negation** `!`

```javascript
!true
// false
!false
// true
!(10 < 2)
// !false -> true
```

**or / ou** `||`

```javascript
true || true
//true
true || false
//true
false || true
//true
false || false
//false
```

**and / et** `&&`

```javascript
true && true
//true
true && false
//false
false && true
//false
false && false
//false
```

## Type conversion & type coercion

### Type conversion, *truthy* et *falsy*

Nous parlons de *type conversion* quand nous changeons le type d'une valeur de la manière explicite (c'est moi, le programmeur, qui décide de le faire dans mon script).


```javascript
// type conversion (changement du type explicite)
Number("1") + Number("2") // Number("1") + Number("2") -> 1 + 2 -> 3
Number("Bonjour") // NaN
Number(false) // 0
Number(true) // 1
Number(null) // 0
Number(undefined) // NaN
Number("   ") // 0
```

```javascript
// type conversion (changement du type explicite)
String(2) // -> "2"
String(true) // -> "true"
```

```javascript
// type conversion (changement du type explicite)
Boolean(2)
// Boolean(2) -> true
Boolean(0)
// Boolean(0) -> false
Boolean("Bonjour")
// Boolean("Bonjour") -> true
```

Comme vous l'observez pour convertir le type nous utilisons les fonctions :

 - `Number` - afin de convertir une valeur vers `"number"`
 - `String` - afin de convertir une valeur vers `"string"`
 - `Boolean` - afin de convertir une valeur vers `"boolean"`
 
Ils existent quelques règles concernant la conversion vers le type `"boolean"`. À cette occasion nous allons parler des notions *truthy* et *falsy.*

Valeurs **falsy** sont celles qui convertissent en `false`. La liste des valeurs **falsy** n'est pas longue :

```javascript
false
null
undefined
0
NaN
""
''
``
```

Valeurs **truthy** sont celles qui convertissent en `true`. Quelle est la liste des valeurs **truthy** ? Ce n'est pas possible de la monter, par contre, il suffit de savoir, qu'une valeur est *truthy* si elle n'est pas *falsy* 🤓.


### Type coercion
  
Nous parlons de *type coercion* quand le type d'une valeur est converti implicitement (c'est JavaScript qui le fait à la volée afin d'effectuer une opération). Souvent un opérateur provoque la conversion du type d'une valeur. Revenons à notre exemple du début du cours où nous essayons d'effectuer l'opération `"1" + 2` (additionner un `"string"` et un `"number"`). Afin que cette opération soit possible, le type d'une de ses opérandes devra être converti. La valeur numérique sera convertie en `"string"` et le résultat de cette opération sera `"12"`.

```javascript
// type coercion (changement du type implicite)
"1" + 2
// "1" + 2 -> "1" + "2" -> "12"
```

Voici quelques règles :

- Opérateurs `-`, `*`, `/`, `**` etc. provoquent la coercion vers le type `"number"`

```javascript
1 - "2" // -> 1 - 2 -> -1
"3" - "5" // -> 3 - 5 -> -2
"2" * 10 // -> 2 + 10 -> 20
 ```
 
 - Opérateur binaire `+` avec une des opérandes de type `"string"` provoque la coercion vers le type `"string"`
 
 ```javascript
1 + "2" // -> "1" + "2" -> "12"
"3" + "5" // -> "3" + "5" -> "35"
"2" + 10 // -> "2" + "10" -> "210"
 ```
 
 mais 
 
 ```javascript
 true + false // -> 1 + 0 -> 1
 true + true // -> 1 + 1 -> 2
 null + null // -> 0 + 0 -> 0
 3 + undefined // -> 3 + NaN -> NaN
 ```
 
 - Négation `!` provoque la coercion vers le type `"boolean"`
 
 ```javascript
 !10 // -> !true -> false
 !null // -> !false -> true
 !0 // ->!false -> true
 ```
 
 - **L'égalité faible** ("double égal")  `==` si les valeurs ne sont pas du même type, **provoque la coercion**
 
 ```javascript
 1 == "1" // -> 1 == 1 -> true
 1 != "1" // -> 1 != 1 -> false
 "-2" == -2 // -> -2 == 2 -> true
 "-2"!= -2 // -> -2 != 2 -> false
 false == 0 // -> 0 == 0 -> true
 false != 0 // -> 0 != 0 -> false
 true == 2 // => 1 == 2 -> false
 true != 2 // => 1 != 2 -> true
 ```
 
 **Attention :** `a != b` est équivalent à `!(a == b)`

- Par contre, dans **l'égalité stricte**  ("triple égal")  **`===` la coercion n'est pas permise**

 ```javascript
 1 === "1" // false
 1 !== "1" // true
 "-2" === -2 // false
 "-2" === -2 // false
 "-2"!== -2 // true
 false === 0 // false
 false !== 0 // true
 ```
 
  **Attention :** `a !== b` est équivalent à `!(a === b)`
 
 Pour en savoir plus [JavaScript Equality Table](https://dorey.github.io/JavaScript-Equality-Table/)
 
 - Opérateurs `<`, `<=` 
 
Pour comparer des strings JavaScript utilise l'ordre alphabétique, par exemple `"A" < "Z" < "a" < "z"`.  
Pour comparer des valeurs de type différent, la coercion vers le type `"number"` est effectuée.

 ```javascript
 "100" < "21" // true
 100 <= "21" // -> 100 < 21 -> false
 false < 2 // -> 0 < 2 -> true
 false < "3" // -> 0 < 3 -> true
 ```
 
## Opérateurs logiques `||` et `&&`

Nous avons déjà vu comment `||` et `&&` opèrent avec des valeurs de type `"boolean"`. Ces opérateurs sont souvent utilisés avec des valeurs de types différents. Il est alors important de comprendre leurs comportements, assez spécifiques.

### `||`

JavaScript procède de gauche à droite. Chaque valeur est convertie dans la mémoire en type `"boolean"` et l'évaluation s'arrête au moment où on tombe sur la valeur _truthy_. Le résultat est la valeur qui est évaluée comme _truthy_. Si aucune des valeur n'est _truthy_, le résultat est la dernière valeur.

```javascript
0 || undefined || "JavaScript" || "" 
// "JavaScript"
0 || undefined || "" 
// ""
0 || undefined || "Lunch" || "JavaScript" 
// "Lunch"
"Burrito" || "Gaspacho" || "Tacos" 
// "Burrito"
```

## `&&`

JavaScript procède de gauche à droite. Chaque valeur est convertie dans la mémoire en type `"boolean"` et l'évaluation s'arrête au moment où on tombe sur la valeur _falsy_. Le résultat est la valeur qui est évaluée comme _falsy_. Si aucune des valeurs n'est _falsy_, le résultat est la dernière valeur.

```javascript
0 && undefined && "" 
// 0
"Lunch" && 0 && undefined && "" 
// 0
"Burrito" && "Gaspacho" && "Tacos" 
// "Tacos"
```

## Opérateur conditionnel (ternaire)

JavaScript permet d'évaluer le résultat en fonction si la condition est vraie ou fausse. Voici la syntaxe de l'opérateur ternaire (ternary en anglais)

```javascript
condition ? expressionSiVrai : expressionSiFaux
```

```javascript
10 > 4 ? "pain au chocolat" : "chocolatine"
// "pain au chocolat"
```

```javascript
2 * 2 === 4 ? "Burrito" : "Tacos"
// "Burrito"
```

```javascript
2 ** 3 === 9 ? "Burrito" : "Tacos"
// "Tacos"
```

Pour la partie `condition` la coercion vers `"boolean"` est effectuée si besoin :)

```javascript
4 % 2 ? "Bonjour" : "Bonsoir"
// -> 0 ? "Bonjour" : "Bonsoir"
// -> false ? "Bonjour" : "Bonsoir"
// -> "Bonsoir"
```

---

## Exercices

[JavaScript Quizz - Types & Expressions](https://javascript-quizzes.netlify.app/types)
