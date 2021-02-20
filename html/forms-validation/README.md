# Validation des Formulaires <span role="img" aria-label="">✅</span>

C'est le rôle de développeur front-end d'assurer que le format correbt des données soumises par un utilisateur via un formulaire. Nous parlons ici de la validation côté client (_client-side validation_). La validation _client-side_ s'ajoute à la validation côté serveur (_server-side validation_). Elle n'est pas pourtant moins importante et ne devrait pas être négligée. Un des avantages est le feedback instantané envoyé à l'utilisateur sur ses saisies. C'est absolument nécessaire dans le contexte de bonne expérience utilisateur.

Avec l'arrivée de _HTML5_ nous avons la validation _"nativement"_ disponible dans le navigateur 🎉. Nous pouvons spécifier le format souhaité grâce aux attributs HTML. L'élément `form` bloquera l'envoi des données si les formats ne sont pas respectés.

## L'utilisation de la validation "native"

Nous avons à notre disposition :

- **l'attribut `type` pour les contrôles `input`** qui impose le format (par exemple `<input type="email">` impose le format d'adresse mail correcte)

```html
<label for="mail">Votre adresse e-mail</label>
<input id="mail" name="mail" type="email" placeholder="ex. mr@alyra.fr" />
```

- **l'attribut `required`** qui indique que le champ doit être rempli

```html
<label for="mail">Votre adresse e-mail</label>
<input
  id="mail"
  name="mail"
  type="email"
  placeholder="ex. mr@alyra.fr"
  required
/>
```

- **les attributs `minlength` et `maxlength`** qui donnent des contraintes à la longueur des entrées textuelles

```html
<label for="slug">Choisissez votre identifiant (entre 3 et 8 caractères)</label>
<input
  id="slug"
  name="slug"
  type="text"
  placeholder="ex. Alyra"
  minlength="3"
  maxlenght="8"
/>
```

- **les attributs `min` et `max`** pour les entrées numériques

```html
<label for="age">Votre âge</label>
<input id="age" name="age" type="number" value="20" min="18" />
```

- **l'attribut `pattern`** qui permet d'imposer un format en particulier. Nous pouvons utiliser cet attribut avec des contrôles `input` de type `text`, `tel`, `email`, `url`, `password`, et `search`.

Ici nous voulons récupérer un e-mail Alyra. Il est toujours en format `prenom@alyra.fr`

```html
<label for="mail"
  >Votre adresse e-mail chez Alyra (uniquement les membres d'Alyra)</label
>
<input
  id="mail"
  name="mail"
  type="email"
  placeholder="ex. mr@alyra.fr"
  pattern="[a-z]{2,}@alyra.fr"
/>
```

```html
<label for="slug"
  >Choisissez votre identifiant (entre 3 et 8 lettres en minuscule)</label
>
<input
  id="slug"
  name="slug"
  type="text"
  placeholder="ex. Alyra"
  pattern="[a-z]{3,8}"
/>
```

```html
<label for="zip-code-pl">Votre code postal en Pologne (format xx-xxx)</label>
<input
  id="zip-code-pl"
  name="zip-code-pl"
  type="text"
  placeholder="50-306"
  pattern="[0-9]{2}-[0-9]{3}"
/>
```

ou

```html
<label for="zip-code-pl">Votre code postal en Pologne (format xx-xxx)</label>
<input
  id="zip-code-pl"
  name="zip-code-pl"
  type="text"
  placeholder="50-306"
  pattern="/d{2}-/d{3}"
/>
```

## Les symboles souvent utilisés dans l'attribut `pattern`

<table><tbody><tr><td><code>.</code></td><td>Caractère générique : correspond à tout caractère à l'exception de la nouvelle ligne (\n)</td></tr><tr><td><code>(a|b)</code></td><td>a ou b</td></tr><tr><td><code>[abc]</code></td><td>Un de (a ou b ou c)</td></tr><tr><td><code>[^abc]</code></td><td>Aucun de (a ou b ou c)</td></tr><tr><td><code>[a-q]</code></td><td>caractère dans la plage de a à q (minuscules)</td></tr><tr><td><code>[A-Q]</code></td><td>caractère dans la plage de A à Q (majuscules)</td></tr><tr><td><code>[0-7]</code></td><td>Chiffre entre 0 et 7</td></tr><tr><td><code>\d</code></td><td >chiffre décimal</td></tr><tr><td><code>\D</code></td><td>aractère autre qu’un chiffre décimal</td></tr><tr><td><code>\s</code></td><td>tout caractère espace blanc</td></tr><tr><td><code>\S</code></td><td>tout caractère autre qu’un espace blanc</td></tr></tbody></table>

<table><tbody><tr><td><code>*</code></td><td>0 ou plus</td></tr><tr><td><code>{3}</code></td><td>Exactement 3</td></tr><tr><td><code>+</code></td><td>1 ou plus</td></tr><tr><td><code>{3,}</code></td><td>3 ou plus</td></tr><tr><td><code>?</code></td><td>0 ou 1</td></tr><tr><td><code>{3,5}</code></td><td>3, 4 ou 5</td></tr></tbody></table>

## _Client-side_ validation avec JavaScript

Parfois les développeurs préfèrent de ne pas utiliser la validation "native". Il est important de savoir comment la désactiver. Nous ajoutons pour cela l'attribut `novalidate` à l'élément `form`.

```html
<form novalidate>
  <!-- -->
</form>
```

---

## Exercices

- [Form Validation with pattern attribute](https://codepen.io/alyra/pen/gOadKZN)
