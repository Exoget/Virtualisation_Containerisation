

```> cat /etc/*release*``` 
information a propos de la version installée


``` >ip a ``` 
information sur l'adresse Ip


``` > sudo shutdown -h 0``` 
fermeture de la machine virtuelle

Acces par Pont : il va utiliser la meme interface reseaux de votre machine physique , cad il aura une @IP exactement c'est comme s'il sagit
d'une vrai machine physique


1 ere tentative de connexion depuis la machine hote ( windwos 10 ) vers la machine virtuelle Ubunto
```
>$ ssh osboxes@xxx.xxx.xxx.xxx
>ssh: connect to host xx.xxxx.xxxx.xxxxx port 22: Connection refused``` 
C'est normal parce que ubunto par defaut ne contient pas open ssh il faut l'installer  
``` 
> sudo apt-get install openssh-server
``` 
### Installation Docker
``` 
# instalation docker avec des script :
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
``` 
> docker version
pour vérifier la version docker instalée

je peux tester avec une petite application qui permets de tester simplement un msg ( whalesay)
https://hub.docker.com/r/docker/whalesay
lancement de cette image avec > sudo docker run docker/whalesay cowsay myNameIsAbdo
``` > sudo docker ps
> sudo docker images
``` 
un autre exemple
``` 
> sudo docker run -d nginx
> sudo docker ps ``` 
lancement du serveur en backGround 
Le staut du serveur il est en Up est se tourne sur le port 80: ( ca c'est important quand on installe docker, il cree sa popre interface
reseau qui lui est propore docker0

pour stoper une instance  d'une image ( conteneur)
``` 
> sudo doker stop ( les 4 premier nombre) ou bien l instance name
root@osboxes:/home/osboxes# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
7fc0dd7248bf        nginx               "nginx -g 'daemon of…"   20 minutes ago      Up 20 minutes       80/tcp              wizardly_boyd
root@osboxes:/home/osboxes# docker stop 7fc0
7fc0``` 

#### Rq: on n'a pas arriver a se connecter sur le serveur Web nginx depuis l'exterieur du conteneur , il faut faire une operation de mapping 
sur le port 80 ( sinon elle est inacccessible depuis l'exterieur) ( sinon à l'interieur de docker c'est bien géré meme si je lance deux instance
nginix conteneur avec le  meme port 80)
``` 
> sudo doker run -d -p 8082:80 nginx ( on peut aussi garder le meme port 80:80 )


> docker stop conteneur 
> docker rm conteneur  ( remove pour les conteneur)   rmi ( remove pour les images)


> docker run --name my-mySQL -e MYSQL_ROOT_PASSWORD=root -d mysql

Pour entrer en mode shell dans un cotenuer en cours d'execution
> docker exec -it <container name> bash
> docker exec -it my-mySQL mysql --password  ( i : mode interactif; t : terminal) ( je suis entrain d'executer un commande mysql a l'interieru
du conteneur my-mySQL en mode interractif consol
``` 
``` 
> docker run --rm -d alpine ping 8.8.8.8
 alpine image Linux tres reduite ( contient l'itentiel pour demmarer des commande ou sctipt shell)
> docker attach ideConteneur ( attacher une consol BackGround 
``` 
utiliser un nom passer en paramettre au lieu du nom standar avec le positionnement de mot de passse pour le user root
############l############l'image de base c'est le premier appel dans les fichier dockerFile
