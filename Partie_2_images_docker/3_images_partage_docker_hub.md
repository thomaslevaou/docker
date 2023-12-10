# Utilisez des images grâce au partage sur le Docker Hub

Maintenant que je sais créer ma propre image, pour la partager je _pourrais_ techniquement partager le Dockerfile aux autres collègues, et leur demander de la créer à l'aide d'un `docker build`. Mais pour économiser du temps, le plus simple est de partager directement l'image en l'envoyant sur mon propre registry, et c'est ce qu'on va faire ici.

Sur le [site du Docker Hub](https://hub.docker.com/), une fois connecté je peux cliquer sur le lien "Create a Repository".
Le nom de l'image va être renseigné dans le champ "Repository Name" (donc je suppose qu'en envoyant mon image sur le registry, je crée un dépôt dessus à la manière d'un dépôt sur GitHub), et on va l'appeler comme dans le cours afin de le suivre plus facilement, autrement dit ici `ocr-docker-build`.

Pour créer un lien entre notre image docker locale et celle du repo (l'équivalent d'un set remote url avec github), on va utiliser la commande `tag` comme ci-dessous :

```bash
docker tag ocr-docker-build:latest peterkolios/ocr-docker-build:latest
```

Si mon conteneur local n'a pas de nom, je peux aussi mettre l'id retourné à la fin d'un `docker build` à la place du premier `ocr-docker-build:latest` ici.

Je peux ensuite pousser mon image docker sur le repo via un `docker push peterkolios/ocr-docker-build:latest`. Ce qui me permet alors de voir qu'une première version de mon image docker existe maintenant sur [la page du repo associée](https://hub.docker.com/repository/docker/peterkolios/ocr-docker-build/general).

Notons que le `latest` est le "nom" de la version de l'image qu'on veut pousser. On peut mettre un autre nom de version, mais ça ne sera alors pas celui qui sera pullé par défaut pour les gens qui veulent récupérer cette image.
