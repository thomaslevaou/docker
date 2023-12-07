# Découvrez les conteneurs

Les conteneurs sont des sortes de VM qu'on crée dans notre ordinateur (pour un conteneur, on parlera _d'enveloppe virtuelle_). Ils fonctionnent sur le même principe que les VM (des instances qui s'isolent du reste de la machine pour faire tourner un code).

Les conteneurs ont la particularité d'être **auto-porteurs** : leur stabilité est assurée qq soit la machine ou l'environnement sur lequel elle est utilisée. Ils sont aussi **auto-documentés** : la documentation est intégrée via le fichier de configuration nécessaire à son fonctionnement.

Sur une VM d'un PC, les ressources sont allouées _spécifiquement_ pour celle-ci : le système complet de la VM est créé dans l'hôte, pour qu'il ait ses propres ressources. Son isolation est totale.
Un conteneur _partage_ ses ressources avec le PC: le conteneur est un système de virtualisation plus léger qui ne consomme pas l'ensemble des ressources du système.
On dit que le conteneur permet de faire de la **virtualisation légère**. Il **isole les processus** de l'hôte, mais partage les ressources avec le système hôte. Mais un conteneur partage les CPU, RAM et disque avec le système hôte par exemple. Donc même si j'alloue 16 GO de RAM à mon conteneur, si finalement il n'utilise que 2 GO, les 14 Go restants seront toujours utilisables par le système hôte.

Un conteneur démarre plus rapidement qu'une VM, et permet de simplifier les infras entre développeurs (notamment s'ils taffent tous sur un même OS par exemple).

Docker n'est d'ailleurs pas le premier conteneur, il en existait d'autres avant. Les conteneurs ayant un kernel Linux, on ne peut pas faire tourner Windows dans l'un d'eux.

Les outils d'intégration continue et de livraison continue (CI/CD) utilisent souvent des conteneurs, permettant de créer des espaces isolés pour gérer les tests.
Docker s'utilise donc à tous les niveaux de l'infra (développement, CI, production).
