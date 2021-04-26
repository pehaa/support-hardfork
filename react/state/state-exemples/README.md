# State - Exemples

```javascript
const [myValue, setMyValue] = React.useState(initialValue)
```

## Ex. Dark mode (1)

Dans cet exemple la variable `darkMode` est de type `boolean`.  
Si `darkMode` est `true` notre élément section prend des classes `bg-dark text-white`, dans le cas contraire il prend uniquement la class `bg-light`.

Le bouton "Toggle mode" permet de _switcher_ le mode.

https://codepen.io/alyra/pen/gOgvzvg

## Ex. Dark mode (2)

Dans cette exemple nous allons utiliser un component `ToggleModeButton`

https://codepen.io/alyra/pen/RwKQMwe

## Ex. Dark mode (3) - Uncontrolled component

https://codepen.io/alyra/pen/RwKQMVv

## Ex. Dark mode (4) - Controlled component

https://codepen.io/alyra/pen/jOyZzdB

## Ex. Select (uncontrolled)

https://codepen.io/alyra/pen/xxgYjbm

## Ex. Select (controlled)

https://codepen.io/alyra/pen/poRaVjd

> Dans la plupart des cas, pour implémenter des formulaires, nous recommandons d’utiliser des composants contrôlés. Dans un composant contrôlé, les données du formulaires sont gérées par le composant React. L’alternative est le composant non-contrôlé, où les données sont gérées par le DOM.

## Wallet

https://codepen.io/alyra/pen/LYNeYeL
