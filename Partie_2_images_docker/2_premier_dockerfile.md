# Créez votre premier Dockerfile

Je souhaite maintenant créer ma propre image Docker, pour pouvoir à la fois installer node.js et les dépendances de mon projet.

Pour ce faire, je dois créer un fichier appelé le **Dockerfile**, qui liste les différents éléments dont j'ai besoin pour faire tourner mon projet, à la manière d'un `package.json` en Node.js.

Un Dockerfile liste différentes instructions, chacune créant une nouvelle **layer** ("couche" ou étape) de la construction du projet. Le but est que le nombre d'étages soit le plus possible limité, pour que le résultat soit le plus léger et performant possible.
