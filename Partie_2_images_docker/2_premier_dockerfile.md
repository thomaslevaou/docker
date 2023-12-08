# Créez votre premier Dockerfile

Je souhaite maintenant créer ma propre image Docker, pour pouvoir à la fois installer node.js et les dépendances de mon projet.

Pour ce faire, je dois créer un fichier appelé le **Dockerfile**, qui liste les différents éléments dont j'ai besoin pour faire tourner mon projet, à la manière d'un `package.json` en Node.js.

Un Dockerfile liste différentes instructions, chacune créant une nouvelle **layer** ("couche" ou étape) de la construction du projet. Le but est que le nombre d'étages soit le plus possible limité, pour que le résultat soit le plus léger et performant possible.

À la racine du projet qu'on souhaite dockeriser, on crée donc un fichier, appelé juste `Dockerfile`.

La commande `FROM` va indiquer l'image à utiliser pour notre conteneur.
Tandis que `CMD ["commande", "paramètres"]` va exécuter la commande `commande` avec les paramètres `paramètres`. Par exemple les commandes ci-dessous vont télécharger l'image `alpine` dans sa version 3.14, et afficher un `Hello World !` en commande :

```bash
FROM alpine:3.14

CMD ["echo", "Hello World !"]
```

Comme le Dockerfile de ma propre image Docker est maintenant écrit, je peux **construire** mon image, avec la commande `docker build`, agrémentée du paramètre `-t` pour donner un nom à l'image, et d'un `.` pour indiquer qu'on veut construire l'image à partir du dossier courant :

```bash
cd ghost-cms
docker build -t mon_image .
```

L'image est alors visible dans la liste de `docker images`.

Elle peut donc être lancée avec un `docker run mon_image`.
