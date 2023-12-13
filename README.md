# Assistant Culinaire API

Assistant Culinaire API est un outil conçu pour aider les utilisateurs à gérer leur liste d'ingrédients et offre des suggestions de recettes basées sur ces ingrédients.

## Sommaire

-   [Aperçu](#aperçu)
-   [Caractéristiques](#caractéristiques)
-   [Technologies](#technologies)
-   [Installation](#installation)
-   [Étapes de Configuration](#étapes-de-configuration)
-   [Comment Utiliser](#comment-utiliser)
-   [Exemples d'API](#exemples-dapi)
-   [Contribuer](#contribuer)
-   [Licence](#licence)

## Aperçu

Assistant Culinaire API fournit une méthode simple et sécurisée pour que les utilisateurs gèrent leur liste d'ingrédients, y compris l'ajout de nouveaux ingrédients, la suppression d'ingrédients, et l'obtention de recommandations de recettes basées sur leur liste.

## Caractéristiques

-   **Inscription et Connexion Utilisateur :** Les utilisateurs peuvent s'inscrire et se connecter en utilisant leur adresse e-mail et un mot de passe sécurisé.

-   **Gestion des Ingrédients :** Les utilisateurs peuvent ajouter, supprimer, visualiser tous les ingrédients et vider leur liste complète d'ingrédients.

-   **Recommandations de Recettes :** Des suggestions de recettes sont fournies aux utilisateurs en fonction de leur liste d'ingrédients.

## Technologies

-   Node.js
-   Express.js
-   MongoDB
-   JWT (JSON Web Token) pour l'authentification
-   Axios pour les requêtes HTTP
-   Nodemailer pour l'envoi d'e-mails
-   ...

## Installation

1. Clonez ce dépôt sur votre machine locale.
2. Exécutez `npm install` pour installer les dépendances.

## Étapes de Configuration

1. Créez un fichier `.env` à la racine du projet et configurez les variables d'environnement nécessaires (base de données, clés API, etc.).

2. Assurez-vous d'avoir une instance MongoDB en fonctionnement et accessible.

## Comment Utiliser

1. Exécutez `npm start` pour lancer le serveur.

2. Accédez à l'API à l'adresse `http://localhost:3000` (ou le port que vous avez configuré).

## Exemples d'API

-   **Inscription :**

    --http
    POST /signup/
    Content-Type: application/json

    {
    "email": "utilisateur@example.com",
    "password": "MotDePasseSécurisé123"
    }

-   **Ajout d'Ingrédient :**

    --http
    POST /ingredients
    Authorization: Bearer <votre_token>
    Content-Type: application/json

    {
    "ingredient": "Tomate"
    }

-   **Recommandations de Recettes :**

    --http
    GET /recipes
    Authorization: Bearer <votre_token>
