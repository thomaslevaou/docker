# Lancez votre premier conteneur en local

**Docker Hub** est la registry officielle de Docker.
Dans le contexte des conteneurs, on appelle **registry** un logiciel qui permet de partager des images à d'autres personnes.
C'est comme ça que Docker peut distribuer et versionner des images, permettre aux outils de CI de jouer des tests avec uniquement Docker, de déployer de manière automatisée les applications, etc.

La commande `docker images` permet de lister les images disponibles sur mon poste. Au moment où j'écris ces lignes, il n'y en a donc aucune.
`docker ps` liste les conteneurs disponibles sur mon poste (donc pareil, il n'y en a aucune).

Et surtout, la commande `docker pull` va télécharger depuis un dossier existant, ou le Docker Hub, une image existante.

Ce qui implique que la commande `docker pull node`, docker va télécharger l'image du langage node.js (parce oui, les langages en Docker ont des images).
Et donc en refaisant un `docker images`, je vois bien cette image dans la liste de mes images Docker.

Pour faire tourner le _conteneur_ de cette image, on lance `docker run`, avec le paramètre `-it` pour que ça soit interactif :

```bash
docker run -it node
```

Et quand le conteneur est lancé, je peux ouvrir une autre fenêtre et voir avec un `docker ps` que le conteneur tourne sur mon poste.

Pour stopper un conteneur, on peut soit faire `Ctrl + D` dans la fenêtre interactive du conteneur, soit `docker stop {{CONTAINER_ID}}`, par exemple ici `docker stop 0ae73bdf6f35`, ce qui coupe aussi la fenêtre interactive.

On peut lancer le conteneur en arrière-plan (on dit alors qu'il est **détaché**), via le paramètre `-d` :

```bash
docker run -it -d node
```
