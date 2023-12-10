# Créez un fichier docker-compose pour orchestrer vos conteneurs

Un exemple de contenu de fichier ̀`docker-compose.yml` est le suivant :

```YML
version: "3"
services:
  redis:
    image: redis
  application:
    image: dockerfacile/app
    ports:
      - 5000:5000
```

Tout comme lors du `docker pull`, les images n'existant pas en local seront directement récupérées du Docker Hub par Docker Compose, même lors d'un `docker-compose up`.
L'application tourne sur le port 5000: le `5000:5000` désigne le port interne et le port externe.

Si je créer un fichier `docker-compose.yml` avec le contenu ci-dessus, je peux alors lancer `docker-compose up` dans son dossier associé en commande (attention à respecter les indentations yml, plus courtes qu'en markdown).

Le site est censé être visible sur `localhost:5000`, ce qui n'est pas le cas en pratique à cause d'une erreur cheloue du navigateur pas visible en commande, mais tant pis.
