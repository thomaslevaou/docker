# Lancez votre premier conteneur en local

## Installation d'images

**Docker Hub** est la registry officielle de Docker.
Dans le contexte des conteneurs, on appelle **registry** un logiciel qui permet de partager des images à d'autres personnes.
C'est comme ça que Docker peut distribuer et versionner des images, permettre aux outils de CI de jouer des tests avec uniquement Docker, de déployer de manière automatisée les applications, etc.

La commande `docker images` permet de lister les images disponibles sur mon poste. Au moment où j'écris ces lignes, il n'y en a donc aucune.
Le paramètre `-a` ou `-all` affiche toutes les images (la commande par défaut cache les _images intermédiaires_), mais osef pour le moment.

`docker ps` liste les conteneurs disponibles sur mon poste (donc pareil, il n'y en a aucune).

Et surtout, la commande `docker pull` va télécharger depuis un dossier existant, ou le Docker Hub, une image existante, sans lancer le conteneur associé.

Ce qui implique que la commande `docker pull node`, docker va télécharger l'image du langage node.js (parce oui, les langages en Docker ont des images).
Et donc en refaisant un `docker images`, je vois bien cette image dans la liste de mes images Docker.

## Lancement et arrêt d'un conteneur

Pour faire tourner le _conteneur_ de cette image, on lance `docker run`, avec le paramètre `-it` pour que ça soit interactif :

```bash
docker run -it node
```

Et quand le conteneur est lancé, je peux ouvrir une autre fenêtre et voir avec un `docker ps` que le conteneur tourne sur mon poste.

Pour stopper un conteneur, on peut soit faire `Ctrl + D` dans la fenêtre interactive du conteneur, soit `docker stop {{CONTAINER_ID}}`, par exemple ici `docker stop 0ae73bdf6f35`, ce qui coupe aussi la fenêtre interactive.

On peut lancer le conteneur en arrière-plan (on dit alors qu'il est **détaché**), via le paramètre `-d` ou `--detach`:

```bash
docker run -it -d node
```

Il se stoppe de la même manière qu'un conteneur attaché.

En général, une bonne installation de Docker se vérifier avec la commande `docker run hello-world`. L'image `hello-world` est installée en local si Docker la trouve, sinon, il s'occupe d'aller la récupérer sur Docker Hub.

Le conteneur `hello-world` ne fait que s'allumer, afficher du contenu, puis s'éteindre. Il n'y a donc pas d'intérêt à utiliser le paramètre `-it` pour celui-ci.

## Interactivité a posteriori

Je ne suis pas obligé de savoir dès le départ si j'ai besoin d'une fenêtre interactive au lancement du conteneur ou non. En prenant l'id retourné lors du lancement d'un conteneur détaché, je peux aussi ouvrir sur demande une fenêtre interactive à l'aide d'un `docker exec -ti ID_RETOURNÉ_LORS_DU_DOCKER_RUN bash`, ce qui m'ouvre une fenêtre de commande bash en root dans mon conteneur.

Je peux donc par exemple lancer un conteneur nginx détaché sur le port 80 (le changement de port pouvant être effectué via le paramètre `-p`), puis ouvrir un terminal Bash dans une commande séparée:

```bash
docker run -d -p 8080:80 nginx # Comme pour hello-world, l'image est téléchargée depuis Docker Hub toute seule
docker exec -it 59be6057049bc3394c09ae0a1cc1057c74fb465cf0cb8a86308b9e5f33dc9b04 bash # -it ou -ti c'est pareil ofc
```

Notons que dans ce cas, le conteneur tourne toujours même après interruption du terminal.
Tant que le conteneur est lancé, je peux voir une page affichant un succès de l'installation de nginx sur `http://127.0.0.1:8080`.

Si je veux modifier le contenu de cette page, je dois installer vim, puis accéder au fichier dans le bon dossier :

```bash
apt-get update
apt-get install vim
vim /usr/share/nginx/html/index.html
```

Une fois le fichier modifié, ses nouvelles modifications sont visibles immédiatement après un rechargement de la page dans le navigateur.

## Suppression d'un conteneur et nettoyage système

Une fois qu'il est stoppé, e peux carrément supprimer un conteneur, avec la commande `docker rm {{ID_du_RUN}}` (ça a l'air d'être le même id que le `CONTAINER ID` du `docker ps`).
Par exemple ici, en faisant :

```bash
docker stop 59be6057049bc3394c09ae0a1cc1057c74fb465cf0cb8a86308b9e5f33dc9b04
docker rm 59be6057049bc3394c09ae0a1cc1057c74fb465cf0cb8a86308b9e5f33dc9b04
```

Je détruis le conteneur **et aussi son contenu** (je pense que la modification YOLOO que j'ai faite dans le fichier html, est perdue à jamais).

Pour supprimer les conteneurs qui ne tournent pas, les réseaux non utilisés par des conteneurs, les images non utilisées et les caches de création d'images, je peux utiliser la commande `docker system prune`. On appelle ceci un **nettoyage du système**.