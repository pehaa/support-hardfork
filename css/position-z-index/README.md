# Propriété CSS <code>position</code> <span role="img" aria-label="stars">✨</span>

![Mask](https://wptemplates.pehaa.com/assets/alyra/mask.jpg)

Propriété CSS `position` est une propriété très particulière. J'aime dire que c'est une propriété aux pouvoirs magiques ✨.


Par défaut la valeur de la propriété `position` est `static`. Quand un élément à la valeur `position` autre que `static`, nous l'appelons **positionné.** Voici ce qui peut arriver à un élément positionné :

- l'élément peut "sortir" du layout, autrement dit il est retiré du flux normal. D'autres éléments se comportent comme s'il n'existe pas.
- nous pouvons placer l'élément en fonction de son parent "positionné" ou par rapport aux bords de l'écran en utilisons les propriétés 
  - `top`,
  - `bottom`,
  - `left`,
  - `right`
- l'élément gagne accès à la troisième dimension - il peut se mettre au-dessus ou au-dessous des autres éléments. Nous pouvons imaginer que l'élément est placé dans sa propre pochette. Ou que l'élément est dans son propre calque (ceci  parlera aux utilisateurs des logiciels type Photoshop).  
La propriété qui permet de "naviguer" dans cette troisième dimension qui gère l'ordre des calques (pochettes), est `z-index`. Vous pouvez [en lire davantage sur MDN](https://developer.mozilla.org/fr/docs/Web/CSS/Comprendre_z-index/Empilement_de_couches)
- l'élément crée les coordonnées pour ses éléments enfants. Les éléments enfants, une fois positionnés eux-mêmes, pourront être placés avec des propriétés `top`, `bottom`, `left` et `right`

## Valeurs de la propriété `position`

Commençons par lister de possibles valeurs :

- `static` (par défaut, magie n'opère pas)
- `relative`
- `absolute`
- `fixed`
- `sticky`

### `position: static;`

❌ élément "sort" du layout  
❌ `top`, `bottom`, `left`, `right` activées   
❌ propriété `z-index` activée  
❌ élément crée des coordonnées pour ses enfants  

### `position: relative;`

❌ élément sort du layout  
✅ `top`, `bottom`, `left`, `right` activées (décalage d'élément par rapport à sa position initiale)  
✅ propriété `z-index` activée  
✅ élément crée des coordonnées pour ses enfants  

https://codepen.io/alyra/pen/KKgpGpx

### `position: absolute;`

✅ élément sort du layout  
✅ `top`, `bottom`, `left`, `right` activées (position par rapport à l'ancêtre le plus proche qui est positionné ou html)  
✅ propriété `z-index` activée  
✅ élément crée des coordonnées pour ses enfants  

https://codepen.io/alyra/pen/bGwdmEL

### `position: fixed;`

✅ élément sort du layout  
✅ `top`, `bottom`, `left`, `right` activées  (position par rapport **aux bords de l'écran**)  
✅ propriété `z-index` activée  
✅ élément crée des coordonnées pour ses enfants  

https://codepen.io/alyra/pen/wvzaYgm

### `position: sticky;`

❌ élément sort du layout  
✅ `top`, `bottom`, `left`, `right` activées  (position par rapport à l'ancêtre le plus proche qui est positionné ou html)  
✅ propriété `z-index` activée  
✅ élément crée des coordonnées pour ses enfants

https://codepen.io/alyra/pen/LYRVgLZ

---

Nous allons maintenant mettre tout cela en pratique avec le pen suivant :

https://codepen.io/alyra/pen/abdBvEQ

https://wptemplates.pehaa.com/assets/alyra/position.mp4

