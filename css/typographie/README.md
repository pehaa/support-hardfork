# Typographie

Vous l'avez peut-être déjà remarqué, certaines propriétés CSS se transmettent du parent vers enfant, d'autres non.

Les propriétés relatives au texte s’héritent.

Par exemple, si vous fixez une color et une font-family pour l'élément `body`, tout élément sera mis en forme avec cette même couleur et cette même police, **à moins qu'on lui ait appliqué directement des règles.**

## `font-family`

La propriété `font-family` définit une liste, ordonnée par priorité, de polices à utiliser pour mettre en forme le texte. Les valeurs sont séparées par des virgules. Le navigateur va utiliser la première police qui est soit installée chez l'utilisateur ou qui peut être chargée depuis un serveur distant.

```css
body {
  font-family: "Open Sans", Helvetica, Arial, sans-serif;
}
```

Dans l'exemple ci-dessus, le navigateur va essayer d'appliquer la police "Open Sans", ensuite la police Helvetica, si elle n'est pas disponible il essayera Arial. Si ni Open Sans, ni Helvetica ni Arial ne sont disponibles, la police de type générique `sans-serif` sera utilisée. **La famille de police générique doit toujours être incluse** (et bien évidemment sur la dernière position).

### Les principales familles de polices génériques

- `serif` - les caractères ont des empattements
- `sans-serif` - les caractères n'ont pas d'empattement
- `cursive` - les extrémités des caractères peuvent se joindre les uns aux autres
- `fantasy` - des polices décoratives
- `monospace` - chaque caractère prend la même largeur (police est à chasse fixe)

https://codepen.io/alyra/pen/eYJOaOO

Dans notre déclaration de `font-family`, juste avant la famille générique, nous listons souvent des polices qui sont installées chez la majorité des utilisateurs. Vous trouverez la liste des polices _safe for the web_ avec les statistiques les concernant sur [Web-safe Fonts](https://www.cssfontstack.com/)

## `font-weight`

La propriété `font-weight` permet de définir la graisse utilisée pour le texte. Les niveaux de graisse disponibles dépendent de la police. Il existe des polices disponibles avec plusieurs niveaux de graisse, et des polices avec une seule variante.

```css
header p {
  font-weight: bold;
}
```

**Les valeurs de `font-weight` :**

- `normal` - équivalent à la valeur numérique de 400
- `bold` - texte est en gras, équivalent à la valeur numérique de 700
- numérique - selon l'ancienne spécification 100, 200, 300, 400, 500, 600, 700, 800 ou 900. Selon la nouvelle spécification une valeur comprise entre 1 et 1000
- `lighter` - on diminue la graisse d'un niveau par rapport à l'élément parent (selon les fontes / graisses disponibles pour la police utilisée)
- `bolder` - on augmente la graisse d'un niveau par rapport à l'élément parent (selon les fontes / graisses disponibles pour la police utilisée)

Si la graisse demandée n'est pas disponible, le navigateur procède à une conversion. Vous pouvez trouver les règles de cette conversion ainsi que des détails de mise en place `font-weight: lighter` et `font-weight: bolder` dans [cet article sur MDN](https://developer.mozilla.org/fr/docs/Web/CSS/font-weight)

## `font-style`

La propriété `font-style` définit si la police devrait utiliser la fonte normale (`normal`), italique (`italic`) ou oblique (`oblique`)

```css
header p {
  font-style: italic;
}
```

**Les valeurs de `font-style`**

- `normal`
- `italic` - une police qualifiée d'italic, s'il n'y a pas de version italique, une version oblique sera sélectionnée à la place.
- `oblique` - une police qualifiée d'oblique, s'il n'y a pas de version oblique, une version italic sera sélectionnée à la place.
- `oblique <angle>` - une police qualifiée d'oblique avec un angle pour la pente du texte spécifié (très rarement utilisée)

Dans chaque cas, si aucune police oblique n'est disponible, le navigateur synthétisera une police penchée en tournant les caractères d'une fonte normale.

## `font-size`

La propriété `font-size` définit la taille de fonte utilisée pour le texte. La propriété font-size peut être définie de deux façons :

- (rarement) comme un mot-clé désignant une taille absolue ou une taille relative (`xx-small`, `x-small`, `small`, `medium`, `large`, `x-large`, `xx-large`)
- (parfois) `<percentage>` - les valeurs exprimées en pourcentages (`%`) sont proportionnelles à la taille de fonte de l'élément parent

  ```css
  small {
    font-size: 75%;
  }
  ```

- (très souvent) comme une valeur de type `<length>`. Nous allons parler ici des unités : `px`, `em` et `rem`.

### `px`

`px` - souvent utilisé, pourtant l'utilisation des pixels pour la taille de police n'est pas le meilleur choix, et nous allons abandonner cette approche. Utilisation de pixels ne permet pas aux navigateurs d'appliquer des réglages utilisateur concernant la taille des polices - ceci est illustré dans la vidéo co-dessous.

https://wptemplates.pehaa.com/assets/alyra/px-vs-rem-em.mp4

### `em`

`em` - la taille d'une valeur exprimée en `em` est dynamique. `1em` est équivalent à la taille de fonte appliquée à l'élément parent de l'élément courant. Si cette taille n'a pas été définie pour l'élément parent, elle correspondra à la taille par défaut du navigateur (généralement 16px).

```css
h2 {
  font-size: 2em;
  /* 2 * 16px = 32px */
}
h2 span {
  font-size: 0.75em;
  /* 0.75 * 32px = 24px */
}
```

Le problème avec les unités `em` est qu'il faut toujours prend en compte la taille de police dans l'élément parent (la composition).

https://codepen.io/alyra/pen/vYXYyRz

### 🤩 `rem` 🥳

`rem` - l'arrivé des unités `rem` a permit de régler le problème de la composition, `rem` est une unité dynamique, mais la taille de police est relative à l'élément `<html>` (root)

```css
h2 {
  font-size: 2rem;
  /* 2 * 16px = 32px */
}
h2 span {
  font-size: 1.5rem;
  /* 1.5 * 16px = 24px */
}
```

https://codepen.io/alyra/pen/VweZJzR

## Autres propriétés typographiques:

`text-decoration` (`none`, `underline`, `overline`, `line-through`)  
`text-align` - alignement au sein d'un élément block (`left`, `right`, `center`, `justify`)  
`line-height` - définit la hauteur de la boîte d'une ligne. Je vous recommande d'utiliser les valeurs numériques (facteur multiplicateur de la taille de fonte utilisée). Pour la bonne lisibilité `line-height` devrait être comprise entre `1.3` et `2`.

## Utilisation de Google Fonts

Les services de police en ligne stockent et servent des polices avec le code CSS associé permettant leur intégration par le navigateur (les déclarations @font-face). [Google Fonts](https://fonts.google.com/) est un service de ce type, et en plus c'est un service gratuit. Comment utiliser Google fonts :

1. Choisissez vos polices, copiez le code indiqué par le service et collez-le dans la partie `head` de votre document HTML. Par exemple

```html
<link rel="preconnect" href="https://fonts.gstatic.com" />
<link
  href="https://fonts.googleapis.com/css2?family=Nerko+One&display=swap"
  rel="stylesheet"
/>
```

2. Ensuite vous pouvez utiliser cette police dans votre feuille de style :

```css
h1,
h2,
h3,
h4 {
  font-family: "Nerko One", cursive;
}
```

Si vous cherchez des polices avec un caractère particulier, vous pouvez vous servir du site [goofonts.com](https://goofonts.com) qui permet de faire une recherche par "tag".

[![](https://wptemplates.pehaa.com/assets/alyra/goofonts.png)](https://goofonts.com)

---

## Exercices

- [Wanted Dead or Alive - Google Fonts](https://github.com/pehaa/hardfork-exercice-police)
