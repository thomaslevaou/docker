# Entraînez-vous en orchestrant vos images Docker avec Docker Compose

## Dockerfile

Dockerfile que j'ai fait avant de voir la correction:

```bash
FROM debian:11

RUN apt-get update \
    && apt-get -y upgrade \
    && apt install -y python3-pip

ADD . /app/
WORKDIR /app
RUN pip install -r requirements.txt

EXPOSE 5000

CMD python3 main.py
```

Après correction:

```bash
# base image
FROM python:3.8-alpine3.14

# copy just the requirements.txt first to leverage Docker cache
# install all dependencies for Python app
# C'est la "technique pour optimiser le temps de chargement" dont il parlait quelques chapitres plus tôt btw
COPY ./requirements.txt /app/requirements.txt

WORKDIR /app

# install dependencies in requirements.txt
RUN pip install -r requirements.txt

# copy all content to work directory /app
COPY . /app

# specify the port number the container should expose
EXPOSE 5000

# run the application
CMD ["python", "/app/main.py"]
```

On pouvait mettre juste une image de Python 3 et ça suffisait, Docker peut ensuite partager les ressources avec le reste.
Et c'est plus propre d'utiliser `CMD` avec les crochets

## Docker Compose

Fichier docker-compose avant correction :

```YML
version: "3"
services:
  redis:
    image: redis
  application:
    image: exo-python
    ports:
      - 5000:5000
```

après correction:

```YML
version: "3"
services:
  redis:
    image: redis
    ports:
      - 6379:6379
  api:
    image: exo-python
    ports:
      - 5000:5000
    depends_on:
      - redis
```

Ne pas oublier depends_on !