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

#### Creation avec maven le plugin maven pour Docker c'est Fabric 8


```
<properties>
        <!--set this to your docker acct name-->
        <docker.image.prefix>sofraTeam</docker.image.prefix>

        <!--Set to name of project-->
        <docker.image.name>springbootdocker</docker.image.name>
    </properties>
....
 <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <plugin>
                <groupId>io.fabric8</groupId>
                <artifactId>docker-maven-plugin</artifactId>
                <version>0.33.0</version>

                <configuration>
                    <!--pour le cas d OSX-->
                    <!--<dockerHost>unix:///var/run/docker.sock</dockerHost>-->
                    <dockerHost>tcp://localhost:2375</dockerHost>
                    <!--<dockerHost>npipe:////./pipe/docker_engine</dockerHost>-->
                    <verbose>true</verbose>
                    <images>
                        <image>
                            <name>${docker.image.prefix}/${docker.image.name}</name>
                            <build>
                                <dockerFileDir>${project.basedir}/src/main/docker/</dockerFileDir>

                                <!--copies artficact to docker build dir in target-->
                                <assembly>
                                    <descriptorRef>artifact</descriptorRef>
                                </assembly>
                                <tags>
                                    <tag>latest</tag>
                                    <tag>${project.version}</tag>
                                </tags>
                            </build>
                            <run>
                                <ports>
                                    <port>8082:8080</port>
                                </ports>
                            </run>
                        </image>
                    </images>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

L'image docker 

```
FROM openjdk

VOLUME /tmp
ADD maven/spring-boot-docker-1.0.0-SNAPSHOT.jar myapp.jar
RUN sh -c 'touch /myapp.jar'
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/myapp.jar"]
```
maven : represente le reperoire target docker qui contient les artifact généré et copié depuis le target maven
Il faut lancer maven package premierement puis maven docker 

```mvn clean package docker:build docker:run```
This command will create my image , put it to my local dockerhub repo, then start a container based on that image
pour plus d'info
* https://docs.docker.com/engine/reference/commandline/build/
* https://dmp.fabric8.io/#running-containers

Avec le plugin maven je peux lancer tous les commandes docker
mvn docker:run ,  docker:start ( il va etre lancer en mode daemon background ), docker:stop  
