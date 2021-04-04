# Projet Gradients

Le temps du projet est arrivé ! Dans ce challenge vous avez pour but de construire une application web _monopage_ [comme celle-ci.](https://alyra-gradients-bonus.netlify.app/)

Vous devez utiliser CRA en tant que starter. Ci-dessous, vous trouverez quelques indications qui vous aideront à structurer votre projet.

## CRA - configuration initiale

```bash
npx create-react-app react-alyra-gradients
```

### Installer Bootstrap5 et mettre en place Bootstrap5

```bash
yarn add bootstrap@next
```

### Head

Configurez la partie head, en particulier :

- title
- lang
- meta

Vous devez aussi remplacer les icônes et configurez `manifest.json`.

## Structure du projet

Mettez en place un dossier `src/components` avec la structure comme ceci. C’est une structure recommandée, mais elle peut varier un peu 😉.

```bash
src
├── App.js
├── App.test.js
├── components
│   ├── Footer.js
│   ├── Gradient
│   │   ├── GradientCode.js
│   │   ├── GradientPill.js
│   │   ├── GradientTagButton.js
│   │   ├── GradientTags.js
│   │   ├── GradientTitle.js
│   │   ├── gradient.css
│   │   └── index.js # (*)
│   ├── Gradients.js # (**)
│   ├── GradientsHeader.js
│   ├── GradientsList.js
│   └── GradientsSelect.js
├── gradients.js
├── index.css
├── index.js
├── serviceWorker.js
└── setupTests.js
```

(`*`) - contient le component `Gradient`  
(`**`) - component parent, contient `GradientsList` et `GradientsSelect`

### src/gradients.js

Ce fichier devrait exporter les variables `gradients` et `uniqueTags`. Vous pouvez vous [le contenu de ce fichier](https://codepen.io/alyra/pen/73e755888ff0a8fa07d0561d108537ac.js)

### src/components/GradientPill/gradient.css

Le fichier `src/components/Gradient/gradient.css` devrait contenir

```css
.card-gradient {
  width: 5rem;
  height: 5rem;
}
code {
  color: grey;
}
```

ce fichier devrait être importé dans `src/components/GradientPill.js`

### src/App.js

Mettez en place `GradiensHeader`, `Gradients` et `Footer` dans `src/App.js`

## Migration depuis CodePen

Utilisez le code des exercices CodePen et migrez le tout dans votre app.

## Filter par tags

Mettez en place un component `GradientsSelect` avec le filtre par tag.

## Tag Buttons

Comme sur l'image ci-dessous, listez des tags pour chaque dégradé. Un tag devrait être rendu en tant qu'un `button`. L'évènement "click" devrait déclencher le filtrage par tag en question (sur le [site de référence](https://alyra-gradients-bonus.netlify.app/) cliquez le bouton 'gris' et observez ce qui se passe).

![](https://wptemplates.pehaa.com/assets/alyra/gradients-tags.png)

---

**Vous pouvez voir le site de référence [ici](https://alyra-gradients-bonus.netlify.app/)**

---

## Validation :

- rendu sur GitHub et déploiement sur Netlify
- bootstrap5 est installé avec yarn (utilisation de bootstrap depuis CDN n'est pas valide)
- title, lang, meta, favicons, manifest sont modifiées
- la structure est respectée
- le header avec le bouton est fonctionnel
- les dégradés sont affichés dans la page
- le select fonctionne et permet de filter des dégradés par tag
- il n'y a pas de warning depuis la console dans VSCode, ni dans la console du navigateur
- [le html est valide (validator w3c)](https://validator.w3.org/nu/)
