

                                      * Rapport de déploiement *
                                     --------------------------




                                     







1 . INTRODUCTION:

Voici un déploiement d'une stack WordPress avec  MySQL en utilisant Docker Compose. J’ai crée un fichier docker-compose.yml, qui execute trois services : WordPress, Redis et MySQL. Ce document explique les étapes de  (configuration, difficultés rencontrées)  pendant l’instalation.




2. ÉTAPES SUIVANTES:



2.1. Préparation de l'environnement


Installation de Docker et Docker Compose : Les outils Docker et Docker Compose ont été installés sur la machine hôte pour permettre le déploiement des conteneurs.
  
  Création des répertoires de données : Les répertoires suivants ont été créés pour stocker les données persistantes :
         /srv/appdata/wordpress pour WordPress.
         /srv/appdata/wordpress/redis pour Redis.
         /srv/appdata/wordpress/db pour MySQL.


   
2.2. Configuration du fichier docker-compose.yml


   Un fichier docker-compose.yml a été créé pour définir les services suivants :
        ◦ WordPress : Service principal pour l'application WordPress.
          .
        ◦ MySQL : Base de données pour stocker les données de WordPress.


   
2.3. Démarrage des services


   La commande suivante a été exécutée pour démarrer les services :
      bash :
      
     docker-compose up -d



2.4. Vérification du déploiement



      
    
  WordPress : Accessible via http://localhost:8989. L'interface est visible,
      
  MySQL : Vérifié en se connectant à : mysql -u wordpress -p.

 CAPTURE D’ECRAN
    

3. CHOIX DE CONFIGURATION:



3.1. Ports
    • WordPress : Le port 8989 de l'hôte est mappé au port 80 du conteneur pour permettre l'accès à l'application.


   
3.2. Volumes
    • WordPress : Le répertoire /var/www/html est monté sur /srv/appdata/wordpress  pour persister 
    • MySQL : Le répertoire /var/lib/mysql est monté sur /srv/appdata/wordpress/db pour persister les données de la base de données.


   
3.3. Variables d'environnement


  WordPress :


   WORDPRESS_DB_HOST : Pointe vers le service db.
   WORDPRESS_DB_USER : Utilisateur de la base de données (wordpress).
   WORDPRESS_DB_PASSWORD : Mot de passe modifie sur nano (Test123).
   WORDPRESS_DB_NAME : Nom de la base de données (wordpress).
   
   MySQL :



  MYSQL_DATABASE : Base de données créée (wordpress).
  MYSQL_USER : Utilisateur de la base de données (wordpress).
  MYSQL_PASSWORD : Mot de passe de l'utilisateur (Test123).
  MYSQL_RANDOM_ROOT_PASSWORD : Génération d'un mot de passe aléatoire pour l'utilisateur root.


4. DIFFICULTEES RENCONTRE:



   
4.1. Problème de permissions sur les volumes
   
   Mysql :   voici un problème rencontre lors de la vérification de l’activation de redis qui m affiche qu il ne peut pas se connecter ,
![Capture d’écran du 2025-02-15 17-10-51](https://github.com/user-attachments/assets/ab5d2654-0bac-4dc8-a1f7-c430c415a513)

   WordPress:  Lors du premier démarrage, WordPress n'a pas pu écrire dans le répertoire monté (/var/www/html) en raison de problèmes de permissions.
   Solution : Les permissions du répertoire /srv/appdata/wordpress ont été modifiées pour permettre à l'utilisateur du conteneur d'écrire dans le répertoire :
      bash
      Copy
      sudo chown -R 1000:1000 /srv/appdata/wordpress



   
4.2. Connexion à la base de données
    • Description : WordPress n'a pas pu se connecter à la base de données MySQL lors du premier démarrage.
    • Solution : Le service db a été vérifié pour s'assurer qu'il était démarré avant WordPress. La dépendance a été explicitement définie dans le fichier docker-compose.yml avec depends_on.






5. CONCLUSION:
   
Le déploiement de la stack WordPress avec MySQL a été réalisé avec succès. Les difficultés rencontrées ont été résolues sauf celle de redis,











                                                       CAPTURE D’ECRAN
                                                       ---------------


   

***La sortie de la commande docker ps montrant vos conteneurs en cours d’exécution et L’exemple d’une page de votre site wordpress et l’interface d’administation de wordpress.


![Capture d’écran du 2025-02-15 16-28-41](https://github.com/user-attachments/assets/a9c03b7c-9556-4d59-8bbb-ee736bf577fb)




![Capture d’écran du 2025-02-15 16-43-03](https://github.com/user-attachments/assets/484e99c0-792b-46a0-81a4-6928dadad7a2)



***La sortie de la commande docker ps montrant vos conteneurs en cours d’exécution et L’exemple d’une page de votre site wordpress et l’interface d’administation de wordpress.

