# Premier TD : Découverte de Docker

## Manipuler un conteneur

- Commandes utiles : https://devhints.io/docker
- Documentation ```docker run```   : https://docs.docker.com/engine/reference/run/

#### 1 - Lancez simplement un conteneur Debian en mode attached. Que se passe-t-il ?

#### 2 - Lancez un conteneur Debian avec la commande ``` echo "Debian container"``` .

#### 3 - Affichez la liste des conteneurs en cours d’exécution

#### 4 - Affichez la liste des conteneurs en cours d’exécution et arrêtés.

#### 5 - Lancez un conteneur debian en mode détaché avec la commande sleep 3600

#### 6 - Réaffichez la liste des conteneurs qui tournent

#### 7 - Tentez de stopper le conteneur, que se passe-t-il ?
``` docker stop <conteneur>``` 

NB: On peut désigner un conteneur soit par le nom qu’on lui a donné, soit par le nom généré automatiquement, soit par son empreinte (toutes ces informations sont indiquées dans un docker ps ou docker ps -a). L’autocomplétion fonctionne avec les deux noms.

#### 8 - Trouvez comment vous débarrasser d’un conteneur récalcitrant (si nécessaire, relancez un conteneur avec la commande ``` sleep 3600```  en mode détaché).

#### 9 - Tentez de lancer deux conteneurs avec le nom ``` debian_container``` 

Le nom d’un conteneur doit être unique (à ne pas confondre avec le nom de l’image qui est le modèle utilisé à partir duquel est créé le conteneur).

Créez un conteneur avec le nom ``` debian2``` 
#### ``` docker run debian -d --name debian2 sleep 500``` 
#### Lancez un conteneur debian en mode interactif (``` options -i -t)```  avec la commande ``` /bin/bash ``` et le nom ``` debian_interactif``` .
#### Explorer l’intérieur du conteneur : il ressemble à un OS Linux Debian normal.
