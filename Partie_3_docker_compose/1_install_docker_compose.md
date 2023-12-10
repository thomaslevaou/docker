# Découvrez et installez Docker Compose

On suppose que je dois déployer un site avec deux conteneurs: un conteneur MySQL, et un conteneur WordPress.
Pour simplifier les déploiements, on va faire appel à un nouvel outil appelé **Docker Compose**. Cet outil va permettre de décrire plusieurs conteneurs comme un ensemble de services, via un fichier yaml.

Docker Compose s'installe sur Linux via la commande `sudo curl -L "https://github.com/docker/compose/releases/download/1.23.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose && sudo chmod +x /usr/bin/docker-compose`.

On peut vérifier s'il s'est bien installé via la commande `docker-compose --version`.

Les commandes pour utiliser Docker Compose sont très proches de celles de Docker. On fera par exemple:

- `docker-compose pull` pour récupérer l'ensemble des images décrites dans un fichier `docker-compose.yml`;
- `docker-compose up` pour lancer un ensemble de conteneurs. Le paramètre `-d` permet de le faire tourner de manière détachée, tout comme dans `docker run`. On appelle **stack** un ensemble de conteneurs Docker lancés via un seul et unique fichier Docker Compose;
- `docker-compose ps` pour vérifier le bon état des conteneurs de la stack lancée;
- `docker-compose logs -f --tail 5` pour afficher les logs des différents conteneurs de façon continue (attention c'est le `-f` qui gère l'affichage en continu du fichier, `--tail` tout seul ne fait qu'afficher les 5 dernière lignes du fichier);
- `docker-compose stop` pour arrêter la stack Docker Compose, mais qui ne supprimera pas les ressources créées par la stack (si on fait un `docker compose up` tout de suite après, la stack redémarre instantanément). Pour détruire l'ensemble des ressources de la stack, on fera `docker-compose down`;
- `docker-compose config` pour valider la syntaxe du fichier yaml (et notamment vérifier l'absence de fautes de frappe).
