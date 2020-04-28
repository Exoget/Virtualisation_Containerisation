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
