# Welcome to the browser

**Web Browser** (en français navigateur web) est un logiciel qui permet de consulter une page web. Il localise, charge et assemble les ressources afin de les rendre accessibles aux utilisateurs (en particulier afficher sur l'écran).

1. Je tape un URL (_Uniform Resource Locator_) dans la barre de l'adresse

   ***

1. Le navigateur contacte un serveur DNS (_Domain Name System_) afin d'obtenir l'addresse IP (_Internet Protocol Address_) correspondant à ma requete. Ensuite, il peut arriver que le navigeteur rencontre un problème, comme ceci:

   ```bash
   Could not resolve host: notexistinghost.com
   ```

   ![This site can't be reached](https://wptemplates.pehaa.com/assets/alyra/cantbereached.png)

   Mais si tout va bien, le serveur est localisé est nous pouvons passer à l'étape suivante

   ***

1. Le navigateur (client) échange avec le serveur. Ils vont utiliser le protocole HTTP pour communiquer. Le navigateur envoie une requete (_request_) et le serveur répond (_response_).

   Jusqu'à ce moment là, le navigateur a agi en tant qu'un client HTTP. Si vous avez `curl` installé (ex. `brew install curl` pour MacOS) vous pouvez executer les 2 dernières étapes avec la ligne de commande :

   **header de la response :**

   ```bash
   curl -I https://alyra.fr
   ```

   **body de la response :**

   ```bash
   curl https://alyra.fr
   ```

   A ce stade notre page web demandée est de cette forme-là :

   ```bash
   <!doctype html><html>......</html>
   ```

   ***

1. ✨ Rendering ✨  
   Le navigateur utilise son moteur de rendu _rendering engine_ afin de parser HTML (en. parse = fr. faire l'analyse grammaticale).

   ```html
   <!DOCTYPE html>
   <html>
     <body>
       <h1>HTML</h1>
       <p>Cette semaine nous allons découvrir quelques de ses sécrets.</p>
     </body>
   </html>
   ```

   1. Avant d'afficher le contenu, le navigateur doit établir deux arborescences **DOM (_Document Object Model_) tree** et **CSSOM (_CSS Object Model_) tree**.

      Autrement dit, le rendering engine parse HTML et crée sa nouvelle représentation - DOM tree.

   1. Ceci n'est pas pourtant suffisant pour l'affichage. Pour définir comment afficher chaque élément du DOM tree, le rendering machine crée un autre objet CSSDOM tree.

   1. Le navigateur assemble DOM tree et CSSOM tree (_render tree_) en prenant en compte uniquement des éléments visibles.

   1. Le navigateur calcule les dimensions de chaque élément - (étape de _layout_ ou _reflow_).

   1. Ensuite il passe à l'étape de _paint_ affichage de chaque pixel de l'écran.

   ***

## Rendering engines

En ce moment, les navigateurs utilisent principalement une des 3 rendering engines: Blink, Webkit ou Gecko.

![](https://wptemplates.pehaa.com/assets/alyra/browser-engines.png)
