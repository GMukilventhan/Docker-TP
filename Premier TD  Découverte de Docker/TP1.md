Premier TD : Découverte de Docker
Manipuler un conteneur
Commandes utiles : https://devhints.io/docker
Documentation docker run : https://docs.docker.com/engine/reference/run/
1- Lancez simplement un conteneur Debian en mode attached. Que se passe-t-il ?

2- Lancez un conteneur Debian avec la commande echo "Debian container".

3- Affichez la liste des conteneurs en cours d’exécution

4- Affichez la liste des conteneurs en cours d’exécution et arrêtés.

5- Lancez un conteneur debian en mode détaché avec la commande sleep 3600

6- Réaffichez la liste des conteneurs qui tournent

7- Tentez de stopper le conteneur, que se passe-t-il ?

docker stop <conteneur>
NB: On peut désigner un conteneur soit par le nom qu’on lui a donné, soit par le nom généré automatiquement, soit par son empreinte (toutes ces informations sont indiquées dans un docker ps ou docker ps -a). L’autocomplétion fonctionne avec les deux noms.

8- Trouvez comment vous débarrasser d’un conteneur récalcitrant (si nécessaire, relancez un conteneur avec la commande sleep 3600 en mode détaché).

9- Tentez de lancer deux conteneurs avec le nom debian_container

Le nom d’un conteneur doit être unique (à ne pas confondre avec le nom de l’image qui est le modèle utilisé à partir duquel est créé le conteneur).

Créez un conteneur avec le nom debian2
docker run debian -d --name debian2 sleep 500
Lancez un conteneur debian en mode interactif (options -i -t) avec la commande /bin/bash et le nom debian_interactif.
Explorer l’intérieur du conteneur : il ressemble à un OS Linux Debian normal.
Chercher sur Docker Hub
Visitez hub.docker.com
Cherchez l’image de Nginx (un serveur web), et téléchargez la dernière version (pull).
docker pull nginx
Lancez un conteneur Nginx. Notez que lorsque l’image est déjà téléchargée le lancement d’un conteneur est quasi instantané.
docker run --name "test_nginx" nginx
Ce conteneur n’est pas très utile, car on a oublié de configurer un port ouvert.

Trouvez un moyen d’accéder quand même au Nginx à partir de l’hôte Docker (indice : quelle adresse IP le conteneur possède-t-il ?).
Arrêtez le(s) conteneur(s) nginx créé(s).
Relancez un nouveau conteneur nginx avec cette fois-ci le port correctement configuré dès le début pour pouvoir visiter votre Nginx en local.
docker run -p 8080:80 --name "test2_nginx" nginx # la syntaxe est : port_hote:port_container
En visitant l’adresse et le port associé au conteneur Nginx, on doit voir apparaître des logs Nginx dans son terminal car on a lancé le conteneur en mode attached.
Supprimez ce conteneur. NB : On doit arrêter un conteneur avant de le supprimer, sauf si on utilise l’option “-f”.
On peut lancer des logiciels plus ambitieux, comme par exemple Funkwhale, une sorte d’iTunes en web qui fait aussi réseau social :

docker run --name funky_conteneur -p 80:80 funkwhale/all-in-one:1.0.1
Vous pouvez visiter ensuite ce conteneur Funkwhale sur le port 80 (après quelques secondes à suivre le lancement de l’application dans les logs) ! Mais il n’y aura hélas pas de musique dedans :(

Attention à ne jamais lancer deux containers connectés au même port sur l’hôte, sinon cela échouera !

Supprimons ce conteneur :
docker rm -f funky_conteneur
