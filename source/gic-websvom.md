# Gic web svom (FSC)


L’interface du GIC WEB SVOM ’interface utilisateur du Centre instrumental (GIC orienté svom) avec les fonctionnalités suivantes :
- Interface Web et API : Surveillance Colibri des produits SVOM
-  SVOM Alerts : Lister les dernières détections,
-  Shifts : Contrôler et gérer le calendrier des responsables.
-  Observation Planning : 
-  Observation Stratégies : Créer et gérer l’interface entre Colibri-Database et Svom-Database


### Objectifs 

 Réaliser l’interface utilisateur du Centre instrumental (GIC orienté svom) avec les fonctionnalités suivantes :

- 1	Interface Web et API : Surveillance Colibri des produits SVOM
- 2 	SVOM Alerts : Lister les dernières détections,
- 3	Shifts : Contrôler et gérer le calendrier des responsables.
- 4	Observation Planning : 
- 5	Observation Stratégies : Créer et gérer l’interface entre Colibri-Database et Svom-Database


## contraintes d'infrastructure

- Authentification  KEYCLOAK des utilisateurs
- gitlab (Lint, Test, Quality , Build),
- Run the sonar scanner and send the results to SonarQube; 
- sonarqube 80 % taux de couverture, no-smells
- dockerisation et portainer 
- Build the Docker image and publish it on a repository (only for the master and develop branches);
- Module de Connexion User, Admin , Module accessible selon les droits Todo
- Gérer l’interface de COLIBRI Interface avec le FSC (Même utilisateur , même mot de passe?) todo
- Hébergement de page web spécifiques au FSC,
- Maquetter les futures pages du monitoring Colibri,



## Interface Web spécifique FSC 

## SVOM Alerts 

- Lister la dernière detection SVOM
- Liste VO identifiers, VOs, OBS identifiers, Candidat Dt et Po
- Light curve de l'Object ID
- Situation avec ALADDIN du sursaut
- AFFICHER fixer a -0.55, burst-id, date en pm, Burs id + scenario (adapater au product)
Ra et Dec , 




## SHIFT (management des shifts IS) 
- Lister les responsables disponible pour intervention sur un calendrier Mensuel
- Ajouter ou modifier un shift
- Liste des responsables (ajout des responsables) 


## OBSERVATIONS PLANNING





## OBSERVATION STRATEGIES




