# 1. Installation et configuration de Symfony
L’installation de Symfony a été réalisée en utilisant Symfony CLI et Composer pour garantir un environnement propre et fonctionnel :

scoop install symfony-cli  # Installation de Symfony CLI
symfony check:requirements  # Vérification des prérequis
symfony new symfony --version="7.2.x"  # Création du projet Symfony (version 7.2)

Symfony CLI simplifie la gestion des projets Symfony.
Vérifier les prérequis système permet d'éviter des erreurs pendant le développement.
Utilisation de Symfony 7.2 pour bénéficier des dernières fonctionnalités et optimisations.

# Installation des dépendances essentielles
Ajout des dépendances nécessaires pour la gestion de la base de données et des entités :

composer require symfony/orm-pack  # Ajout de Doctrine ORM pour gérer MariaDB
composer require --dev symfony/maker-bundle  # Génération rapide des entités et commandes

Doctrine ORM est indispensable pour gérer la base de données relationnelle (MariaDB).
Maker Bundle permet de générer rapidement les entités, contrôleurs et commandes.

# Gestion du dépôt Git et organisation du projet
Création d’un repository distant pour le projet EcoRide-BackEnd, intégré comme submodule dans un repository parent ECF qui contient les dossiers :

Front-End (HTML, SCSS, JavaScript)
Back-End (API Symfony)
Pourquoi ce choix ?

Permet une organisation claire entre le front-end et le back-end.
Facilite la gestion des versions séparées tout en maintenant un repository global.

#  Configuration des bases de données
Ajout des URLs des bases de données MariaDB et MongoDB dans les fichiers .env et .env.local.
.env stocke les variables d’environnement pour la configuration dynamique.
Séparation entre développement et production via .env.local.

# Gestion des migrations et des schémas de base de données

Réinitialisation et mise à jour des bases de données :
del /s /q migrations\*  # Suppression des anciennes migrations
php bin/console doctrine:database:drop --force  # Suppression de la base de données
php bin/console doctrine:mongodb:schema:update  # Mise à jour du schéma MongoDB
Assure une base propre avant de recréer les entités et migrations.
Permet de synchroniser les schémas de base de données avec le code.

# Sécurité et authentification
Ajout du composant Security de Symfony et gestion des rôles utilisateur :
composer require symfony/security-bundle  # Installation du composant de sécurité
composer require lexik/jwt-authentication-bundle  # Gestion des tokens JWT
Security Bundle permet de gérer l’authentification et les permissions.
JWT (JSON Web Token) est utilisé pour sécuriser les échanges entre le client et l’API.
Initialisation des rôles dès le démarrage de l’API.

# Gestion des fixtures et données initiales
Ajout d’orm-fixtures pour générer des données de test :
composer require --dev orm-fixtures  # Installation des fixtures
composer require --dev symfony/test-pack  # Installation des outils de test
Tests unitaires :
php bin/phpunit --filter UtilisateurTest
php bin/phpunit --filter CovoiturageTest
php bin/phpunit --filter AvisTest

Chargement des fixtures dans la base de données :
php bin/console doctrine:fixtures:load

# Gestion du dépôt Git
Assurez-vous que votre dépôt est à jour :
git fetch origin
git checkout main
git pull origin main
git merge developpement
git push origin main
git branch -d developpement
git push origin --delete developpement

# Autres configurations

Suppression du bundle JWT après utilisation :

composer remove lexik/jwt-authentication-bundle

Activation de l'extension ZIP PHP :

Décommenter l’extension zip dans php.ini
extension=zip

tilisation des logiciels pour la conception des interfaces :

Balsamiq pour les wireframes
Figma pour les mockups
Installation de Node.js et de bibliothèques front-end :

npm install bootstrap  # Installation de Bootstrap
npm i bootstrap-icons  # Installation des icônes Bootstrap
Nouveau serveur pour prendre en charge le routage front end javascript :
npm install -g http-server
http-server -c-1 --gzip --proxy http://localhost:8080?/  # Démarrage du serveur

Installation de CORS pour gérer les règles de partage entre origines avec Symfony :

composer require nelmio/cors-bundle  # Installation du bundle CORS

Configuration des règles CORS dans le fichier config/packages/nelmio_cors.yaml.

Installation de AWS SDK :
npm install aws-sdk  # Installation du SDK AWS côté Node.js
composer require aws/aws-sdk-php  # Installation du SDK AWS côté PHP

# Deploiement en local

L'api est disponible dans le repository mbourema/EcoRide-BackEnd: Back end de l'application web EcoRide. Il faut disposer d'un serveur comme apache,
d'une version de php récente, de symfony cli et d'une base de donnée mysql et mongo db. En se rendant dans le dossier symfony, il faut exécuter la
commande symfony server:start -d afin de lancer le serveur et la documentation de l'api sera disponible en locale en entrant /api/doc après l'adresse
du serveur dans un navigateur. Les routes sont toutes utilisables et commentée, mais certaines nécessitent une authentification à l'aide de l'api token
retrouvé dans la table utilisateur pour un utilisateur ayant un role correspondant. Les authentifications peuvent être modifiée dans config/package/security.yaml.
Il faut afin de pouvoir utiliser les différentes route, creer la base de données EcoRide, faire les migrations et exécuter les commande SQL suivantes dans
la base de données mySQL : 
-- Requetes d'insertion pour la table Role

INSERT INTO role (libelle) VALUES ('ROLE_ADMIN'); 
INSERT INTO role (libelle) VALUES ('ROLE_EMPLOYE');
INSERT INTO role (libelle) VALUES ('ROLE_CONDUCTEUR');
INSERT INTO role (libelle) VALUES ('ROLE_PASSAGER');

-- Requetes d'insertion pour la table Marque

INSERT INTO marque (libelle) VALUES ('Alfa Romeo'); 
INSERT INTO marque (libelle) VALUES ('Audi');
INSERT INTO marque (libelle) VALUES ('BMW');
INSERT INTO marque (libelle) VALUES ('Dacia');
INSERT INTO marque (libelle) VALUES ('Fiat'); 
INSERT INTO marque (libelle) VALUES ('Peugeot');
INSERT INTO marque (libelle) VALUES ('Renault');
INSERT INTO marque (libelle) VALUES ('Volkswagen');
INSERT INTO marque (libelle) VALUES ('Mercedes'); 
INSERT INTO marque (libelle) VALUES ('Ford');
INSERT INTO marque (libelle) VALUES ('Nissan');
INSERT INTO marque (libelle) VALUES ('Opel');
INSERT INTO marque (libelle) VALUES ('Volvo');

celà va remplir les tables Marque et Role qui sont essentielles à l'initialisation d'enregistrements dans les autres tables ainsi que pour le fonctionnement du front.

Le front end est disponible à : mbourema/EcoRide-FrontEnd: Front end de l'application web EcoRide. L'extension Live server de visual studio code ne permet pas de charger
le système de routage qu'il comporte. L'utilisation de : npm install -g http-server permet d'actionner le système de routage en locale. Enfin il est nécessaire de réaliser
les installations suivantes :
npm install bootstrap  # Installation de Bootstrap
npm i bootstrap-icons  # Installation des icônes Bootstrap
