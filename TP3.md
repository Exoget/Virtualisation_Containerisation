## Maipulations des conteneurs

c'est le TP de rabbitMq avec la partie client qui envoie un msg vers la file, le micro service va consomer le msg et va l'inserer dans la base mysql
* https://github.com/Exoget/page-view-service
* https://github.com/Exoget/page-view-service-model
* https://github.com/Exoget/page-view-client
* https://github.com/Exoget/spring-boot-docker

#### Mysql

lancement d'un conteneur mysql

```docker run --name mysql -e MYSQL_DATABASE=rabbitDB -e MYSQL_ALLOW_EMPTY_PASSWORD=yes -p 3306:3306 -d mysql```

lancement en mode interactif

```docker exec -it mysql mysql --password```  (pour voir le contenu)

#### RabbitMQ
```docker run -d --hostname localhost --name myrabbitmq -p 5671:5671 -p 5672:5672 -p 15672:15672  rabbitmq:3-management```

#### MicroService
Pour comuniquer entre deux conteneur on doit utiliser la commande --link
```
docker run --name pageviewservice -p 8081:8081 \
--link myrabbitmq:myrabbitmq \
--link mysql:mysql \
-e SPRING_DATASOURCE_URL=jdbc:mysql://mysql:3306/rabbitDB?useSSL=false \
-e spring.rabbitmq.host=myrabbitmq \
exoget/pageviewservice
```

#### Utilisation de maven pour gerer les conteneurs

```
<plugin>
                <groupId>io.fabric8</groupId>
                <artifactId>docker-maven-plugin</artifactId>
                <version>0.33.0</version>

                <configuration>
                    <!--pour le cas d OSX-->
                    <!--<dockerHost>unix:///var/run/docker.sock</dockerHost>-->
                    <!--<dockerHost>tcp://localhost:2375</dockerHost>-->
                    <dockerHost>npipe:////./pipe/docker_engine</dockerHost>
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
                                    <port>8080:8080</port>
                                </ports>
                                <links>
                                    <link>myrabbitmq:rabbitmq</link>
                                </links>
                                <env>
                                    <SPRING_RABBITMQ_HOST>rabbitmq</SPRING_RABBITMQ_HOST>
                                </env>
                                <dependsOn>
                                    <container>myrabbitmq</container>
                                    <container>pageviewservice</container>
                                </dependsOn>
                            </run>
                        </image>
                        <!--mysql container-->
                        <image>
                            <name>mysql</name>
                            <alias>mysqldb</alias>
                            <run>
                                <!--expose port for dn-->
                                <ports>
                                    <port>3306:3306</port>
                                </ports>
                                <!--set env prams for mysql image-->
                                <env>
                                    <MYSQL_DATABASE>rabbitDB</MYSQL_DATABASE>
                                    <MYSQL_ALLOW_EMPTY_PASSWORD>yes</MYSQL_ALLOW_EMPTY_PASSWORD>
                                </env>
                                <!--wait for db to start-->
                                <wait>
                                    <time>30000</time>
                                </wait>
                            </run>
                        </image>
                        <!--rabbit container-->
                        <image>
                            <name>rabbitmq:3-management</name>
                            <alias>myrabbitmq</alias>
                            <run>
                                <ports>
                                    <port>5671</port>
                                    <port>5672</port>
                                    <port>4369</port>
                                    <port>25672</port>
                                </ports>
                                <!--wait for db to start-->
                                <wait>
                                    <time>30000</time>
                                </wait>
                            </run>
                        </image>
                        <!--docker run &#45;&#45;name pageviewservice -p 8081:8081 &#45;&#45;link rabbitmq:rabbitmq &#45;&#45;link mysqldb:mysqldb-->
                        <!-- -e SPRING_DATASOURCE_URL=jdbc:mysql://mysqldb:3306/pageviewservice -e SPRING_PROFILES_ACTIVE=mysql-->
                        <!-- -e SPRING_RABBITMQ_HOST=rabbitmq springframeworkguru/pageviewservice-->
                        <!--page container-->
                        <image>
                            <name>exoget/pageviewservice</name>
                            <alias>pageviewservice</alias>
                            <run>
                                <ports>
                                    <port>8081:8081</port>
                                </ports>
                                <links>
                                    <link>myrabbitmq:rabbitmq</link>
                                    <link>mysqldb:mysqldb</link>
                                </links>
                                <env>
                                    <SPRING_DATASOURCE_URL>jdbc:mysql://mysqldb:3306/rabbitDB</SPRING_DATASOURCE_URL>
                                    <SPRING_PROFILES_ACTIVE>mysql</SPRING_PROFILES_ACTIVE>
                                    <SPRING_RABBITMQ_HOST>rabbitmq</SPRING_RABBITMQ_HOST>
                                </env>
                                <dependsOn>
                                    <container>myrabbitmq</container>
                                    <container>mysqldb</container>
                                </dependsOn>
                                <wait>
                                    <http>
                                        <url>http://localhost:8081/actuator/health</url>
                                        <method>GET</method>
                                        <status>200..399</status>
                                    </http>
                                    <time>75000</time>
                                </wait>
                            </run>
                        </image>
                    </images>
                </configuration>
            </plugin>
```

Au final , il faut builer l'image et lancer tous les conteneurs 
```
mvn clean package docker:build docker:run
```
