# Data Challenge

![gic components](images/schema1.png)

## DC2 

- Hébergement de page web spécifiques a l'ijcLAB (https://fsc.svom.org/)

### Activités Colibri SVOM au CPPM


 |N° | Contenu   |
|--|--|
| 	 Nos réalisations pour DC2 |• Le principal objectif est de tester les parties SVOM de nos activités|
||• Le télescope Colibri ne produit pas encore d’images|
||• Test de la chaine de fabrication des produits basée sur des observations simulées|
|Principales étapes de notre test :|• La réception de VO Events du FSC, |
||le premier est le trigger de|
|la fabrication des produits| La fabrication des produits scientifiques GFT à partir de|
|Colibri-products|produits bruts issus du GP1, ces produits bruts sont issus|
||d’images d’une alerte sur d’autres télescopes|
|| Réception des produits au FSC, vérification, envoi en SDB|
|| Vérification et visualisation des produits|


### Etat de développement des processus



 |N° | Contenu   |
|--|--|
|• Colibri-interface|Livré avec tag dc-2|
|• Colibri-products|Module prêt, tourne au CPPM dans un container|
||En attente de démarrage par un VO Event du FSC|
|• Gic-webs|Module prêt, tourne au CPPM,|
||Utilisé pour vérifier l’écriture des produits en SDB|
||Permet de voir évoluer les propriétés des produits|


### Parties non testées


 |N° | Contenu   |
|--|--|
||• Réception des VO Events au CPPM testée|
||• Emission des VO Events du FSC constatées|
||• Adéquation à tester entre les VO Events et les observations simulées|
||Coordonnées du GRB, très important|
||o Temps, on peut s’adapter et gérer un décalage|




## DC3

- Utilisation NatsIo, Sdbio, JestreamIo
- Hébergement de page web spécifiques a l'ijcLAB (https://fsc.svom.org/)


### Et après
Colibri-interface: améliorations de la validation des produits avant
insertion en SDB
Consultation de la base de données VO Events
Vérification des IDs VO Event et produits scientifiques
Gic-webs: réception des VO Events en même temps que les produits
Affichage des 2 sources simultanée
Gérer l’autentification
Colibri-products: développement spécifique pour dc-2
Certains modules seront réutilisés dans l’application finale