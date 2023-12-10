# Créez un fichier docker-compose pour orchestrer vos conteneurs

## Fichier docker-compose basique

Un exemple de contenu de fichier ̀`docker-compose.yml`, qui va utiliser la version 3 (la plus utilisée actuellement), pour faire tourner un conteneur redis (un SGBD en gros) et une app quelconque dont l'image existe, est le suivant :

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

Les conteneurs doivent être définis sous l'argument `services`. Chaque conteneur commence avec un nom qui lui est propre, et que je choisis (`application` pour `dockerfacile/app` par exemple).

Ici on crée le conteneur via des images, en utilisant l'argument `image:`, suivi du nom de l'image qu'on veut utiliser. On aurait aussi pu utiliser l'argument `build:` à la place, mais dans ce cas le paramètre doit être le chemin vers le fichier Dockerfile à construire pour avoir notre conteneur.

Tout comme lors du `docker pull`, les images n'existant pas en local seront directement récupérées du Docker Hub par Docker Compose, même lors d'un `docker-compose up`.

L'application du premier ̀`docker-compose.yml` en haut de ce fichier, tourne sur le port 5000: le `5000:5000` désigne le port interne et le port externe.

Si je créer un fichier `docker-compose.yml` avec le contenu ci-dessus, je peux alors lancer `docker-compose up` dans son dossier associé en commande (attention à respecter les indentations yml, plus courtes qu'en markdown).

Le site est censé être visible sur `localhost:5000`, ce qui n'est pas le cas en pratique à cause d'une erreur cheloue du navigateur pas visible en commande, mais tant pis.

## Fichier docker-compose plus enrichi

Attention les conteneurs Docker ne sont pas faits pour faire fonctionner des services stateful par défaut. Mais on peut, si besoin, utiliser l'argument `volumes` pour garder des données dans un disque persistant, comme dans cette partie de fichier docker-compose ci-dessous, où les données sont gardées dans le dossier `/data/mysql` (qui est ici l'équivalent de l'alias `db_data` qu'utilise Docker):

```YML
services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
```

Les données dans `/var/lib/mysql` seront gardées en interne dans `db_data`, c'est-à-dire `/data/mysql`, après arrêt du conteneur, de ce que je comprends (à tester un jour quand même).

Le `restart: always` est là pour dire que si jamais le serveur MySQL rencontre une erreur fatale qui l'arrête, il doit alors se redémarrer automatiquement.

L'argument `environment` permet de spécifier des variables d'environnement, ici celles qui permettent de se connecter à la base de données.

L'argument `depends_on` permet d'indiquer le service qui doit démarrer avant le service courant. Ainsi, si dans le service Wordpress je mets:

```YML
depends_on:
  - db
```

Le service appelé ̀`db` se lancera systématiquement avant le service Wordpress.

Un `docker-compose.yml` un peu plus enrichi que celui en début de chapitre pourrait donc être le suivant :

```YML
version: '3'
services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress

volumes:
  db_data: {}
```

En l'exécutant (`docker-compose up -d`) puis en allant sur `127.0.0.1:8000` de mon navigateur, je tombe bien sur une page Wordpress !
