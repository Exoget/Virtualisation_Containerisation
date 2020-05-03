## utilisation Docker Compose

Dans les autres Tps , on a utilisé le plugin maven fabric8 pour orchestrer les conteneurs.

Avec Docker Compose, c'est la meme chose un outil pour orchestrer les conteneurs en utilisant YML pour décrire les environements utilsés.
Maintenant si on parle de devops et les gens de la prod les ops on va pas leurs fournir le plugin maven fabric8 ( il ne sont pas intéressé par maven qui reste un outil de build).
Docker compose a ete concu pour la production pour la gestion des applications, il y a d'autre outil qui sont plus sophistiqué à titre d'exp Kubernetes ( pour la grande échelle ) 

### Docker-compose exemple

Instalation d'un environement wordpress avec un base mysql en local

```
version: '2' #API Version of Docker Compose

services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: wordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_PASSWORD: wordpress
# pour le mapping de volume partage
volumes:
  db_data:
```
pour démarrer les conteneurs, il faut se deplacer dans le repertoire conteant le fichier docker-compose!!
 
```docker-compose up -d```


pour arreter les conteneurs, il faut se deplacer dans le repertoire conteant le fichier docker-compose!!

```docker-compose down```