# Formulaires <span role="img" aria-label="">📩<span>

Formulaires permettent aux utilisateurs d'interagir avec un site web ou une application. Les formulaires que nous utilisons le plus souvent sont :
- un champ de recherche au sein d'un site web 🔎
- formulaire de contact sur un site web 📩
- formulaire que nous remplissons afin d'effectuer un achat en ligne 🛒

HTML fournit différents éléments permettant de créer des formulaires. Nous allons apprendre comment mettre en place des contrôles interactifs (les champs qui permettent aux utilisateurs d'envoyer des données). Ce qui se passe ensuite avec ces données ne nous concerne pas pour l'instant.

## <code>form</code>

Notre aventure commence avec l'élément `<form>`. `<form>` représente une section qui regroupe les éléments interactifs.

```html
<form action="/ou/envoyer/" method="POST">
</form>
```

Je mentionne ici très brièvement les attributs `action` et `method` :

- `action` - L'URL vers le script qui traitera les données envoyées par le formulaire. Nous serons redirigés vers cette adresse. Si `action` n'est pas spécifié, les données sont envoyées à l'URL de la page contenant le formulaire.
- `method` - comment les données sont envoyées. Dans la majorité des cas, les formulaires utilisent la méthode `GET` (méthode par défaut) ou `POST`. Avec la méthode GET les données sont envoyées via l'URL.  Avec la méthode POST les données sont envoyées dans le body de la requête.

Vous pouvez [en lire davantage ici (MDN).](https://developer.mozilla.org/fr/docs/Web/Guide/HTML/Formulaires/Envoyer_et_extraire_les_donn%C3%A9es_des_formulaires)

### <code>fieldset + legend</code>

`<fieldset>`permet de sectionner le formulaire en  regroupant plusieurs contrôles interactifs. Il est particulièrement utile dans le cas de formulaires complexes ou nous devons récupérer plusieurs types de données (par exemple : personnelles, expérience, mode de contact, etc.)

```html
<form>
  <fieldset>
    <legend>Données personnelles</legend>
    <!-- à venir -->
  </fieldset>
  <fieldset>
    <legend>Expérience web</legend>
    <!-- à venir -->
  </fieldset>
  <fieldset>
    <legend>Mode de contacte</legend>
     <!-- à venir -->
  </fieldset>
</form>
```

![](https://wptemplates.pehaa.com/assets/alyra/fieldset.png)

## Contrôle interactif - <code>input</code>

L'élément vide `<input>` crée un contrôle interactif. Les saisies possibles et son comportement dépendent fortement de la valeur indiquée par son attribut `type`.

```html
<input type="email" id="email" name="email" required />
```

Regardons les attributs souvent utilisés avec `input`

- `type` - par défaut text. Comme vous pouvez l'observer dans la vidéo ci-dessous, le comportement de l'élément `<input>` change grandement en fonction de son attribut `type`.   
Vous trouverez la liste des types disponibles avec des exemples [ici (MDN)](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input)

https://wptemplates.pehaa.com/assets/alyra/input-type.mp4

- `name` - la référence de la donnée utilisée pour le traitement quand le formulaire est envoyé
- `required` - si ajouté le champ devient obligatoire
- `value` - la valeur par défaut, rarement utilisé avec le `type="text"` ou le `type="email"` par contre très pratique pour les type tels que `"number"`, `"color"` ou `"range"`

## input + label 🦜🦜

Chaque contrôle interactif (`input`) devrait être accompagné par un élément [`label`](https://developer.mozilla.org/fr/docs/Web/HTML/Element/label) qui le décrit. Pour que la liaison fonctionne l'attribut `for` de `<label>` doit correspondre à l'attribut `id` de `input`.

```html
<label for="user-name">Votre nom</label>
<input id="user-name" name="user-name" type="text" />
```
![](https://wptemplates.pehaa.com/assets/alyra/labelinput.png)

Pourquoi utiliser `label` ?
- Le texte du `label` est techniquement associé avec le champ. C'est ce qui sera énoncé aux utilisateurs des lecteurs d'écran. 
- Vous pouvez cliquer sur le libellé pour passer le focus ou activer le champ (meilleure expérience utilisateur !)

https://wptemplates.pehaa.com/assets/alyra/label.mp4

### <code>input</code> type text

```html
<label for="user-name">Votre nom</label>
<input id="user-name" name="user-name" type="text" />
```

### <code>input</code> type email 

```html
<label for="user-email">Votre e-mail</label>
<input id="user-email" name="user-email" type="email" />
```

La valeur saisie est validée afin de vérifier si son format correspond à l'adresse mail.

### <code>input</code> type number

```html
<label for="age">Votre d'âge</label>
<input id="age" name="age" type="number" value="20">
```

Nous pouvons aussi configurer la valeur maximale, minimale et l'incrémentation :

```html
<label for="age">Votre tranche d'âge</label>
<input id="age" name="age" type="number" value="20" min="20" max="110" step="10">
```

https://wptemplates.pehaa.com/assets/alyra/input-type-number.mp4

### <code>input</code> type range

```html
<fieldset>
  <legend>Expérience web</legend>

  <label for="html">HTML</label>
  <input id="html" name="html" type="range" value="50" min="0" max="100">

  <label for="css">CSS</label>
  <input id="css" name="css" type="range" value="10" min="0" max="100">

  <label for="js">JavaScript</label>
  <input id="js" name="js" type="range" value="10" min="0" max="100">

</fieldset>
```

![""](https://wptemplates.pehaa.com/assets/alyra/input-type-range.png)

### <code>input</code> type radio

Nous utilisons les éléments `<input type="radio">` pour donner le choix d'une valeur parmi plusieurs. Dans ce cas-là nous pouvons alternativement utiliser l'élément `<select>` dont nous allons aussi parler.

```html
  <fieldset>
    <legend>Mode de contacte</legend>
    <p>Comment préférez-vous être contacté ?</p>
    <label for="par-tel">Par téléphone</label>
    <input id="par-tel" name="mode-contact" type="radio" value="tel">
    
    <label for="par-mail">Par Mail</label>
    <input id="par-mail" name="mode-contact" type="radio" value="mail">
    
    <label for="par-sms">Par SMS</label>
    <input id="par-sms" name="mode-contact" type="radio" value="sms" checked>
    
  </fieldset>
```

Observez que nos trois `input` de type `radio` partagent le même `name="mode-contact"`.  
L'attribut `checked` indique la valeur choisie par défaut. 

![""](https://wptemplates.pehaa.com/assets/alyra/input-type-radio.png)

### <code>input</code> type checkbox

L’élément `<input type=checkbox>` permet de cocher une (ou plusieurs) valeur.

```html
<label for="satisfaction">Cochez si ça vous a plus</label>
<input id="satisfaction" name="satisfaction" type="checkbox">
```

### <code>textarea</code>

`textarea` est un contrôle qui permet d'éditer du texte sur plusieurs lignes. Nous l'utilisons pour le corps de message, un commentaire, etc.

```html
<fieldset>
  <legend>Vos questions à nous</legend>
  <label for="questions">Vos questions</label>
  <textarea id="questions" name="questions" cols="50" rows="6"></textarea>
</fieldset>
```

- `textarea` n'est pas un élément vide. Pour prévoir un contenu par défaut, il faut l'ajouter entre les balises de l'élément (l'attribut `value` n'est pas pris en charge).
- Les attributs `rows` et `cols` permettent de définir la taille de l'élément.

### <code>select</code>

Comme `<input type="radio">`, l'élément `<select>` permet de choisir parmi plusieurs options.

```html
<fieldset>
  <legend>Mode de contacte</legend>
  <p>Comment préférez-vous être contacté ?</p>
  
  <label for="select-mode-contact">Mode Contacte</label>
  <select id="select-mode-contact" name="select-mode-contact">
    <option value="tel">Par téléphone</option>
    <option value="mail">Par mail</option>
    <option value="sms" selected>Par sms</option>
  </select>

</fieldset>
```

- Chaque élément `<option>` doit avoir un attribut `value` (si `value` n'est pas spécifiée explicitement, le texte contenu dans `<option>..</option>` sera utilisé)
- L'attribut `selected` sur un élément <option> indique 'option soit sélectionnée par défaut au chargement de la page.


https://codepen.io/alyra/pen/qBOzGRo

## Exercices

- [Monstre](https://codepen.io/alyra/pen/LYpJrwg) | [solution](https://codepen.io/alyra/pen/6476cabbc1a5a1849f5bb349a4fa4ea0)
- [Form - Dev Freelance](https://codepen.io/alyra/pen/pojOZoP) | [solution](https://codepen.io/alyra/pen/6614f36dcfcc8ae7045f250135dc77e8)

