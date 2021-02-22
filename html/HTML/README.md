# HTML - Introduction

**HTML** _HyperText Markup Language_ est un langage de balises permettant :

- de représenter le contenu d'une page web et de le structurer
- d'avoir de l'hypertexte dans un navigateur web.

Il coexiste avec deux autres technologies Web :

- **CSS** pour décrire la présentation visuelle
- **JavaScript** pour des fonctionnalités interactives

**HTML** est un standard définit par [WHATWG (_Web
Hypertext Application Technology Working Group_)](https://whatwg.org/).

**HTML** est un language _déclaratif_ (en opposition aux languages impératifs)

> Les pages HTML sont déclaratives car elles décrivent ce que contient une page (texte, titres, paragraphes, etc.) et non comment les afficher. Alors qu'en programmation impérative, on décrit le comment, c'est-à-dire la structure de contrôle correspondant à la solution (source [Wikipedia](https://fr.wikipedia.org/wiki/Programmation_d%C3%A9clarative))

```javascript
/* javascript */
const p = document.createElement("p")
p.textContent = "Salut !"
document.body.append(p)
const img = new Image()
img.src = "panda-roux.jpg"
img.alt = "Panda roux sur une branche"
document.body.append(img)
```

```html
<!-- html -->
<body>
  <p>Salut !</p>
  <img src="panda-roux.jpg" alt="Panda roux sur une branche" />
</body>
```

HTML utilise des **balises** (en anglais _tags_) qui sont insérées au sein d'un texte. Par exemple, chaque paragraphe est encadré par une balise paragraphe (`p`). Le titre principal sera encadré par la balise `h1`. L'ensemble, le titre + les paragraphes autour d'un sujet seront encadrés par une balise `article`.

![](https://wptemplates.pehaa.com/assets/alyra/text-pandaroux.png)

![](https://wptemplates.pehaa.com/assets/alyra/html-pandaroux.png)

https://codepen.io/alyra/pen/bGeywNy

https://codepen.io/alyra/pen/MWbvVjp

## Métaphore

![rangement ikea](https://s3-us-west-2.amazonaws.com/s.cdpn.io/4515922/wardrobe.jpg)

J'aime comparer la structure HTML d'un document web à un rangement Ikea. Les balises sont des placards, des étagères, des tiroirs, des boîtes, des cintres...

- Chaque élément à son usage spécifique (comme une balise a son sens sémantique). Nous n'accrochons pas des chaussettes sur les cintres et nous ne mettons pas des costumes dans les tiroirs.
- Les éléments sont emboîtés l'un dans l'autre - mais pas tout à fait librement - par exemple, nous ne mettons pas de cintres dans les tiroirs. Pareil, dans HTML nous n'encadrons pas un titre par un paragraphe.
- Les éléments du même type peuvent avoir leurs spécificités (par exemple un tiroir magnétique). Nous pouvons aussi leur donner des étiquettes (accrocher un autocollant chaussettes 🧦 sur un tiroir, marqués certains placards "hiver", etc.)

## Anatomie d'un élément HTML

```html
<nomdebalise>contenu de l'élément</nomdebalise>
```

![](https://wptemplates.pehaa.com/assets/alyra/balises-html1.png)

Les éléments HTML peuvent aussi avoir des attributs (caractéristiques et étiquettes).  
L'ordre dans lequel nous listons des attributs n'a pas d'importance. Les valeurs devraient être encadrées par des guillemets (`""`).

```html
<nomdebalise attribut1="sa valeur" attribut2="sa valeur">
  contenu de l'élément
</nomdebalise>
```

![](https://wptemplates.pehaa.com/assets/alyra/balises-html2.png)

Il existe aussi des éléments qui n'ont pas de contenu, des éléments vides. Dans ce cas-là, nous allons utiliser des balises _autofermantes_ (en anglais _self-closing tags_).  
Les exemples les plus simples des éléments vides sont :

- `<br />` - saut à la ligne (`<br>` est aussi correcte),
- `<hr />` - la ligne horizontale (`<hr>` est aussi correcte),

L'élément vide le plus souvent utilisé est `img`. La balise `img` permet d'afficher des images 🌄. L'élément `<img>` a toujours deux attributs

- `src` - la source d'image à afficher
- `alt` - le texte qui décrit le contenu de l'image (texte alternatif).

```html
<img src="path/to/myimage.png" alt="Le contenu de mon image" />
```

https://codepen.io/alyra/pen/zYBQoWe

https://codepen.io/alyra/pen/MWbvVjp

## Structure du document HTML

```html
<!DOCTYPE html>
<html>
  <head>
    <!-- Métadonnées du document -->
  </head>
  <body>
    <!-- Contenu du document -->
  </body>
</html>
```

```html
<!DOCTYPE html>
<html lang="fr">
  <head>
    <meta charset="utf-8" />
    <title>Mon Document</title>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <!-- Document metadata -->
  </head>
  <body class="full light">
    <header id="top">
      <h1>Bienvenue</h1>
    </header>
    <main>
      <!-- ... contenu principal ... -->
    </main>
    <footer>
      <a href="#top">Retour vers le haut</a>
    </footer>
  </body>
</html>
```

Revenons à notre métaphore "rangement Ikea". Le `body` du document html correspond au rangement lui-même. Mais serons-nous capables de l'assembler sans sa notice (partie `head` du document html) ?

L'élément HTML `<head>` fournis des informations générales (métadonnées) sur le document. Les plus importants sont :

- le titre du document
- liens ou des définitions vers des scripts et feuilles de style
- le jeu de caractères utilisé

<a href="https://s3-us-west-2.amazonaws.com/s.cdpn.io/4515922/removehead.mp4" target="_blank" rel="noreferrer noopener">Voici ce qui se passe quand on enlève la partie head.</a>

## Classement des balises HTML

Ci-dessous vous trouverez les principales balises HTML **classées par le contexte d'utilisation.**

Attention: Cette liste n'est pas 100% complète. Dans un objectif de clarté, certaines balises rarement utilisées, obsolètes ou "expérimentales" ne sont pas incluses. Vous pouvez en lire davantage dans la ["Référence des éléments HTML"](https://developer.mozilla.org/fr/docs/Web/HTML/Element)

### Racine principale (root)

`html`

### Métadonnées du document

`head`  
`base`  
`link`  
`style`  
`title`  
`meta`

### Racine de sectionnement

`body`

### Sectionnement du contenu

**Objectif** : donner une structure logique au document !

`main`  
`section`  
`article`  
`header`  
`h1, ... h6`  
`footer`  
`aside`  
`nav`

### Organisation du contenu textuel

**Objectif** : identifier le sens du contenu !

`p`  
`blockquote`  
`ul` `ol` `li`
`dl` `dt` `dd`  
`figure` `figcaption`  
`pre`  
`main`  
`div`

### Organisation des fragments au sein d'une ligne de texte

**Objectif** : identifier la signification ou la mise en forme

`a`  
`strong` `b`  
`em` `i`  
`br`  
`code`  
`small`  
`span`  
`sub` `sup` (par exemple : H<sub>2</sub>O et m<sup>3</sup>)  
`time`  
`del` `ins` (par exemple : Cette formation durera <del>5 mois</del> <ins>6 mois</ins>).

### Images, Multimedia et Embedded Content

**Objectif** : afficher les images et multimédias, intégrer le contenu _externe_

`img`  
`picture`  
`source`  
`audio`  
`video`  
`track`  
`iframe`

### Scripts

**Objectif** : Gérer l'exécution des scripts

`canvas`  
`script` et `noscript`

```html
<script>
  alert("Hello JavaScript Lovers!")
</script>
<noscript
  >Pour recevoir nos salutations, veillez activer JavaScript dans votre
  navigateur :)</noscript
>
```

Vous pouvez voir le code ci-dessus en action en cliquant [ce lien](https://cdpn.io/alyra/debug/1a70873c7b9713e08a6d00b45ee1b0ab) et ensuite en désactivant JavaScript dans votre navigateur et en rechargeant la page.

### Tables

**Objectif** : Mettre le contenu dans les cases 😉

`table`  
`thead` `tbody` `tfoot`  
`tr` `td` `th`  
`colgroup` `col`

### Formulaires

**Objectif** : Envoyer des données !!!

`form`  
`fieldset`  
`legend`  
`label`  
`button`  
`input`  
`select`  
`optgroup`  
`option`  
`progress`  
`textarea`

### Éléments interactifs

`details` + `summary`

### Éléments obsolètes ⚠️

**Attention** - évitez à les utiliser

`acronym`
`applet`
`basefont`
`bgsound`
`big`
`blink`
`center`
`command`
`content`
`dir`
`element`
`font`
`frame`
`frameset`
`image`
`isindex`
`keygen`
`listing`
`marquee`
`menuitem`
`multicol`
`nextid`
`nobr`
`noembed`
`noframes`
`plaintext`
`shadow`
`spacer`
`strike`
`tt`
`xmp`

## Catégories de contenu

Chaque élément HTML est membre d'un certain nombre de catégories de contenu (par exemple _phrasing content_ ou _interactive content_). Il existe plusieurs règles qui sont basées sur ce classement. En particulier, certains éléments peuvent contenir uniquement des éléments appartenant à la classe _contenu phrasé_. [Plus de détails sur MDN](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Content_categories)

### Flow content

Eléments qui contiennent texte ou embedded content : `a`, `abbr`, `address`, `article`, `aside`, `audio`, `b`, `bdo`, `bdi`, `blockquote`, `br`, `button`, `canvas`, `cite`, `code`, `command`, `data`, `datalist`, `del`, `details`, `dfn`, `div`, `dl`, `em`, `embed`, `fieldset`, `figure`, `footer`, `form`, `h1`, `h2`, `h3`, `h4`, `h5`, `h6`, `header`, `hgroup`, `hr`, `i`, `iframe`, `img`, `input`, `ins`, `kbd`, `keygen`, `label`, `main`, `map`, `mark`, `math`, `menu`, `meter`, `nav`, `noscript`, `object`, `ol`, `output`, `p`, `picture`, `pre`, `progress`, `q`, `ruby`, `s`, `samp`, `script`, `section`, `select`, `small`, `span`, `strong`, `sub`, `sup`, `svg`, `table`, `template`, `textarea`, `time`, `ul`, `var`, `video`, `wbr` et texte.

### Phrasing content (contenu phrasé)

`a` (s'il contient lui-même _phrasing content_)  
 `abbr` `audio` `b` `bdo` `br` `button` `canvas` `cite` `code` `command` `data` `datalist` `dfn` `em` `embed` `i` `iframe` `img` `input` `kbd` `keygen` `label` `mark` `math` `meter` `noscript` `object` `output` `picture` `progress` `q` `ruby` `samp` `script` `select` `small` `span` `strong` `sub` `sup` `svg` `textarea` `time` `var` `video` `wbr` et texte.

Une des balises que nous utilisons particulièrement souvent est `p` (paragraphe). Il est important de savoir que pour un paragraphe le seul contenu autorisé est contenu phrasé.

```html
<!-- ceci est correct (et vrai) -->
<p>Ça se complique, hein ? Ne vous inquiétez pas, on va continuer doucement en veillant que vous appreniez de bonnes pratiques.</p>

<!-- ceci n'est pas correct (et faux) -->
<p><div>Pfff, c'est trop compliqué.</div></p>
```

Regardons ensemble, le code dans le pen suivant. Pourquoi le texte n'est pas écrit en rouge tomate ??? 🤔

https://codepen.io/alyra/pen/QWjJzRB

**Comment savoir ?**

- Suivre la spécification - [MDN - Référence des éléments HTML](https://developer.mozilla.org/fr/docs/Web/HTML/Element)
- [Veiller à toujours valider son document HTML](https://validator.w3.org/)

### Ressources

- [MDN - Référence des éléments HTML](https://developer.mozilla.org/fr/docs/Web/HTML/Element)
- [HTML Living Standard](https://html.spec.whatwg.org/multipage/)
- [HTML Reference](https://htmlreference.io/)
- [CodePen Template with simplified DOM visualisation](https://codepen.io/pen?template=rNWwGvy)
- [HTML Tree Generator](https://chrome.google.com/webstore/detail/html-tree-generator/dlbbmhhaadfnbbdnjalilhdakfmiffeg)

---

## Exercices (2 jours)

- [Lecture obligatoire (MDN)](https://developer.mozilla.org/fr/docs/Apprendre/Commencer_avec_le_web/Les_bases_HTML)
- [Sac de voyage](https://codepen.io/alyra/pen/KKNvRYb)
- [Trottinette](https://codepen.io/alyra/pen/GRNvBpL)
- [Personnages Star Wars](https://codepen.io/alyra/pen/vYyJzBE)
