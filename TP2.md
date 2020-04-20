### Creation d'images Docker

Nous devons partir d'une image de base. voici un exemple de manipluation d'une image OS de base.

```$ docker run -d centos```

```$ docker run -d centos tail -f /dev/null```
Pour forcer le conteneur a etre tjr en cours d'execution, puisque par defaut il va sortir et ne rien faire.
* Le périphérique nul est généralement utilisé pour éliminer un flux de sortie indésirable.
* Le périphérique /dev/null est un fichier spécial, et non un dossier.

```
$ docker exec -it "docker id" bash ( il nous renvoie le curseur bash de system, et je peux lancer des commande par ma suite )
$ yum install java ( installer la version java )
```

Tous ces étapes on peut les créer dans un premier DockerFile avec une simple application springboot

Version 1
```
FROM openjdk:8-jdk-alpine
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```

Version 2
```
FROM centos
RUN yum install -y java
VOLUME /tmp
ADD /spring-boot-web-0.0.1-SNAPSHOT.jar myApp.jar
RUN sh -c 'touch /myApp.jar'  ( pour mettre une nouvelle date sur les ensemble des fichers)
ENTRYPOINT ["java","-Djava.security.edg=file:/dev/./urandom","-jar","/myApp.jar"]
```

Puis pour lancer la contruction de l'image il faut lancer avec ces options
```$ docker build -t spring-boot-docker .```
* - t : tag name
* . : chercher le fichier DockerFile dans le repertoir courant


pour lancer monc conteneur 
```$ docker run -d -p 8080:8080 spring-boot-docker```
