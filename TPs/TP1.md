### TP : installation mongoDB sur Docker

```> docker run mongo```
important il faut télécharger mongo en mode interactif -it ( sinon lorsque je vais quitter le terminal le processus va être stoppé ) 

```> docker run -d mongo```
mongo lancé en background

```
> docker logs -f dockerRéférence  ( afficher les logs du conteneur- rendu statique)
> docker run -d -p 27017:27017 mongo
> mvn spring-boot:run ( lancer l’application mongoDB ) il va utiliser le port 27017 par défaut , c'est grace a spring boot qui connaît les standard MongDB 
```
#########################################################################

Rappel : Docker Images
c’est comme la class en java, et l’instance de la class c’est le conteneur docker.
image est immutable: une fois buildée on peut pas la modifiée.
image est buildée en layer ( couche ) aussi des fichier immutable.
chaque layer possède un ID hashé ( codé en SHA256)
l’id final de l’image docker est calculé en tenant compte des ids de layers.
si le hash id d’un layer change l’id de limage docker change aussi par conséquence.

le docker hub registry est comme le repo maven 
								   
#########################################################################
### TP : installation rabbitMQ
```
> docker run -d --hostname localhost --name myrabbitmq -p 5671:5671 -p 5672:5672 -p 15672:15672  rabbitmq:3-management
```
#########################################################################

On peut définir un volume pour une image docker -v paramètre	
```$ docker run -p 27017:27017 -v D:/Docker/data/mongo:/data/db -d mongo```

alternative sous window: le sharing folder n’est pas pris en compte avec mongoDB
```$docker volume create --name=mongodata 
$docker run --name mongodb -v mongodata:/data/db -d -p 27017:27017 mongo
```			   
#########################################################################
### TP : installation mySql
```$docker volume create --name=mysqldata 
$docker run --name mysql -v mysqldata:/var/lib/mysql -e MYSQL_ROOT_PASSWORD= yes -p 3306:3306 -d mysql
```
sans volume
```$docker run --name mysql -e MYSQL_ROOT_PASSWORD=yes -p 3306:3306 -d mysql
```
##################################

### TIPS : Cleaning Up after Docker
La manipulation des images ainsi des conteneurs docker va engendrer des espaces mémoire dans le temps.
Lorsque le conteneur est tjr défini il y a un espace mémoire qui lui est rattaché, aussi un espace de stockage lui aussi créer dans le cas ou on a défini un volume physique.

* cleaning up Container:

on ne peut pas toucher le volume d'un conteneur en cours d'exécution, il faut l'arrêter tout d'abord  

suppression de tous les conteneurs en cours d'exécution
```>docker kill ($docker ps -q)```

suppression de tous les conteneurs stoppés
```>docker rm ($docker ps -a -q)```


* cleaning up Images:

suppression d'une image bien précise 
```>docker rmi <image name>```

suppression de tous les images non tagées ( par exemple j'ai téléchargé une version x
puis j'ai téléchargé la version latest x+1 ) la version x devient non taggé 
inexploitable : dangling
```>docker rmi ($docker images -q -f dangling=true)```

suppression de tous les images
```>docker rmi ($docker images -q)```

* cleaning up Volume:

une fois le volume est devenu inexploitable ( n'est plus associé avec un conteneur)
il est considéré comme dangling   

suppression de tous les volumes dangling
```>docker volume rm ($docker volume ls -f dangling=true -q)```

suppression de tous les conteneurs stoppés
```>docker rm ($docker ps -a -q)```
