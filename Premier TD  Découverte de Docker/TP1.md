

Manipulation de conteneurs
Pour ces exercices, référez-vous aux commandes utiles : https://devhints.io/docker et à la documentation docker run : https://docs.docker.com/engine/reference/run/

Lancez simplement un conteneur Debian en mode attached. Que se passe-t-il ?
docker run --rm -it debian

Un conteneur Debian est lancé en mode interactif et on est connecté à un shell bash à l'intérieur du conteneur.

Lancez un conteneur Debian avec la commande echo "Debian container".
docker run --rm debian echo "Debian container"

Un conteneur Debian est lancé et la commande echo "Debian container" est exécutée, puis le conteneur s'arrête.

Affichez la liste des conteneurs en cours d'exécution.
docker ps

Cette commande affiche la liste des conteneurs en cours d'exécution.

Affichez la liste des conteneurs en cours d'exécution et arrêtés.
docker ps -a

Cette commande affiche la liste de tous les conteneurs, y compris ceux qui sont arrêtés.

Lancez un conteneur Debian en mode détaché avec la commande sleep 3600.
docker run -d --name mydebian debian sleep 3600

Un conteneur Debian est lancé en mode détaché avec la commande sleep 3600, ce qui signifie qu'il reste en arrière-plan pendant 3600 secondes (1 heure).

Réaffichez la liste des conteneurs qui tournent.
docker ps

La liste des conteneurs en cours d'exécution est affichée, y compris celui qui a été lancé en mode détaché.

Tentez de stopper le conteneur, que se passe-t-il ?
docker stop mydebian

Le conteneur est arrêté.

Trouvez comment vous débarrasser d'un conteneur récalcitrant (si nécessaire, relancez un conteneur avec la commande sleep 3600 en mode détaché).
docker rm -f mydebian

Le conteneur est supprimé de force.

Tentez de lancer deux conteneurs avec le nom debian_container.
docker run --name debian_container debian sleep 3600

Un conteneur est lancé avec le nom "debian_container".

docker run --name debian_container debian sleep 3600

Lorsque l'on tente de lancer un autre conteneur avec le même nom, une erreur apparaît : "Error response from daemon: Conflict. The container name "/debian_container" is already in use by container..."

On ne peut pas lancer deux conteneurs avec le même nom.

Créez un conteneur avec le nom debian2 :

docker run -d --name debian2 debian sleep 500

Lancez un conteneur Debian en mode interactif (options -i -t) avec la commande /bin/bash et le nom debian_interactif.

docker run -it --name debian_interactif debian /bin/bash

Un conteneur Debian est lancé en mode interactif avec un shell bash.
