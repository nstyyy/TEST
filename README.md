# Documentation pour le POC Symfony avec WAMP et MySQL

## Table des Matières

1. Introduction
2. Prérequis
3. Installation de l'Environnement de Développement
   - Installation de WAMP
   - Installation de Composer
   - Installation de Symfony CLI
4. Configuration de la Base de Données MySQL
   - Création de la Base de Données et de la Table
5. Création du Projet Symfony
6. Configuration de Symfony pour se Connecter à la Base de Données
7. Création de l'Entité et de la Migration
8. Création du Contrôleur pour Gérer la Requête GET
9. Test de l'Application avec Postman
10. Conclusion
11. Annexes

## Introduction

Ce document a pour objectif de guider les étudiants de niveau bac+1 à réaliser un Proof of Concept (POC) utilisant le framework Symfony. Le POC consiste à développer une application backend qui reçoit une requête GET et renvoie le contenu d'une table MySQL. L'environnement de développement utilisé est WAMP (Windows, Apache, MySQL, PHP).

## Prérequis

Avant de commencer, assurez-vous de disposer des éléments suivants :

* Ordinateur Windows avec les droits administrateurs pour installer les logiciels nécessaires.
* Connexion Internet pour télécharger les outils requis.
* Notions de base en PHP et SQL.
* IDE ou éditeur de texte (comme Visual Studio Code, PhpStorm, ou Sublime Text).

## Installation de l'Environnement de Développement

### Installation de WAMP

WAMP (Windows, Apache, MySQL, PHP) est une solution tout-en-un pour développer des applications web sous Windows.

1. Téléchargement de WAMP :
   * Rendez-vous sur le site officiel de WAMP.
   * Téléchargez la version correspondant à votre système (32 bits ou 64 bits).

2. Installation de WAMP :
   * Exécutez le fichier téléchargé.
   * Suivez les instructions de l'installateur :
     * Choisissez le répertoire d'installation (par défaut : C:\wamp64).
     * Sélectionnez le navigateur par défaut et l'éditeur de texte (optionnel).
   * Terminez l'installation et lancez WAMP.

3. Vérification de l'Installation :
   * Une icône WAMP devrait apparaître dans la barre des tâches (généralement verte).
   * Cliquez sur l'icône et sélectionnez "Localhost" pour vérifier que le serveur fonctionne. Vous devriez voir la page d'accueil de WAMP.

### Installation de Composer

Composer est un gestionnaire de dépendances pour PHP, essentiel pour installer Symfony et ses composants.

1. Téléchargement de Composer :
   * Rendez-vous sur getcomposer.org.
   * Téléchargez le programme d'installation pour Windows.

2. Installation de Composer :
   * Exécutez le programme d'installation.
   * Lors de l'installation, spécifiez le chemin vers l'exécutable PHP de WAMP (généralement C:\wamp64\bin\php\phpX.X.X\php.exe).
   * Terminez l'installation.

3. Vérification de l'Installation :
   * Ouvrez l'invite de commande (CMD).
   * Tapez `composer --version` et appuyez sur Entrée.
   * Vous devriez voir la version de Composer installée.

### Installation de Symfony CLI

Symfony CLI facilite la gestion des projets Symfony.

1. Téléchargement de Symfony CLI :
   * Rendez-vous sur symfony.com/download.
   * Téléchargez l'installateur pour Windows.

2. Installation de Symfony CLI :
   * Exécutez le fichier téléchargé et suivez les instructions.
   * Assurez-vous que le chemin d'installation est ajouté aux variables d'environnement.

3. Vérification de l'Installation :
   * Ouvrez l'invite de commande (CMD).
   * Tapez `symfony -v` et appuyez sur Entrée.
   * Vous devriez voir la version de Symfony CLI installée.

## Configuration de la Base de Données MySQL

### Création de la Base de Données et de la Table

1. Accéder à phpMyAdmin :
   * Ouvrez votre navigateur et allez sur http://localhost/phpmyadmin/.

2. Créer une Nouvelle Base de Données :
   * Cliquez sur "Nouvelle base de données".
   * Entrez un nom pour la base de données, par exemple symfony_poc.
   * Cliquez sur "Créer".

3. Créer une Table :
   * Sélectionnez la base de données symfony_poc.
   * Dans la section "Créer une table", entrez le nom de la table, par exemple products.
   * Définissez le nombre de colonnes, par exemple 3 (id, name, price).
   * Cliquez sur "Exécuter".

4. Définir les Colonnes :
   * id :
     * Type : INT
     * Index : PRIMARY
     * A_I (Auto Increment) : Oui
   * name :
     * Type : VARCHAR
     * Longueur : 255
   * price :
     * Type : DECIMAL
     * Longueur : 10,2

5. Insérer des Données :
   * Allez dans l'onglet "Insérer".
   * Ajoutez quelques enregistrements dans la table products.
   * Cliquez sur "Exécuter" pour enregistrer les données.

## Création du Projet Symfony

1. Ouvrir l'Invite de Commande :
   * Appuyez sur Win + R, tapez cmd, et appuyez sur Entrée.

2. Naviguer vers le Répertoire des Projets :
   * Par exemple, pour créer le projet dans C:\wamp64\www\, tapez :
   ```bash
   cd C:\wamp64\www\
   ```

3. Créer un Nouveau Projet Symfony :
   * Utilisez Symfony CLI pour créer un projet :
   ```bash
   symfony new symfony_poc --full
   ```
   * Cette commande crée un nouveau projet Symfony avec toutes les dépendances nécessaires.

4. Accéder au Répertoire du Projet :
   ```bash
   cd symfony_poc
   ```

## Configuration de Symfony pour se Connecter à la Base de Données

1. Ouvrir le Fichier .env :
   * Dans votre éditeur de texte, ouvrez le fichier .env situé à la racine du projet Symfony.

2. Configurer la Variable DATABASE_URL :
   * Modifiez la ligne suivante avec vos informations MySQL :
   ```
   DATABASE_URL="mysql://username:password@127.0.0.1:3306/symfony_poc?serverVersion=5.7"
   ```
   * Remplacez username et password par vos identifiants MySQL. Par défaut, WAMP utilise souvent root sans mot de passe :
   ```
   DATABASE_URL="mysql://root:@127.0.0.1:3306/symfony_poc?serverVersion=5.7"
   ```

3. Sauvegarder le Fichier .env.

## Création de l'Entité et de la Migration

1. Créer une Entité Product :
   * Dans l'invite de commande, assurez-vous d'être dans le répertoire du projet et exécutez :
   ```bash
   php bin/console make:entity Product
   ```
   * Suivez les instructions pour définir les propriétés :
     * name : string (255)
     * price : decimal (10,2)

2. Créer la Migration :
   * Génère les fichiers de migration basés sur l'entité :
   ```bash
   php bin/console make:migration
   ```

3. Exécuter la Migration :
   * Applique les migrations à la base de données :
   ```bash
   php bin/console doctrine:migrations:migrate
   ```
   * Confirmez en tapant y lorsqu'on vous le demande.

4. Vérification :
   * Retournez sur phpMyAdmin et vérifiez que la table product correspond à l'entité créée.

## Création du Contrôleur pour Gérer la Requête GET

1. Créer un Contrôleur :
   * Dans l'invite de commande, exécutez :
   ```bash
   php bin/console make:controller ProductController
   ```
   * Cela crée un fichier ProductController.php dans src/Controller/.

2. Modifier le Contrôleur :
   * Ouvrez src/Controller/ProductController.php et modifiez-le comme suit :
   ```php
   <?php

   namespace App\Controller;

   use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
   use Symfony\Component\HttpFoundation\JsonResponse;
   use Symfony\Component\Routing\Annotation\Route;
   use App\Repository\ProductRepository;

   class ProductController extends AbstractController
   {
       private $productRepository;

       public function __construct(ProductRepository $productRepository)
       {
           $this->productRepository = $productRepository;
       }

       /**
        * @Route("/api/products", methods={"GET"})
        */
       public function getProducts(): JsonResponse
       {
           $products = $this->productRepository->findAll();
           $data = [];

           foreach ($products as $product) {
               $data[] = [
                   'id' => $product->getId(),
                   'name' => $product->getName(),
                   'price' => $product->getPrice(),
               ];
           }

           return $this->json($data);
       }
   }
   ```

   * Explications :
     * Route : La méthode getProducts est accessible via une requête GET à l'URL /api/products.
     * Récupération des Données : Utilise le ProductRepository pour récupérer tous les produits de la base de données.
     * Transformation des Données : Les données sont transformées en un tableau associatif.
     * Réponse JSON : Retourne les données au format JSON.

3. Vérifier le Routage :
   * Symfony utilise les annotations pour définir les routes. Assurez-vous que le fichier config/routes/annotations.yaml est configuré pour charger les annotations.

## Test de l'Application avec Postman

1. Lancer le Serveur Symfony :
   * Dans l'invite de commande, exécutez :
   ```bash
   symfony server:start
   ```
   * Vous devriez voir une URL locale, par exemple http://127.0.0.1:8000.

2. Ouvrir Postman :
   * Téléchargez et installez Postman si ce n'est pas déjà fait.

3. Créer une Nouvelle Requête GET :
   * Cliquez sur "New" > "Request".
   * Nommez la requête, par exemple Get Products.
   * Entrez l'URL : http://127.0.0.1:8000/api/products.
   * Assurez-vous que la méthode est GET.

4. Envoyer la Requête :
   * Cliquez sur "Send".
   * Vous devriez recevoir une réponse JSON contenant les produits de la table product.

5. Exemple de Réponse :
   ```json
   [
       {
           "id": 1,
           "name": "Produit A",
           "price": "19.99"
       },
       {
           "id": 2,
           "name": "Produit B",
           "price": "29.99"
       }
   ]
   ```

## Conclusion

Ce guide vous a conduit à travers les étapes nécessaires pour créer un POC Symfony intégrant une base de données MySQL sous un environnement WAMP. Vous avez appris à configurer l'environnement, créer un projet Symfony, connecter la base de données, créer une entité, et développer une API simple pour récupérer les données via une requête GET. Ce projet constitue une base solide pour développer des applications Symfony plus complexes.

## Annexes

### Commandes Utiles Symfony

* Créer une Entité :
  ```bash
  php bin/console make:entity
  ```
* Créer un Contrôleur :
  ```bash
  php bin/console make:controller
  ```
* Générer une Migration :
  ```bash
  php bin/console make:migration
  ```
* Exécuter les Migrations :
  ```bash
  php bin/console doctrine:migrations:migrate
  ```
* Lancer le Serveur Symfony :
  ```bash
  symfony server:start
  ```

### Structure des Répertoires Symfony

* src/ : Contient le code source de l'application.
* config/ : Contient les fichiers de configuration.
* public/ : Point d'entrée de l'application (fichier index.php).
* templates/ : Contient les fichiers de templates Twig.
* var/ : Fichiers temporaires et cache.
* vendor/ : Dépendances installées via Composer.

### Bonnes Pratiques

* Sécurité : Toujours valider et assainir les entrées utilisateur.
* Version Control : Utilisez Git pour suivre les modifications de votre code.
* Documentation : Commentez votre code pour le rendre plus compréhensible.
* Tests : Écrivez des tests pour assurer la fiabilité de votre application.
