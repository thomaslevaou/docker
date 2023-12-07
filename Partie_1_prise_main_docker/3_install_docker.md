# Installez Docker sur votre poste

**Docker Hub** est un site spécialisé dans le stockage d'images pour Docker.

Pour installer Docker sur Debian, on va appliquer les commandes suivantes :

```bash
sudo apt-get update && sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"

# Attention ici j'ai jeté un oeil à la doc officielle de Docker, pas Openclassrooms
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo usermod -aG docker thomaslevaou
```

On peut alors exécuter `docker login` puis `newgrp docker` (pour bien indiquer que je suis connecté au groupe docker), et rentrer mes identifiants Docker Hub pour m'y commander en fenêtre de commande.