
# Colibri Svom Interface

Installé au FSC  (French Science Center).

Ce module reçoit ou extrait ? les images du GP1 ( les données, images L0, L1a et produits des produits scientifiques du CP) , les valide et les écrit dans la Scientifique DB,
EN DÉTAILS :
 - La description des produits scientifiques est partagée par différentes parties du FSC.
	http://svom.iap.fr/fiches/index.php

- Le pipeline doit les connaître pour générer des fichiers de données conformes.
- La base de données scientifique a besoin d'une description des produits à ingérer

### Contrainte
 - gitlab, pytest, sonarcube
 - dockerisation et portainer + dossier distant
 - Application dossier local, et sur serveur FSC
 - Authentification  de la machine qui transmets les images (key) vers le FSC (voir notice )


### Programmes
- Création d’un dossier de Partage de Dossier (UPLOAD_FOLDER)
- envoi d’image par CURL  curl -F "Product=@DT_GFT_FGFT.fits" http://127.0.0.1:5000/upload-image 
- Automatisation envoi sdb
- Utilisation de sdb.io dans le CSI ?

- Création d’un dossier de Partage de Dossier (UPLOAD_FOLDER)
- envoi d’image par CURL  curl -F "Product=@DT_GFT_FGFT.fits" http://127.0.0.1:5000/upload-image 
- Automatisation envoi sdb
- Utilisation de sdb.io dans le CSI ?


### pipeline   http://svom-fsc-0.lal.in2p3.fr:20139 

- receive.py  /upload-product , image sauvée dans dossier
- Contrôle du FITS (program= svom,card=....) 
- Envoi de l’image a la  sdb  http://sdb_api-import/v0/add_product"
- commentaire du code


  Fichier README

- Critére de demarrage/scan = abonnement a nats(si y'a un burst id)
- il faut aussi les fichiers de calibration des l0->l1 soient disponible
- espace de stockage pour le fichier de calibration

