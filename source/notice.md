# Document, Notice

 

## Documents colibri


 |N°| | Contenu   |
|--|--|--|
||Control Center (CC) Interface Control Document|https://docs.google.com/document/d/1oaIpG3hnNkukqTZHAvWJ9ybCNE6xaUUI72z0k6HSIZg/edit |
|06/11/2017 |Colibri GFT Control Center|https://docs.google.com/document/d/1iFjhXA8IsbkXYqsd7VYVk3cRTMXoj0sGYWApoDwViG0/edit |
|16/11/2016| (Colibri GFT Control Center)|https://docs.google.com/document/d/1zNcX2T7FW5ZawYYfLejQqLcgUdyxC3UdqizzUjN860I/edit|
 | |Complete system of a building |https://indico.in2p3.fr/event/18260/contributions/72012/attachments/53738/70107/Colibri.pdf|
 |2017/02/23| Interface Specification ICD GIC / CC-OCS|| 
|12/10/2017|Design Description for GIC||


 
 
## Logiciels
 
### POSTGRESQL


```

--utilistateur postgres
sudo -i -u postgres 
psql
 
CREATE ROLE root superuser
??CREATE USER root
ALTER ROLE root WITH LOGIN;
ALTER ROLE root WITH CREATEDB;
ALTER USER root WITH ENCRYPTED password '051074';
CREATE DATABASE testdb OWNER root
 
\conninfo
 
docker exec -it 175a19a66a3b bash
psql -h localhost -U test -d testdbb  
\c testdb root
\dt
\d sensors
 
docker exec -it root psql -U postgres
//switch database 
\c dbname username
 
postgres=# \c dvdrental
You are now connected to database "dvdrental" as user "postgres".

```


#### shortcuts

```
1) List available databases
\l
 
4) List available tables
\dt
voir toute les tables
\d *
 
5) Describe a table
\d table_name
 
9) List users and their roles
\du
 
10 ) Insertion
\i file.sql
``` 

#### Configuration pg_hba

```
//# Database administrative login by Unix domain socket
local   all             postgres                                trust
 
//# TYPE  DATABASE        USER            ADDRESS                 METHOD
 
//# "local" is for Unix domain socket connections only
local   all             all                                     md5
//# IPv4 local connections:
host    all             all             127.0.0.1/32            trust
//# IPv6 local connections:
host    all             all             ::1/128                 md5
//# Allow replication connections from localhost, by a user with the
//# replication privilege.
local   replication     all                                     peer
host    replication     all             127.0.0.1/32            md5
host    replication     all             ::1/128                 md5
 ```

#### TroubbleShouting

```
sudo netstat -tanup | grep 5000
sudo locate pg_hba.conf
su
systemctl list-units|grep postgresql
sudo nano /var/lib/pgsql/12/data/pg_hba.conf
sudo nano /etc/postgresql/12/main/pg_hba.conf;
sudo systemctl restart postgresql
 ```

Modifier vartype table

```
ALTER TABLE documents ALTER COLUMN corps TYPE text;
```


Insert data  , upload file si pas connecté
```
psql -h hostname -d databasename -U username -f file.sql 
psql -h localhost -d testdb -U root -f localhost.sql;
 ```


UPDATE 11->12

[https://www.kostolansky.sk/posts/upgrading-to-postgresql-12/](upgrading-to-postgresql-12)
 [https://www.paulox.net/2020/04/24/upgrading-postgresql-from-version-11-to-12-on-ubuntu-20-04-focal-fossa/#:~:text=The%20recommended%20procedure%20is%20to,12%20cluster%20and%20drop%20it.&text=Upgrade%20the%2011%20cluster%20to%20the%20latest%20version.&text=Check%20that%20the%20upgraded%20cluster%20works%2C%20then%20remove%20the%2011%20cluster](upgrading-postgresql-from-version-11)
  
### GIT-LAB

 // ERREUR PYTEST
- 1)changer version de pytest dans requirements

--> line 8, in <module>
 -    sys.exit(main())
 - ------------------------OK 3 TESTS

POUR PYLINT
- py.test -s 
- ->lister les erreurs Pylint a corriger
- pylint *.py src --output-format=text | tee reports/pylint.log
- ->Vérifier les erreurs d'un fichier
- pylint --const-rgx='[a-z_][a-z0-9_]{2,30}$' src/observ/puissance.py
- -->afin d'eviter de scanner tous les 

dossier ajouter a la racine 
- ..coveragerc avec "paths et omit" a configurer

Module not found
- --verifier le gitlab-ci et faire appel au requirements

 POUR COVERAGE
- -> demarrer la couverture d'un dossier
- coverage run --source src -m pytest tests

-> rapport
ou
coverage report
-> effacer le rapport
coverage erase

@ pytest.fixture () peut avoir les valeurs de fonction , classe , module ou session
execute une fois la fonction

co
```
coverage run TU.py
coverage run --omit TU.py TU.py
coverage report
coverage erase
```

Test
```
pytest --cov TU.py Puissance.py
overage run --source fun -m pytest tests
man pytest  
pytest TU.py
pytest --cov sample TU.py
py.test -s  
pytest --fixtures
less /home/benamar/.local/bin/coverage
coverage3 run -m pytest && coverage3 report
coverage report --omit TU.py  
python3 import observ/watch_file.py 
 python3 test_Watch.py 
```

### VS-code


Problème d'import
nit.py vide a la racine
+ settings.json
{
    "python.envFile": ".env",
    "python.linting.pylintPath": ".venv/bin/pylint",
}
+ .env
PYTHONPATH="/home/benamar/travail/DC2/colibri-svom-interface"

###	Docker

```
docker exec -it 175a19a66a3b bash

sudo netstat -tanup | grep 5000
docker build -t rp .  
 docker run  --rm rp       
docker-compose up  
//consulter en local le dossier data du docker
●/var/lib/docker/devicemapper/devicemapper/data stores the images
●/var/lib/docker/devicemapper/devicemapper/metadata the metadata
// se connecter en tant qu'utilisateur dans le container
docker exec -it --user root 07ee2e6b7d19 /bin/bash 
//creer un dossier dans le conteneur
docker exec -i e18850ce0671 /bin/bash -c 'cat > /src/data' < data/1594.fits 
//copier un repertoire dans le docker 
docker cp ./doudou e18850ce0671:/src/data 
```


``` 
------------VERSION 10/10
DOCKERFILE
FROM python:3.6-slim-buster
RUN apt-getupdate
WORKDIR /src
COPY ./requirements.txt .
RUN pip install -r requirements.txt
#ENV PYTHONUNBUFFERED 1
COPY . .
CMD ["python", "setup.py", "build"]
DOCKER COMPOSE
version: '3.3'
services:
colibri_start:
image: svomtest.svom.eu:5543/colibri-pip:1.0
```


```
---------------VERSION 14.10
DOCKERFILE
FROM python:3.6-slim-buster
RUN apt-get update \
&&apt-getinstall-y--no-install-recommendsgcc \
&&apt-getpurge-y--auto-removegcc
# set up environment
ENV USR svom
COPY . /home/${USR}/app
WORKDIR /home/${USR}/app
RUN useradd -m ${USR} \
&&pipinstall-r requirements.txt \
&&chown-R${USR}:${USR}/home/${USR} \
#root ok for upload fits, svom not possible
&&chmod+x/home/${USR}/app/data/* \
&&rm-rf${HOME}/.cache/pip
VOLUME /home/${USR}/app
ENTRYPOINT ["bin/colibri_svom_interface"]
```


```
 DOCKER COMPOSE

version: '3.3'
services:
colibri_start:
image: rp
volumes:
- ~/Images/fits/:/home/svom/app/data
```




```
//EXEMPLE DE FONCTION DOCKER
sudo systemctl start docker
sudo docker run hello-world

sudo netstat -tanup | grep 5601

//effacer les images
sudo docker rmi $(docker images -q)
sudo docker rm $(docker ps -a -q)

// les volumes
docker volume ls
docker rm idvolume
effacer les volumes
docker volume prune

docker system prune //del all
https://www.codeflow.site/fr/article/how-to-remove-docker-images-containers-and-volumes

//demarrer dans le docker la connexion POSTGRES
docker exec -it postgres psql -U postgres

//demarrer fichier docker-compose 
docker-compose up

$ docker ps
$ docker exec -it [POSTGRES_CONTAINER_ID] /bin/bash ou BASH
root@[CONTAINER_ID] $ su - postgres
root@[CONTAINER_ID] $ psql db

OU 
#psql -h localhost -U test -d testdb

docker inspect [db-container] |grep IPAddress
```


##	LEXIQUE


 |Abbreviations |Détails   |
|--|--|
|BA	|Burst Advocate|
|CAGIRE|	Capturing GRB InfraRed Emission (French NIR camera, IRAP)|
|CEA	|Commissariat à l’Energie Atomique|
|CNES	|Centre National d’Etudes Spatiales|
|DDRAGO	|Mexican Visible Camera, UNAM|
|ESO	|European Southern Observatory|
|FSC	|SVOM French Science Center|
|GCN	|Gamma-ray burst Coordinates Network|
|GFT	|Colibri Ground Follow-up Telescope |
|GCC (or GFT-CC)|	Colibri Ground Follow-up Telescope Control Center|
|GRB	|Gamma-Ray Burst|
|GIC (or IC-GFT)|	Instrument Center for the Colibri Ground Follow-up Telescopes|
|GP1	|GFT Pipeline Level 1 (in GCC)|
|GP2	|GFT Pipeline Level 2 (in FSC)|
|IE, IS	|Instrument Expert, Instrument Scientist|
|(N)IR	|(Near) InfraRed|
|IRAP	|Institut de Recherche en Astrophysique et Planétologie|
|IVORM	|International Virtual Observatory Resource Name |
|LAM	|Laboratoire d’Astrophysique de Marseille|
|MC 	|Mission Center|
|OCS	|Observatory & Observations Control Software (also named PyROS)|
|OHP	|Observatoire de Haute Provence|
|(N)RT	|(Near) Real Time|
|PLC	| (Programmable Logic Controller|
|SA	|System Administrator|
|STB	|Spécifications Techniques des Besoins = Requirements Document|
|SVOM	|Space-based multi-band astronomical Variable Objects Monitor|
|TBD	|To Be Defined|
|TBC	|To Be Confirmed|
|TCS	|contrôle/commande du telescope|
|VHF	|Very High Frequency|
|VTP	|VOEvent Transport Protocol
