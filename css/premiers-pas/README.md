# CSS - Introduction

CSS vient de _Cascading Style Sheets_, ce qui se traduit en français par « feuilles de style en cascade » ([source MDN.](https://developer.mozilla.org/en-US/docs/Web/CSS))

Est-ce CSS un langage de programmation ? Pas tout à fait, c'est plutôt un langage de feuille de style. Il permet de communiquer avec le navigateur en indiquant l'aspect visuel d'un élément HTML.

CSS permet de passer le message:

> "Hey navigateur, affiche des paragraphes en rouge, et aussi pour chaque paragraphe, espace les lettres davantage, disons de 2px".

Ici on communique sur les propriétés: couleur - `color` et espacement des lettres - `letter-spacing`, mais aussi on indique sur quels éléments ces propriétés doivent être appliquées. CSS est alors lié très étroitement au HTML.

On vient aussi de parler des "pixels" - avec CSS nous allons apprendre à utiliser des unités adaptées aux écrans. Nous allons utiliser des `px` (pixels), ainsi que des `rem` et `em` (basée sur des pixels). Nous allons aussi utiliser des `vh` et `vw` (basée sur la largeur de la fenêtre dans le navigateur) et le pourcentage `%`.

## Syntaxe

```css
/*
selecteur  {
  propriété1: sa valeur;
  propriété2: sa valeur;
}
*/
p {
  color: red;
  letter-spacing: 2px;
}
```

## Comment lier CSS à un document HTML

Pour que les styles soient appliqués aux éléments dans le document HTML, leurs déclarations doivent être visibles par ce document.

Ceci peut être effectué à plusieurs façons :

1. Les déclarations CSS sont écrites dans un fichier séparé. Ce fichier aura pour extension `.css`, par exemple `style.css`.

Afin que le document HTML puisse "lire" les styles, nous devons ajouter une balise dans la partie `head` du document, comme ci-dessous :

```html
<!DOCTYPE html>
<html>
  <head>
    <!-- .. -->
    <link rel="stylesheet" href="chemin/vers/le/fichier/style.css" />
    <!-- .. -->
  </head>
</html>
```

2. Les déclarations de styles peuvent être aussi incluses directement dans la partie `head` (sans passer par un fichier séparé). Dans ce cas-là, on utilise les balises `<style>` comme ci-dessous :

```html
<!DOCTYPE html>
<html>
  <head>
    <!-- .. -->
    <style>
      p {
        color: red;
        letter-spacing: 2px;
      }
      h1 {
        color: magenta;
      }
    </style>
    <!-- .. -->
  </head>
</html>
```

3. On peut aussi appliquer des styles css à un élément directement dans HTML, avec un attribut `style`

```html
<p style="color: crimson; letter-spacing: 3px;">
  Haha ! Je crois que je vais rougir !
</p>
```

## Exercices

- [Texte](https://codepen.io/alyra/pen/XWmGaxE)
- [Ajouter des classes](https://codepen.io/alyra/pen/VwvRzRJ)
- [Sac à dos](https://codepen.io/alyra/pen/LYpwabq)
- [Trottinette ](https://codepen.io/alyra/pen/abvMLBM)
- [Chotto Motto](https://github.com/pehaa/hardfork-ex-chotto-motto)
