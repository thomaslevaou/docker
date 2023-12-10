# Utilisez des images grâce au partage sur le Docker Hub

Maintenant que je sais créer ma propre image, pour la partager je _pourrais_ techniquement partager le Dockerfile aux autres collègues, et leur demander de la créer à l'aide d'un `docker build`. Mais pour économiser du temps, le plus simple est de partager directement l'image en l'envoyant sur mon propre registry, et c'est ce qu'on va faire ici.

Sur le [site du Docker Hub](https://hub.docker.com/), une fois connecté je peux cliquer sur le lien "Create a Repository".
Le nom de l'image va être renseigné dans le champ "Repository Name" (donc je suppose qu'en envoyant mon image sur le registry, je crée un dépôt dessus à la manière d'un dépôt sur GitHub), et on va l'appeler comme dans le cours afin de le suivre plus facilement, autrement dit ici `ocr-docker-build`.
