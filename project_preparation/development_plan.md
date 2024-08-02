# Plan de Développement

## Phase 1 : Conception et Architecture
### 1.1. Analyse des besoins
- Comprendre et bien définir les exigences du projet. 
- Définir les concepts (patterns) et outils (Bibliothèques Python, GitHub, base de données, etc.)
- Répondre aux besoins des utilisateurs finaux (qu'est-ce qui sera nécessaire ? Quels utilisateurs ? Bibliothécaire, emprunteur, etc.)
- Décrire les scénarios d'utilisation typiques pour chaque fonctionnalité (Exemples):
    - Ajouter un livre : le Bibliothécaire se connecte pour ajouter un livre à la collection, le système demande les informations requises, le Bibliothécaire saisit les informations, le système vérifie la validité des informations, le système ajoute le livre à la db. Exceptions à prévoir : ISBN existant, informations manquantes ou non valides.
    - Emprunter un livre : un Utilisateur se connecte pour emprunter un livre, le système demande n°ISBN du livre (ou autres informations), le système vérifie la validité des informations, s'il est empruntable (disponible) et validité de l'utilisateur, le système enregistre l'emprunt. Exceptions : livre non disponible, informations et utilisateur non valides. 
- Lister les fonctionnalités principales et secondaires : 
    - Gestion des livres :
    Ajouter un livre (titre, auteur, ISBN (doit être unique), livre papier ou numérique), supprimer un livre (par l'ISBN, vérifier existence livre), mise à jour des informations sur un livre (Vérification des donnés pour qu'elles soient valides), formatage des informations/données.
    - Gestion des utilisateurs : Ajouter un utilisateur (Nom unique), supprimer un utilisateur (si existant).
    - Gestion des emprunts et retours : Emprunter livre (nom emprunteur + ISBN, utilisateur valide, livre présent), retour livre (Nom emprunteur + ISBN)
    - Statistiques : Nombre total de livres, nombre de livres empruntés, nombre total d'utilisateurs, livres les plus empruntés, etc.
- Critères de validation au niveau des données.
- Identifier les erreurs potentielles : erreurs de saisie, enregistrement de l'actualisation de la base de données
- Modélisation du projet (classes, méthodes, interactions).

### 1.2. Conception des classes
Diagramme UML ? Chaque classe doit représenter une entité ou une responsabilité spécifique.
#### Classe Livre :
- Attributs : titre (type *str*), auteur (type *str*), isbn (formatage spéficique de type *str*), type (type *str*)
- Méthodes : 
    - __init__() : Initilise nouveau livre avec les informations renseignées
    - __str__() : Représentation du livre sous forme de chaîne de caractères
    - update() : Mise à jour des informations du livre
#### Classes dérivées LivrePapier et LivreNumerique :
- Héritent de Livre
- Attributs supplémentaires pour LivrePapier et LivreNumerique (s'il y a lieu)
#### Classe Utilisateur :
- Attributs : nom (type *str*)
- Méthodes : 
    - __init__() : Initialise un nouvel utilisateur
    - __str__() : Représentation de l'utilisateur sous forme de chaîne de caractères 
#### Classe Bibliotheque :
- Classe responsable de la gestion de l'ensemble des livres et des utilisateurs
- *Singleton*, pour un accès aux ressources partagées - [design pattern *singleton*](https://refactoring.guru/fr/design-patterns/singleton)
- Attributs : 
    - livres : dictionnaire dont la clé est le numéro ISBN
    - utilisateurs : dictionnaire dont la clé est le nom
    - emprunts : dictionnaire dont la clé est le numéro isbn et la valeur l'utilisateur
- Méthodes : ajouter_livre(), supprimer_livre(), mettre_a_jour_livre(), ajouter_utilisateur(), supprimer_utilisateur(), emprunter_livre(), retourner_livre(), rechercher_livre(), lister_livres(), generer_statistiques()

### 1.3. Définir les relations entre les classes
Diagramme UML :
           
           +----------------------+
           |        Livre         |
           +----------------------+
           | - titre: str         |
           | - auteur: str        |
           | - isbn: str          |
           | - type: str          |
           +----------------------+
           | + __init__(...)      |
           | + __str__(): str     |
           | + update(...)        |
           +----------------------+
                     ^
                     |
        +------------+------------+
        |                         |
+------------------+    +-------------------+
|   LivrePapier    |    |   LivreNumerique  |
+------------------+    +-------------------+
| + __init__(...)  |    | + __init__(...)   |
+------------------+    +-------------------+

           +----------------------+
           |     Utilisateur      |
           +----------------------+
           | - nom: str           |
           +----------------------+
           | + __init__(...)      |
           | + __str__(): str     |
           +----------------------+

           +----------------------+
           |    Bibliotheque      |
           +----------------------+
           | - livres: dict       |
           | - utilisateurs: dict |
           | - emprunts: dict     |
           +----------------------+
           | + __new__(): cls     |
           | + ajouter_livre(...) |
           | + supprimer_livre(...)|
           | + mettre_a_jour_livre(...)|
           | + ajouter_utilisateur(...)|
           | + supprimer_utilisateur(...)|
           | + emprunter_livre(...)|
           | + retourner_livre(...)|
           | + rechercher_livre(...)|
           | + lister_livres(...)  |
           | + generer_statistiques()|
           +----------------------+

## Phase 2 : Implémentation des Fonctionnalités de Base
### 2.1. Implémenter la gestion des livres
#### Ajouter un livre :
- Méthode : ajouter_livre(titre, auteur, isbn, type)
- Ajouter le livre à la collection livres de la bibliothèque.
#### Supprimer un livre :
- Méthode : supprimer_livre(isbn)
- Supprimer le livre de la collection livres par isbn.
#### Mettre à jour les informations d'un livre :
- Méthode : mettre_a_jour_livre(isbn, new_titre=None, new_auteur=None, new_type=None)
- Modifier les attributs du livre correspondant à isbn.

### 2.2. Implémenter la gestion des utilisateurs
#### Ajouter un utilisateur :
- Méthode : ajouter_utilisateur(nom)
- Ajouter l'utilisateur à la collection utilisateurs de la bibliothèque.
#### Supprimer un utilisateur :
- Méthode : supprimer_utilisateur(nom)
- Supprimer l'utilisateur de la collection utilisateurs par nom.

### 2.3. Implémenter les emprunts et les retours de livres
#### Emprunter un livre :
- Méthode : emprunter_livre(isbn, nom)
- Ajouter l'emprunt à la collection emprunts et marquer le livre comme emprunté.
#### Retourner un livre :
- Méthode : retourner_livre(isbn, nom)
- Supprimer l'emprunt de la collection emprunts et marquer le livre comme disponible.

## Phase 3 : Recherche et Statistiques
### 3.1. Implémenter les fonctionnalités de recherche de livres
#### Rechercher des livres :
- Méthode : rechercher_livre(critere, valeur)
- Rechercher des livres par titre, auteur ou isbn et retourner les résultats correspondants.

### 3.2. Implémenter la liste des livres disponibles et empruntés
#### Lister tous les livres :
- Méthode : lister_livres(disponibles=True)
- Lister les livres disponibles si disponibles=True, sinon lister tous les livres.

### 3.3. Implémenter la génération de statistiques (optionnel)
#### Générer des statistiques :
- Méthode : generer_statistiques()
- Calculer et retourner des statistiques comme le nombre total de livres, le nombre de livres empruntés, le nombre total d'utilisateurs, et les livres les plus empruntés.

## Phase 4 : Interface en Ligne de Commande
### 4.1. Développer le menu interactif
- Créer un menu principal avec des options numérotées pour chaque fonctionnalité.
- Utiliser une boucle pour afficher le menu après chaque action jusqu'à ce que l'utilisateur choisisse de quitter.

### 4.2. Utiliser une bibliothèque comme click ou typer
- Installer et configurer la bibliothèque choisie.
- Définir les commandes pour chaque fonctionnalité (ajouter un livre, supprimer un livre, etc.).
- Gérer les entrées utilisateur et les erreurs potentielles.

## Phase 5 : Optimisation et Tests
### 5.1. Optimiser les structures de données et les algorithmes utilisés
- Analyser les performances des opérations de recherche, d'ajout, de suppression, etc.
- Utiliser des structures de données appropriées (par exemple, dictionnaires pour les recherches par ISBN).

### 5.2. Écrire des tests unitaires et des tests d'intégration
- Utiliser une bibliothèque de tests comme unittest ou pytest.
- Tests unitaires : Tester les méthodes des classes individuellement.
- Tests d'intégration : Tester les interactions entre les différentes classes et fonctionnalités.

### 5.3. Corriger les bugs et améliorer la robustesse du code
- Identifier et corriger les erreurs rencontrées lors des tests.
- Ajouter des validations et des gestions d'erreurs pour rendre l'application plus robuste.

## Phase 6 : Documentation et Livraison
### 6.1. Rédiger la documentation utilisateur
- Expliquer comment installer et exécuter l'application.
- Décrire chaque fonctionnalité et comment l'utiliser à partir de l'interface en ligne de commande.

### 6.2. Commenter le code
- Ajouter des commentaires explicatifs pour chaque classe, méthode et section de code complexe.
- Utiliser des docstrings pour décrire les paramètres et les retours des méthodes.

### 6.3. Préparer les livrables
- Code source complet de l'application.
- Documentation utilisateur.
- Rapport de statistiques sur l'utilisation de la bibliothèque (optionnel).

