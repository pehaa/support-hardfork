# Welcome to the browser

**Web Browser** (en français navigateur web) est un logiciel qui permet de consulter une page web. Il localise, charge et assemble les ressources afin de les rendre accessibles aux utilisateurs (en particulier afficher sur l'écran).

1. Je tape un URL (_Uniform Resource Locator_) dans la barre de l'adresse
1. Le navigateur contacte un serveur DNS (\*\*) afin d'obtenir l'addresse IP (Internet Protocol Address) correspondant à ma requete. Ensuite, il peut arriver que le navigeteur rencontre un problème, comme ceci:

   ```bash
   Could not resolve host: notexistinghost.com
   ```

   ![This site can't be reached](https://wptemplates.pehaa.com/assets/alyra/cantbereached.png)

   Mais si tout va bien, le serveur est localisé est nous pouvons passer à l'étape suivante

1. Le navigateur (client) échange avec le serveur. Ils vont utiliser le protocole HTTP pour communiquer. Le navigateur envoie une requete (_request_) et le serveur répond (_response_).

   Jusqu'à ce moment là, le navigateur a agi en tant qu'un client HTTP :

   header de la response :

   ```bash
   curl -I https://alyra.fr
   ```

   body de la response :

   ```bash
   curl https://alyra.fr
   ```

   A ce stade notre page web demandée est en cette forme-là :

   ```bash
   <!doctype html><html>......</html>
   ```

1. ✨ Rendering ✨  
   Le navigateur utilise son moteur _rendering engine_ afin de parser HTML (en. parse = fr. faire l'analyse grammaticale).

   ```html
   <html>
     <body>
       <h1>HTML</h1>
       <p>Cette semaine nous allons découvrir quelques de ses sécrets.</p>
     </body>
   </html>
   ```

   Avant d'afficher le contenu, le navigateur doit établir deux arborescences DOM (Document Object Model) tree et CSSOM (CSS Object Model) tree.

   Autrement dit, le rendering engine parse HTML et crée sa nouvelle représentation -- DOM tree.

   Ceci n'est pas pourtant suffisant pour l'affichage. Pour définir comment afficher chaque élément du DOM tree, le rendering machine crée un autre objet CSSDOM tree.

   Nous pouvons visualiser[Template](https://codepen.io/pen?template=rNWwGvy)
