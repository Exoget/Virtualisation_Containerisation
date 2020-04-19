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
