# Welcome to the browser

**Web Browser** (en français navigateur web) est un logiciel qui permet de consulter des pages web. Il localise, charge et assemble les ressources afin de les rendre accessibles aux utilisateurs (en particulier afficher sur l'écran).

1. Je tape un URL (_Uniform Resource Locator_) dans la barre de l'adresse.

   ***

1. Le navigateur contacte un serveur DNS (_Domain Name System_) afin d'obtenir l'adresse IP (_Internet Protocol Address_) correspondant à ma requête.

Ensuite, il peut arriver que le navigateur rencontre un problème, per exemple comme ceci:

```bash
Could not resolve host: notexistinghost.com
```

![This site can't be reached](https://wptemplates.pehaa.com/assets/alyra/cantbereached.png)

Mais si tout va bien, le serveur est localisé et nous pouvons passer à l'étape suivante.

---

1. Le navigateur (client) échange avec le serveur. Ils vont utiliser le protocole HTTP (_Hypertext Transfer Protocol_) pour communiquer. Le navigateur envoie une requête (_request_) et le serveur répond (_response_).

   Jusqu'à ce moment là, le navigateur a agi en tant que simple client HTTP. Si vous avez `curl` installé (ex. `brew install curl` pour MacOS) vous pouvez executer les 2 dernières étapes avec la ligne de commande :

   **header de la response :**

   ```bash
   curl -I https://alyra.fr
   ```

   **body de la response :**

   ```bash
   curl https://alyra.fr
   ```

   À ce stade notre page web demandée est de cette forme-là :

   ```bash
   <!doctype html><html>......</html>
   ```

   ***

1. ✨ Rendering ✨  
   Le navigateur utilise son moteur de rendu (_rendering engine_) afin de parser HTML (en. parse = fr. faire l'analyse grammaticale).

   ```html
   <!DOCTYPE html>
   <html lang="fr">
     <head>
       <meta charset="utf-8" />
       <title>HTML - premiers pas</title>
       <link href="style.css" rel="stylesheet" />
     </head>
     <body>
       <h1>HTML - premiers pas</h1>
       <p>
         Cette semaine nous allons <b>découvrir quelques-uns de ses secrets.</b>
       </p>
     </body>
   </html>
   ```

   1. **DOM :** Le rendering engine parse HTML et crée sa nouvelle représentation - **DOM (_Document Object Model_) tree.**
      ![](https://wptemplates.pehaa.com/assets/alyra/dom1.png)

   2. **CSSOM :** Pour définir comment _afficher_ chaque élément du DOM tree, le rendering machine crée un autre objet **CSSOM (_CSS Object Model_) tree.**

      ```css
      /* style.css */
      body {
        font-family: sans-serif;
      }
      b {
        color: magenta;
      }
      ```

      ![](https://wptemplates.pehaa.com/assets/alyra/cssom.png)

   3. **Render tree :** Le navigateur assemble DOM tree et CSSOM tree en prenant en compte uniquement des éléments visibles (_render tree_).

      ![](https://wptemplates.pehaa.com/assets/alyra/render-tree.png)

   4. **Layout :** Le navigateur calcule les dimensions de chaque élément - (cette étape est aussi appelée _reflow_).

   5. **Paint :** Ensuite il passe à l'étape de _paint_ - remplissage de pixels. Il s'agit de dessiner du texte, des couleurs, des images, des bordures et des ombres, essentiellement toutes les parties visuelles des éléments. Le dessin est généralement effectué sur plusieurs couches (_layers_).

   6. **Compositing :** Les _layers_ sont dessinées à l'écran dans le bon ordre (les éléments au-dessus et en-dessous, axe Z).

   ***

## Rendering engines

En ce moment, les navigateurs utilisent principalement une des 3 rendering engines: Blink, Webkit ou Gecko.

![](https://wptemplates.pehaa.com/assets/alyra/browser-engines.png)

![](https://wptemplates.pehaa.com/assets/alyra/statcounter-browsers.png)
