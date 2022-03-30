# Installation

TODO
Tools
  - Docker compose
  - nodejs
  - python

## GIC SERVER

- Base de données
- Création du serveur Nodejs
- Mise en place de l'api
- Composants 
  - Connection au PLC
  - Composant Insertion Calibration
  - Composant Incertion Sensors
  - Composant Insertion Seeing
  - Explorateur de Fits
  - Rsync
  - Fake data

### Requirements

Serveur OHP 134.158.16.36

- Centos
- PostgreSQL 12 
    user root mdp *** port 5432
- nodejs

### Base de données


#### Installer POSTGRES

```json
    sudo apt install postgresql
    sudo su - postgres
    pwd
    psql -l
    psql 
    postgres=# CREATE USER root;
    ALTER ROLE root WITH CREATEDB;
    CREATE DATABASE test OWNER root;
    ALTER USER root WITH ENCRYPTED PASSWORD '*****';
    \q
    psql test
    sudo service postgresql restart
    //se connecter a la base 
    psql -U root test
    or psql -h localhost -U root -d colibridb
```

en cas de probleme d'acces a la base de donnée , changer les parametres sur le fichier pg_hba.conf

```
etc/postgresql/11/main/pg_hba.conf
```

Modifier les autorisations de l'utilisateur de cette facon

```
#Database administrative login by Unix domain socket
local   all             postgres                                trust
# TYPE  DATABASE        USER            ADDRESS                 METHOD
# "local" is for Unix domain socket connections only
local   all             all                                     md5
```

### Création du serveur Nodejs 

Prépartion des dossiers partagés 			
-	|--incoming_calibrations (nodejs scan ce dossier toute les 15 secondes, insere les données dans la bdd, et deplace le fichier traité dans     ->archivecalibration)
- |--incoming_sensors (nodejs scan ce dossier toute les 15 secondes, insere les données dans la bdd, et deplace le fichier traité dans ->archivesensors)
- |--archivecalibration (fichiers des données inserées dans la bdd)
- |--archivesensors (fichiers des données inserés dans la bdd)
- |--archivesensors (fichiers des données inserés dans la bdd)
- |--server (server nodejs, fichier api, et de configuation)
- |-- webapp (webUi)
	
#### Installer NODEJS

```json
     curl -sL https://rpm.nodesource.com/setup_10.x | sudo bash -
     sudo yum install nodejs
```

##### requirements
*requirements :*
- yarn
- node >= 8.11.3
- sequelize-cli
- PostgreSQL

##### Setup project
add `.env` file in the backend folder of project and add following lines
```text
DB_USER=
DB_PASS=
DB_NAME=colibridb
DB_HOST=127.0.0.1
DB_DIALECT=postgres
DATABASE_URL=postgres://USER:MDP@localhost:5432/colibridb
JWT_SECRET='Colibri'
```

#### Installer YARN

```     
     curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
     sudo rpm --import https://dl.yarnpkg.com/rpm/pubkey.gpg
     sudo yum install yarn
```

#### Installation from source

- port 5000

You can install gic-server either from website or the from the git repo TODO.

```json
    git clone https://drf-gitlab.cea.fr/svom/gic/gic-server
    cd gic-server
    yarn                 //installation des modules
    nodemon              //demarrage de l'application
```

Visit [https://localhost:3000/gic-server/](http://localhost:5000/gic-server)

####  Démarrage manuel du serveur

/server
 run `nodemon` into the backoffice folder (or the root project)

####  Démarrage automatique du serveur via  PM2

```json
sudo npm install pm2@latest -g
```

- pm2 list des process en cours (se placer sur profil root)
- pm2 monitor (logs en cours des applis tournant sur pm2)
- pm2 restart "2" (redemarre le programme 2)
		
#### Seveur OHP on marsvom1

Link server  /var/opt/nodejscolibri

Espace de stockage 
  -   data/                       //GIC WEB, SVOM-DOC, 
  -   /usr/local/var/coatli/bin
  -   data_storeA/coatli          //dark, flat_bank sextractor, 


### Fichiers JSON

Les fichiers JSON contiennent des données horodatées, ils proviennent de différentes sources:
- 	Les fichiers JSON sont créés par un automate PLC (programmable Logic Controller). Dans ce cas, TCS ne fait que collecter et envoyer les fichiers.
-	Concernant le Télescope, GP1 et les détecteurs, TCS récupère les données de ces pièces et crée des fichiers JSON avec ces données et les envois.
-	Le TCS transmets aussi  des liens vers des images d'étalonnage . Comme décrit ci-dessus .


Les fichiers JSON sont accumulées dans un fichier synchronisé avec point de terminaison en France toutes les 5 minutes avec l’outil rsync dans la Colibri Database (CDB)


#### Contraintes

-	la base de données peut être délocalisée et gérée indépendamment.
-	La modélisation de la base de données est effectuée dans ce processus.
-     La modélisation de la base de données concerne les données du PLC et, en partie, ceux du télescope.
-	 Plusieurs données d'état sont manquantes, notamment celles du télescope et des détecteurs.
-	Faire évoluer le modèle de base de données conformément aux nouvelles définitions de données d'état du détecteur et du télescope


#### JSONB :


	Inconvénients :
les données sont stockées sous une forme binaire,	-  Occupe plus d’espace(plus grande empreinte),
Enorme gain de rapidité dans la recherche des mots clés	-  requêtes agrégées sont plus lentes ( COUNT, AVG, SUM)
	-  Attention a la valeur NULL non acceptée dans JSONB, ainsi que les caractères non ASCII,
	- Conception des requêtes un peu compliqué
    - Proposition du LAM indexation des fichiers

Les échantillons indexés ne  correspondent pas aux besoins d’utilisation des données.
Aucune recherche n’est possible, 
Perte de performance (téléchargement et re-indexation des fichiers),



### Composants

#### Insertion données calibrations

1. récupère les fichiers dans incoming_calibrations
2. insère les données dans la bdd
3. déplace le fichier dans archivecalibration

il est possible de générer des jsons qui seront automatiquement insérer dans le dossier de scan "incoming_calibrations"
il faudra modifier la date pour choisir les périodes générées (/var/opt/nodejscolibri/server/seeds/103_calibrations.js  -> dateInsertion = new Date(2020, 01, 01);)

##### Folder Scan incoming_calibrations

incoming_calibrations -->archive_calibration
- les fichiers deposés dans ce dossier seront scanné,
- exploités et inserés dans la Bd 
- et deposé dans le dossier


#####  mode manuel :

```
cd modules/jsonb/
node insertCalibration.js
```
he insert all the data in incoming_calibrations

#####  mode automatique :

```
cd modules/jsonb/

 pm2 start insertCalibration.js --name insertCalibration
```




#### Connect Plc
you can read the plc and transfert data in Folder Scan


#####  mode manuel :

```
cd modules/plc/
node connectPlc.js
```


#####  mode automatique :

```
cd modules/plc/

 pm2 start connectPlc.js --name connectPlc
```

#### Insertion des données sensors

Demarrage du PLC Obligatoire

1. récupère les fichiers dans incoming_sensors
2. insère les données dans la bdd
3. déplace le fichier dans archivesensors

il est possible de générer des jsons qui seront automatiquement insérer dans le dossier de scan "incoming_sensors"

il faudra modifier la date pour choisir les périodes générées (/var/opt/nodejscolibri/server/seeds/91_tcs  , 92_tcs ... 95_instrument.js  -> dateInsertion = new Date(2020, 01, 01);)


##### Folder Scan incoming_sensors
incoming_sensors -->archive_sensors
- les fichiers deposés dans ce dossier seront scanné,
- exploités et inserés dans la Bd 
- et deposé dans le dossier

#####  mode manuel :

```
cd modules/jsonb/
node proinsertJson.js
```


#####  mode automatique :

```
cd modules/jsonb/

 pm2 start proinsertJson.js --name proinsertJson
```



#### Insertion des données seeings


Connection au ftp pour récuperer le fichier seing_data.txt qui est mis a jour toutes les 51s.

https://www.colibri-obs.org/wp-includes/images/ftp_colibri/SeeingMonitor/

1. récupère les fichiers dans incoming_seeings
2. insère les données dans la bdd
3. déplace le fichier dans archiveseeings

il est possible de générer des jsons qui seront automatiquement insérer dans le dossier de scan "incoming_seeings"

il faudra modifier la date pour choisir les périodes générées (/var/opt/nodejscolibri/server/seeds/91_tcs  , 92_tcs ... 95_instrument.js  -> dateInsertion = new Date(2020, 01, 01);)


##### Folder Scan incoming_seeings
incoming_seeings -->archive_seeings
- les fichiers deposés dans ce dossier seront scanné,
- exploités et inserés dans la Bd 
- et deposé dans le dossier

#####  mode manuel :

```
cd modules/perfo/
node connectSeeing.js
node insertJsonperfo.js
```


#####  mode automatique :

```
cd modules/perfo/

 pm2 start connectSeeing.js --name connectSeeing
 pm2 start insertJsonperfo.js --name insertJsonperfo
```

##### mode automatique PM2

















## GIC WEBS 

Pour demarret l'application GIC-webs

- cloner le repository et demarrer l'application
 	

### Environnement FSC



| Environnement        | Tools      |Authentification      |
| ------|-----|-----|
| - gitlab|- Docker compose | KEYCLOAK |
| - pytest|- nodejs||
| - sonarcube|- python | |
| - portainer ||   |
| - docker|-----| |


### Installation from repository

- port 3000

You can install gic-server either from website or the from the git repository.
 run the following commands:


```
    git clone https://drf-gitlab.cea.fr/svom/gic/gic-webs.
    cd gic-webs
    yarn && yarn start`
```

Visit [https://localhost:3000/gic-web/dashboard](https://localhost:3000/gic-web/dashboard)



#### Testing on FSC

The web UI should be visible
here [https://fsc.svom.org/gic-web](https://fsc.svom.org/gic-web).


### Start with docker
    docker-compose up
Visit [https://localhost:3000/gic-web/dashboard](https://localhost:3000/gic-web/dashboard)



## GIC WEBC

### Installation from repository

- port 3000

You can install gic-server either from website or the from the git repo TODO.

 ```
    git clone https://drf-gitlab.cea.fr/svom/gic/gic-web.
    cd gic-web
    yarn && yarn start
```

Visit [https://localhost:3000/gic-web/dashboard](https://localhost:3000/gic-web/dashboard)


##### Testing
The web UI should be visible

192.168.100.27 on Vpn server vpn.colibri-obs.org
here [http://vpn.colibri-obs.org](http://vpn.colibri-obs.org).


### Start with docker

```
    cd gic-web
    docker-compose up
```
Visit [https://localhost:3000/gic-web/dashboard](https://localhost:3000/gic-web/dashboard)

#### Connection VPN OHP

Installation "FortiClient VPN only"
here [https://www.fortinet.com/support/product-downloads#vpn](https://www.fortinet.com/support/product-downloads#vpn).


FORTICLIENT vpn.colibri-obs.org 443			
Connexion user ssh user@192.168.100.27  

reinitialisation du mot de passe
 https://ldap.colibri-obs.org/


##### contraintes sécurité

A l'ohp nous avons derrière le vpn (vpn.colibri-obs.org)

 Gic-server(nodejs, api 192.168.100.27:5001)
 Gic-colibri(react 192.168.100.27:3000)

 Problème le serveur n'est pas en https et ne posséde pas de certificat :

 Solution pour alimenter le Gic-webs 

 ```
     location /colibri/ {
        auth_request            /jwt-validate/hint=colibri;
        proxy_pass              http://svom-fsc-1.lal.in2p3.fr:8052/;
    }
    location /gic-web/colibri-obs/ {
        proxy_pass              http://vpn.colibri-obs.org:5001/;
    }
    location /gic-web/ {
        proxy_pass              http://svom-fsc-1.lal.in2p3.fr:8053/;
    }
```

## COLIBRI SVOM INTERFACE


This repository contains the source code of the server implementing the API to import ans control of products Fits  before insertion into the Sdb



### Download source

You can install gic-server either from website or the from the git repo TODO.
    
```
    git clone https://drf-gitlab.cea.fr/svom/gic/gic-webs.
    cd gic-webs
    python3 -m venv .venv
    source .venv/bin/activate


```

### 1. File organisation
    ├── src                    #  Source files 
    │   ├── receive.py         # Load the product
    │   ├── validation.py      # Controls header
    │   └── sendsdb.py         # Send the product to Sdb
    └── README.md



### 2. Testing in Local

        change upload_folder = '/home/svom/app/data'
        upload_folder = '/home/USER/travail/svom/versions/colibri-interface/data'

docker-compose.yml
version: '3.3'
services:
  colibri_start:
      build:
        context: .
        dockerfile: Dockerfile
      ports:
        - '5000:5000'
      volumes:
        - ~/Images/fits/:/home/svom/app/data

#### 2.a Installation 

```shell
$ python3 -m venv .venv 
$ source .venv/bin/activate   
$ pip install -r requirements.txt 
```

#### 2.b Testing with installation

```shell
$ pip install pytest coverage
$ python -m pytest
$ coverage run --source src -m pytest tests 
```

#### 2.c Start API

Configure UPLOAD_FOLDER in src/receive.py
the image folder is in data

```shell
$ python src/receive.py                                             
```

### 3. Continuous integration

The `.gitlab-ci.yml` file contains the configuration to run Gitlab continuous
integration to:

- Run the tests;
- Run the sonar scanner and send the results to SonarQube;
- Build the Docker image and publish it on a repository (only for the `master`
    and `develop` branches);

### 4. Building and Running Docker image in FSC

```shell
$ docker-compose up
```
#### 4.a Test Colibri-interface status

[Home page] : https://fsc.svom.eu/colibri  response is : The response should be 'Colibri interface is alive'


#### 4.a Test with curl

 curl -F "Product=@DT_GFT_FGFT.fits" https://fsc.svom.eu/colibri/upload-product  -H "Authorization: Bearer ${token}"


### 5 Volume

The shared volume is data in container, and portainer_data in portainer
