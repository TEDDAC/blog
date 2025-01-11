---
layout: post
title:  "Client, Backend, BDD: Introduction à l'architecture d'une application"
date:   2025-01-11 12:20:13 +0100
categories: architecture
---

Quand on débute, il est compliqué de comprendre comment est faite une application dans sa globalité. Frontend, client, backend... des termes qu'on rencontre beaucoup mais dans lequels on peut vite se perdre. Cette article a pour but de vous présenter la structure des applications modernes.

> Savoir ce qu'est le HTML et le css est important pour cette article.

# Au commencement, HTML, CSS et site statique

Un site web c'est en réalité quelque chose de très simple, mais qui fait déjà intervenir nos deux acteurs principaux, le navigateur et le serveur.

Prenons par exemple l'article que vous êtes actuellement en train de lire. Quand vous arrivez sur cette page, votre navigateur demande au serveur sur lequel le site est hébergé de lui donner le fichier ``architecture-application.html``. On appel ces demandes des **requêtes**. Une fois le fichier reçu, le navigateur interprète le contenu du fichier dans le but de vous l'afficher de manière un peu jolie. Pour styliser les sites web, on n'utilise des fichiers css qu'on vient importer dans le fichier HTML, par exemple ``styles.css``. Quand le navigateur voit l'import du fichier dans le code, il demande au serveur de le lui fournir. La page HTML peut aussi contenir des images, que le navigateur va demander au serveur.

![échanges entre le navigateur et le serveur](./TODO)

C'est toujours la même chose: le navigateur demande un fichier - qu'on appelle aussi ressource - et le serveur lui donne. Puis si la ressource n'existe pas vous avez la fameuse erreur [404](lien_mort)

> Pour les curieux, allez dans les outils développeur de votre navigateur en appuyant sur F12, allez dans l'onglet réseaux, recharger la page. Vous devriez voir toutes les **requêtes** effectuées par votre ordinateur vers le serveur.

Les sites web qui ne font que distribuer des fichiers sont appelés **sites statiques**. Le contenu renvoyé par le serveur est toujours le même, peu importe l'utilisateur ou le moment.

Mais le web serait morose et monotone si on voyait encore et encore les mêmes choses. Puis même côté code HTML, comment faire pour avoir une barre de navigation commune à toutes les pages ? On est obligé de recopier le code de la barre tout le temps ? Donc je dois modifier toutes mes pages pour chaque changement de la barre ?

C'est pour ça qu'on a cherché à dynamiser les sites web, avec des **sites dynamiques**.

# Dynamiser le web avec le SSR

Contrairement à un site web statique, un site dynamique peut faire changer son contenu, c'est le cas par exemple des blogs ou de wikipedia. Quand vous accédez à un [article](https://fr.wikipedia.org/wiki/Page_web_dynamique) wikipedia, l'article à toujours la même structure, le même design, et possède même des éléments en commun avec les autres articles (barre de navigation, menu de l'apparence,...). Pourtant le titre, le contenu et le sommaire de l'article sont propre à l'article. Rentrons un peu plus dans le fonctionnement d'un site dynamique, et dans le début de la programmation serveur, appelé aussi programmation **backend**.

Même principe que pour le site statique, votre navigateur demande une ressource, et le serveur répond. La différence est que dans l'url, on vient spécifier ce qu'on cherche. Par exemple, en rentrant  [https://fr.wikipedia.org/wiki/Page_web_dynamique](https://fr.wikipedia.org/wiki/Page_web_dynamique) dans votre navigateur, ce dernier demande la ressource ``wiki`` au serveur *wikipedia.org*, et précise comme paramètre de la requête qu'il veut l'article identifié par *Page_web_dynamique*. En voyant ça, le serveur de wikipedia va charger la ressource ``wiki`` en y mettant - là où le développeur l'a voulu - les informations de l'article demandé qu'il a récupéré dans une base de données, avant de renvoyer le contenu HTML au navigateur.

![échanges entre le navigateur et le serveur dans un site dynamique](./TODO)

Ce qui dans pourrait ressembler dans le code du serveur à

```html
<html>
  <header>
    <title>{titre}</title>
  </header>
  <body>
    {sommaire}
    ...
    {contenu}
  </body>
</html>
```

Mais c'est complètement transparent pour votre navigateur qui lui reçoit

```html
<html>
  <header>
    <title>Page web dynamique</title>
  </header>
  <body>
    <ul>
      <li>Génération</li>
      <li>Applications</li>
      ...
    </ul>
    ...
    <p>Une <b>page web dynamique</b> est une page...<p>
  </body>
</html>
```

C'est le serveur qui génère la page HTML pour que votre navigateur n'ait plus qu'à afficher, on appel ça le **SSR: Serveur Side Rendering**, ou Rendu Côté Serveur.

Le SSR permet donc via de simples liens hypertexts de passer d'articles en articles et de page en page sur un site internet. Le SSR a été modernisé durant la dernière décennie et reste encore très utilisé de nos jours. (lire: [Le Server Side Rendering (SSR)](https://technologies.insign.fr/articles/les-differents-modes-de-rendu-dun-site-web-1-4))

# Fluidifier la navigation avec le CSR

A partir des années 2010, le web a eu besoin de modernité. Par exemple, le SSR ne permet pas la création de tchat en temps réel, qui sont pourtant omniprésent dans notre sociétés. C'est alors que les développeurs se sont dit

> Et si le contenu était généré par le navigateur, et que le serveur ne répondait plus des pages html, mais simplement les données, que le navigateur mettrait en forme ?

C'est le principe du **CSR: Client Side Rendering**. Dans une application, le **client** - appelé aussi **frontend** - est la partie que va pouvoir manipuler l'utilisateur, c'est donc le code chargé par votre navigateur, ou votre téléphone portable dans le cas d'application mobile.

Par exemple, quand vous accéder à [https://www.youtube.com/watch?v=dQw4w9WgXcQ](https://www.youtube.com/watch?v=dQw4w9WgXcQ), le navigateur demande d'abord le HTML, le CSS, et le Javascript. Une fois chargé, le client (donc votre navigateur) va demander les données de la vidéo *dQw4w9WgXcQ* (titre, description,...) et les mettre en forme dans le navigateur. Ainsi, le serveur ne renvoi plus des pages HTML entières mais des données, ce qui fait que quand vous naviguez pendant plusieurs minutes sur l'application, c'est toujours le même code HTML qui est chargé pour chaque vidéos, mais l'application demande les nouvelles données pour chaque vidéos.

![échanges entre le navigateur et le serveur avec le CSR](./TODO)

Il faut donc penser autrement les échanges client - serveur, et le serveur doit être conçu pour renvoyer des données et non plus des fichiers, on appel ça des **API: Application Programming Interface**. Une API a aussi le gros avantage de permettre à d'autre application de se brancher sur elle, ce qui est très pratique si on a plusieurs application à faire fonctionner ensemble. Exemple: quand vous vous connecter sur un site de bot discord, vous vous connecter avec votre compte, et le site du bot récupère depuis l'API discord les informations de votre compte.

Un autre avantage du CSR est de soulager le serveur, car pour des sites très fréquentés, renvoyer une données est plus léger que de renvoyer une page entière.

# Webographie

Pour aller plus loin et approfondire...

- [Le Client Side Rendering (CSR)](https://technologies.insign.fr/articles/client-side-rendering-csr)
- [Le Server Side Rendering (SSR)](https://technologies.insign.fr/articles/les-differents-modes-de-rendu-dun-site-web-1-4)