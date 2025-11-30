### Build and Test (7 Points)

Depuis mon repo git perso, j'ai ajouté un fichier .env (contenant les différentes variables sensibles) et un fichier .gitignore (qui contient le .env pour éviter qu'il ne soit copié lors du git clone). Aussi, j'ai ajouté le répertoire initdb et certains fichiers inutiles lors de l'étape copy lors du build de l'image backend-app.
Ensuite depuis une session Docker Playground, j'ai cloné le repo mini projet docker en local. J'ai buildé une image backend-app depuis le dockerfile et puller une image officiel mysql:8.0.


Après avoir déployer un registre privé docker en local:

[node1] (local) root@10.0.14.4 ~/bootcamp-project-update/mini-projet-docker
$ docker run -d -p 5000:5000 --name myregistry --restart=always registry:2
Unable to find image 'registry:2' locally
2: Pulling from library/registry
44cf07d57ee4: Pull complete 
bbbdd6c6894b: Pull complete 
8e82f80af0de: Pull complete 
3493bf46cdec: Pull complete 
6d464ea18732: Pull complete 
Digest: sha256:a3d8aaa63ed8681a604f1dea0aa03f100d5895b6a58ace528858a7b332415373
Status: Downloaded newer image for registry:2
aa172fb15718a810e9dfca3bc9b47731107cfb51e1bbd108fe267b6e49dd5185


j'ai taggué mes 2 images pour pouvoir les pousser vers mon registre privé:

[node1] (local) root@10.0.14.4 ~/bootcamp-project-update/mini-projet-docker
$ docker tag backend-app:latest localhost:5000/backend-app:latest
[node1] (local) root@10.0.14.4 ~/bootcamp-project-update/mini-projet-docker
$ docker tag mysql:8.0 localhost:5000/mysql:8.0
[node1] (local) root@10.0.14.4 ~/bootcamp-project-update/mini-projet-docker
$ docker images
REPOSITORY                   TAG       IMAGE ID       CREATED          SIZE
backend-app                  latest    07d8bf9856f0   20 minutes ago   340MB
localhost:5000/backend-app   latest    07d8bf9856f0   20 minutes ago   340MB
mysql                        8.0       34178dbaefd0   5 weeks ago      783MB
localhost:5000/mysql         8.0       34178dbaefd0   5 weeks ago      783MB
registry                     2         26b2eb03618e   2 years ago      25.4MB
 
[node1] (local) root@10.0.14.4 ~/bootcamp-project-update/mini-projet-docker
$ docker push localhost:5000/backend-app:latest
The push refers to repository [localhost:5000/backend-app]
9ee60b9553cc: Pushed 
49ecf9f4c639: Pushed 
edbf455418ea: Pushed 
256f393e029f: Pushed 
latest: digest: sha256:e5662e1a41f1d6e3dfdb140eb9ce65c41b906bab12ffe0b3d696cf71cd00b2f9 size: 1160
[node1] (local) root@10.0.14.4 ~/bootcamp-project-update/mini-projet-docker
$ docker push localhost:5000/mysql:8.0
The push refers to repository [localhost:5000/mysql]
81561263622f: Pushed 
eb5dbce12c26: Pushed 
9cc5cce37eed: Pushed 
ba3c3f2e0e0b: Pushed 
626845a732cb: Pushed 
748b89f8339d: Pushed 
cd6e82c8f993: Pushed 
7ef7308b563e: Pushed 
1dc0db901fb8: Pushed 
03106cca3a80: Pushed 
827b99c091a5: Pushed 
8.0: digest: sha256:e7860b91f55ef0ad252531b9f781e5f371fd12114c6589bfacc5d77588535ed8 size: 2619


Test contenu registre OK:

[node1] (local) root@10.0.14.4 ~/bootcamp-project-update/mini-projet-docker
$ curl -X GET http://localhost:5000/v2/_catalog
{"repositories":["backend-app","mysql"]}


Deploiement des images (backend + mysql) depuis le dockercompose (voir repo):

$ docker compose up -d
[+] Building 0.0s (0/0)                                                                          docker:default
[+] Running 4/4
 ✔ Network mini-projet-docker_paymybuddy-net          Creat...                                             0.0s 
 ✔ Volume "mini-projet-docker_mysql-data"             Created                                              0.0s 
 ✔ Container mini-projet-docker-paymybuddy-db-1       St...                                                0.0s 
 ✔ Container mini-projet-docker-paymybuddy-backend-1  Started                                              0.0s 

 [node1] (local) root@10.0.14.4 ~/bootcamp-project-update/mini-projet-docker
docker ps
CONTAINER ID   IMAGE                               COMMAND                  CREATED          STATUS          PORTS                               NAMES
8a695b24034b   localhost:5000/backend-app:latest   "java -jar app.jar"      30 minutes ago   Up 30 minutes   0.0.0.0:8080->8080/tcp              mini-projet-docker-paymybuddy-backend-1
f8bb23824acc   localhost:5000/mysql:8.0            "docker-entrypoint.s…"   30 minutes ago   Up 30 minutes   0.0.0.0:3306->3306/tcp, 33060/tcp   mini-projet-docker-paymybuddy-db-1
aa172fb15718   registry:2                          "/entrypoint.sh /etc…"   38 minutes ago   Up 38 minutes   0.0.0.0:5000->5000/tcp              myregistry
[node1] (local) root@10.0.14.4 ~/bootcamp-project-update/mini-projet-docker



Test application backend depuis le port 8080 OK (voir capture écran)

