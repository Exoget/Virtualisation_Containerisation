# Docker Swarm Mode

```Docker swarm``` est utilisé pour fournir plusieurs conteneurs et services et qu'on a le niveau de service qui est attendu.

##### C'est quoi le mode Docker Swarm
Docker nous donne un standrad pour lancer les conteneurs n'importe ou, c'est comme la JVM dans le monde Java.
Docker peut etre executé sur beaucoup de systemes d'operation (Linux/Window) et des platformes ( Amazon, Azure, Google Cloud ...)

Avec le mode containerized deployment maintenant, les conteneurs sont distribués sur plusierus noeud (machine physique ou virtuelle),
et comme les conteneurs ont besoin d'etre maintenu et deployé avec toute la gestion, il y a beaucoup de question qui se pose !!
* comment augementer le nombre de conteneur si nous avons besoin de plus en plus des conterus
* comment assurer la communiction entre les conteneurs point de vue reseau network dans le contexte de cloud
Avec d'autre problemes a gerer comme les questions de sécurité ( si on travaille avec des données sencible ..) 

Le Docker Swarm mode est installé par defaut avec Docker mais pas activé, il faut faire ```Docker swarm init```, une fois activé nous aurons les commandes suivantes:
* Docker swarm
* Docker node
* Docker service
* Docker stack
* Docker secret

Il y a d'autres solution plus mature est sophihstiqué dans le marché comme Kubernates ( solution Goole) et OpenShit ( Red Hat ) et Appach Mesos et fianlement MesosPhere. Il y  beaucoup de concurrence et compétition entre ces fournisseurs.

