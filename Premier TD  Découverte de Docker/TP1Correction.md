# Premier TD : Découverte de Docker

## Manipuler un conteneur

- Commandes utiles : https://devhints.io/docker
- Documentation ```docker run```   : https://docs.docker.com/engine/reference/run/

#### 1 - Lancez simplement un conteneur Debian en mode attached. Que se passe-t-il ?

#### CORRECTION :
```
docker run debian
# ou
docker run --attached debian
```
#### Il ne se passe rien car comme debian ne contient pas de processus qui continue de tourner le conteneur s'arrête
#### 2 - Lancez un conteneur Debian avec la commande echo "Debian container".

CORRECTION :

docker run debian echo "Debian container"
=> Debian container
3- Affichez la liste des conteneurs en cours d’exécution

CORRECTION :

docker ps
4- Affichez la liste des conteneurs en cours d’exécution et arrêtés.

CORRECTION :

docker ps -a
5- Lancez un conteneur debian en mode détaché avec la commande sleep 3600

6- Réaffichez la liste des conteneurs qui tournent

7- Tentez de stopper le conteneur, que se passe-t-il ?

docker stop <conteneur>
NB: On peut désigner un conteneur soit par le nom qu’on lui a donné, soit par le nom généré automatiquement, soit par son empreinte (toutes ces informations sont indiquées dans un docker ps ou docker ps -a). L’autocomplétion fonctionne avec les deux noms.

8- Trouvez comment vous débarrasser d’un conteneur récalcitrant (si nécessaire, relancez un conteneur avec la commande sleep 3600 en mode détaché).

CORRECTION :

docker kill <conteneur>
9- Tentez de lancer deux conteneurs avec le nom debian_container

CORRECTION :

docker run debian -d --name debian_container sleep 500
docker run debian -d --name debian_container sleep 500
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
CORRECTION :

Dans un nouveau terminal lancez docker inspect test_nginx (c’est le nom de votre conteneur Nginx). Cette commande fournit plein d’informations utiles mais difficiles à lire.

Lancez la commande à nouveau avec | grep IPAddress à la fin. Vous récupérez alors l’adresse du conteneur dans le réseau virtuel Docker.

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
Pour aller plus loin Facultatif : Wordpress, MYSQL et les variables d’environnement
Lancez un conteneur Wordpress joignable sur le port 8080 à partir de l’image officielle de Wordpress du Docker Hub
Visitez ce Wordpress dans le navigateur
Nous pouvons accéder au Wordpress, mais il n’a pas encore de base MySQL configurée. Ce serait un peu dommage de configurer cette base de données à la main. Nous allons configurer cela à partir de variables d’environnement et d’un deuxième conteneur créé à partir de l’image mysql.

Depuis Ubuntu:

Il va falloir mettre ces deux conteneurs dans le même réseau (nous verrons plus tarde ce que cela implique), créons ce réseau :
docker network create wordpress
Cherchez le conteneur mysql version 5.7 sur le Docker Hub.

Utilisons des variables d’environnement pour préciser le mot de passe root, le nom de la base de données et le nom d’utilisateur de la base de données (trouver la documentation sur le Docker Hub).

Il va aussi falloir définir un nom pour ce conteneur

CORRECTION :

docker run --name mysqlpourwordpress -d -e MYSQL_ROOT_PASSWORD=motdepasseroot -e MYSQL_DATABASE=wordpress -e MYSQL_USER=wordpress -e MYSQL_PASSWORD=monwordpress -p 3306:3306 --network wordpress mysql:5.7
inspectez le conteneur MySQL avec docker inspect
Faites de même avec la documentation sur le Docker Hub pour préconfigurer l’app Wordpress.
En plus des variables d’environnement, il va falloir le mettre dans le même réseau, et exposer un port
CORRECTION :

docker run --name wordpressavecmysql -d -e WORDPRESS_DB_HOST="mysqlpourwordpress:3306" -e WORDPRESS_DB_PASSWORD=monwordpress -e WORDPRESS_DB_USER=wordpress --network wordpress -p 80:80 wordpress
regardez les logs du conteneur Wordpress avec docker logs

visitez votre app Wordpress et terminez la configuration de l’application : si les deux conteneurs sont bien configurés, on ne devrait pas avoir à configurer la connexion à la base de données

avec docker exec, visitez votre conteneur Wordpress. Pouvez-vous localiser le fichier wp-config.php ? Une fois localisé, utilisez docker cp pour le copier sur l’hôte.
<!-- - (facultatif) Détruisez votre conteneur Wordpress, puis recréez-en un et poussez-y votre configuration Wordpress avec docker cp. Nous verrons ensuite une meilleure méthode pour fournir un fichier de configuration à un conteneur. -->

Faire du ménage
Lancez la commande docker ps -aq -f status=exited. Que fait-elle ?

Combinez cette commande avec docker rm pour supprimer tous les conteneurs arrêtés (indice : en Bash, une commande entre les parenthèses de "$()" est exécutée avant et utilisée comme chaîne de caractère dans la commande principale)

CORRECTION :

docker rm $(docker ps -aq -f status=exited)
S’il y a encore des conteneurs qui tournent (docker ps), supprimez un des conteneurs restants en utilisant l’autocomplétion et l’option adéquate

Listez les images

Supprimez une image

Que fait la commande docker image prune -a ?

Décortiquer un conteneur
En utilisant la commande docker export votre_conteneur -o conteneur.tar, puis tar -C conteneur_decompresse -xvf conteneur.tar pour décompresser un conteneur Docker, explorez (avec l’explorateur de fichiers par exemple) jusqu’à trouver l’exécutable principal contenu dans le conteneur.
Portainer
Portainer est un portail web pour gérer une installation Docker via une interface graphique. Il va nous faciliter la vie.

Lancer une instance de Portainer :
docker volume create portainer_data
docker run --detach --name portainer \
    -p 9000:9000 \
    -v portainer_data:/data \
    -v /var/run/docker.sock:/var/run/docker.sock \
    portainer/portainer-ce
Remarque sur la commande précédente : pour que Portainer puisse fonctionner et contrôler Docker lui-même depuis l’intérieur du conteneur il est nécessaire de lui donner accès au socket de l’API Docker de l’hôte grâce au paramètre --mount ci-dessus.

Visitez ensuite la page http://localhost:9000 ou l’adresse IP publique de votre serveur Docker sur le port 9000 pour accéder à l’interface.

il faut choisir l’option “local” lors de la configuration

Créez votre user admin et choisir un mot de passe avec le formulaire.

Explorez l’interface de Portainer.

Créez un conteneur.

<!-- ## Installer Docker Desktop for Windows

À l’aide des instructions du site officiel, téléchargez et installez Docker Desktop for Windows.
après avoir vérifié que Docker fonctionnait avec docker info dans une invite de commande Windows, installez Visual Studio Code. Ensemble, explorons son interface. . -->
<!-- - Facultatif : installez l’extension VSCode “Docker” par Microsoft pour vous faciliter la vie. Explorez l’interface. -->

<!-- ## Facultatif : explorer Gitpod

Se créer un compte sur github.com

Créer un dépôt vide (vous pouvez l’appeler tp_docker par exemple) et l’ouvrir avec Gitpod

Dans Gitpod, lancer la commande suivante pour installer Docker (brew est un gestionnaire de packet créé initialement pour MacOS et qui ne nécessite pas de droits sudo) :
brew install docker

Dans Gitpod, copiez-collez la clé privée fournie dans le dossier de partage dans un fichier appelé ssh.private, changez ses permissions avec :
chmod go-rwx ssh.private

Puis (après avoir retrouvé l’IP publique de votre VM Scaleway) lancez le tunnel SSH qui nous permettra d’accéder à notre dæmon Docker comme ceci :

export DOCKER_IP=INSEREZ_VOTRE_IP_ICI
ssh -nNT -L localhost:23750:var/run/docker.sock $DOCKER_IP -N -l root -i ssh.private
Puis dans un autre terminal :

export DOCKER_HOST=localhost:23750
