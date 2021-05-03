# React Google Fonts Widget

![](https://assets.codepen.io/4515922/pangramme_1.jpg)

Vous trouverez ici le modèle à reproduire [alyra-google-fonts-widget.](https://alyra-google-fonts-widget.netlify.app/)

Votre applications utilise [**Developer API de Google Fonts**](https://developers.google.com/fonts/docs/developer_api) afin d'afficher selon le choix de l'utilisateur :

- 10 polices les plus récentes
- 10 polices les plus _trending_
- 10 polices les plus populaires

---

**Chaque de polices** est affichée avec des détails :
 
   - son nom 
   - nombre de variants
   - catégorie (sérif, sans-sérif ...)
   - l'aperçue 
   - le lien vers la police sur [fonts.google.com](https://fonts.google.com)
   
---

**L'aperçu** contient un texte (initialement un [pangramme](https://fr.wikipedia.org/wiki/Pangramme), par exemple _Portez ce whisky au vieux juge blond qui fume._) ce texte peut être modifié par utilisateur.

Le texte d'apperçu est affiché avec la propriété _font-family_ égale à la police en question.

L'utilisateur peut également modifier la taille d'affichage (entre 8px et 48px).

---

**Le lien vers la page de police sur fonts.google.fonts** - attention aux polices dont le nom est composé des plusieurs mots,
par exemple le lien pour Zen Dots est https://fonts.google.com/specimen/Zen+Dots (il contient le symbole `+` entre chaque mot)

---

Afin d'afficher des aperçus, vous devez **charger les polices** google en question.  
Il existe quelques solution pour charger des polices google dynamiquement, par exemple [react-google-font-loader.](https://github.com/jakewtaylor/react-google-font-loader)

---

La mise en page peut être modifiée, elle n'est pas imposée. Tâchez à avoir votre HTML valide et réfléchissez sur l'expérience utilisateur.

## Validation :

- rendu sur GitHub et déploiement sur Netlify
- title, lang, meta, favicons sont modifiées (1/10)
- utilisation correcte d'API Google Fonts (3/10)
- police est affichée correctement (tous les éléments demandés) (1/10)
- les polices affichées sont chargées (2/10)
- le texte est "modifiable" (1/10)
- la taille de polices est modifiable (1/10)
- pas de warning depuis la console dans VSCode ni dans navigateur (1/10)
