# Créez votre premier Dockerfile

Je souhaite maintenant créer ma propre image Docker, pour pouvoir à la fois installer node.js et les dépendances de mon projet.

## Image Docker "Hello World de base"

Pour ce faire, je dois créer un fichier appelé le **Dockerfile**, qui liste les différents éléments dont j'ai besoin pour faire tourner mon projet, à la manière d'un `package.json` en Node.js.

Un Dockerfile liste différentes instructions, chacune créant une nouvelle **layer** ("couche" ou étape) de la construction du projet. Le but est que le nombre d'étages soit le plus possible limité, pour que le résultat soit le plus léger et performant possible.

À la racine du projet qu'on souhaite dockeriser, on crée donc un fichier, appelé juste `Dockerfile`.

La commande `FROM` va indiquer l'image à utiliser pour notre conteneur. Celle-ci n'est utilisable qu'une seule fois dans un Dockerfile.

Tandis que `CMD ["commande", "paramètres"]` va exécuter la commande `commande` avec les paramètres `paramètres`. Par exemple les commandes ci-dessous vont télécharger l'image `alpine` dans sa version 3.14, et afficher un `Hello World !` en commande :

```bash
FROM alpine:3.14

CMD ["echo", "Hello World !"]
```

Ce Dockerfile est basique, mais on va en recréer un un peu plus riche.

## Image Docker typique d'une utilisation dans un projet

L'instruction `RUN` permet d'exécuter une commande dans le conteneur. Attention au fait que cette commande crée une nouvelle **layer** dans le projet. Elle doit donc être utilisée le moins possible.

Pour copier ou télécharger des fichiers en local dans l'image (il faut bien faire ça pour que les autres développeurs récupèrent les bons fichiers en installant l'image), on utilise l'intruction `ADD`.
La commande `WORKDIR` permet de changer le répertoire courant (équivalent de `cd` en shell).
Et `EXPOSE` permet d'indiquer le port sur lequel votre application écoute.
L'instruction `VOLUME` permet d'indiquer quel répertoire vous voulez partager avec votre host.

Avec ces commandes, on peut donc créer le Dockerfile un peu plus riche qu'est celui ci-dessous :

```bash
FROM debian:11

RUN apt-get update -yq \
    && apt-get upgrade -yq \
    && apt-get install curl gnupg -yq \
    && curl -sL https://deb.nodesource.com/setup_10.x | bash \
    && apt-get install -y nodejs npm \
    && apt-get clean -y

ADD . /app/
WORKDIR /app
RUN npm install

EXPOSE 2368
VOLUME /app/logs

CMD npm run start
```

Attention à mettre Debian 11, la 9 est obsolète et ne fait plus tourner les commandes du RUN ci-dessus ! Et j'ai du préciser explicitement l'installation de `npm`, qui ne l'était pas (probablement par erreur ?) sur Openclassrooms.

On peut créer un ficher `.dockerignore`, à placer à côté du Dockerfile, pour que Docker ignore l'ajout de certains fichiers du dossier indiqué en paramètre lors d'un `ADD`. Son contenu ici sera le suivant :

```bash
node_modules
.git
```

## Construction de l'image

Comme le Dockerfile de ma propre image Docker est maintenant écrit, je peux **construire** mon image, avec la commande `docker build`, agrémentée du paramètre `-t` pour donner un nom à l'image, et d'un `.` pour indiquer qu'on veut construire l'image à partir du dossier courant :

```bash
cd ghost-cms
docker build -t mon_image .  # ou ̀`docker build -t ocr-docker-build .` pour les images un peu plus compliquées
```

Attention Docker créé alors **un conteneur pour chaque instruction** (donc pour chaque ̀`RUN`, et donc chaque layer, de ce que je comprends). Docker ne reconstruit pas une image qui n'a pas bougé par rapport au build précédent.

L'image est alors visible dans la liste de `docker images`.

Elle peut donc être lancée avec un `docker run mon_image` (ou `docker run -d -p 2368:2368 ocr-docker-build` pour lancer un conteneur détaché sur le port 2368, mais ça ne marche pas à cause d'un problème de dépendances du `package.json` du projet, mais tant pis je ne prends pas le temps de déboguer ça plus en détail; et je me déboguerai pour le faire marcher seulement si on en a vraiment besoin après, sinon osef).

Pour déboguer mon `sudo apt-get update`, qui me dit que je manque de fichier `Release` (dans ma commande perso):

```bash
cd /etc/apt/sources.list.d
sudo rm nilarimogard-ubuntu-webupd8-mantic.list*
```
