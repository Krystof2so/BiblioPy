# Plan de Développement

## Phase 1 : Conception et Architecture
### 1.1. Analyse des besoins
- Comprendre et bien définir les exigences du projet. 
- Définir les concepts (patterns) et outils (Bibliothèques Python, GitHub, base de données, etc.)
- Répondre aux besoins des utilisateurs finaux (qu'est-ce qui sera nécessaire ? Quels utilisateurs ? Bibliothécaire, emprunteur, etc.)
- Lister les fonctionnalités principales et secondaires : 
    - Gestion des livres :
    Ajouter un livre (titre, auteur, ISBN (doit être unique), livre papier ou numérique), supprimer un livre (par l'ISBN, vérifier existence livre), mise à jour des informations sur un livre (Vérification des donnés pour qu'elles soient valides).
    - Gestion des utilisateurs : Ajouter un utilisateur (Nom unique), supprimer un utilisateur (si existant).
    - Gestion des emprunts et retours : Emprunter livre (nom emprunteur + ISBN, utilisateur valide, livre présent), retour livre (Nom emprunteur + ISBN)
    - Statistiques : Nombre total de livres, nombre de livres empruntés, nombre total d'utilisateurs, livres les plus empruntés, etc.
- Critères de validation au niveau des données.
- Identifier les erreurs potentielles : erreurs de saisie, enregistrement de l'actualisation de la base de données
- Modélisation du projet (classes, méthodes, interactions).

### 1.2. Conception des classes
#### Classe Livre :
- Attributs : titre, auteur, isbn, type
- Méthodes : __init__(), __str__(), update()
#### Classes dérivées LivrePapier et LivreNumerique :
- Héritent de Livre
- Attributs supplémentaires pour LivrePapier et LivreNumerique (s'il y a lieu)
#### Classe Utilisateur :
- Attributs : nom
- Méthodes : __init__(), __str__()
#### Classe Bibliotheque 
- *Singleton*, pour un accès aux ressources partagées (?)
- Attributs : livres, utilisateurs, emprunts
- Méthodes : ajouter_livre(), supprimer_livre(), mettre_a_jour_livre(), ajouter_utilisateur(), supprimer_utilisateur(), emprunter_livre(), retourner_livre(), rechercher_livre(), lister_livres(), generer_statistiques()

### 1.3. Définir les relations entre les classes
- La classe Bibliotheque contient des collections de Livre et Utilisateur.
- Les interactions entre Utilisateur et Livre passent par la classe Bibliotheque.

### 1.4. Mettre en place le design pattern Singleton pour Bibliotheque
- Assurer qu'il n'y a qu'une seule instance de la classe Bibliotheque dans l'application.

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

