[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/8wqRNR_u)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=19864020&assignment_repo_type=AssignmentRepo)
# 🎓 Projet de Fin d’Année – PLATEFORME QUIZ FASI



## 📌 Objectif du projet

Quiz Fasi est une plateforme d'apprentissage interactif dédiée à l'enseignement supérieur. Elle permet aux professeurs de créer et gérer des quiz personnalisés, et aux étudiants de participer activement à leur formation de manière ludique et engageante.


## 🛠️ Technologies utilisées

•	Backend : PHP 
•	Base de données : MySQL
•	Frontend : HTML5, CSS3, JavaScript
•	Framework CSS : Bootstrap 5
•	Icônes : Font Awesome 6
•	Génération PDF : TCPDF 6.6
•	Serveur : Apache/Nginx


## Structure de la Base de Données

Tables principales :
•	Users (admin)
•	Professeurs
•	Étudiants
•	Quiz
•	Questions
•	Résultats

##Architecture des Dossiers

Quiz Fasi test/
├── admin/           # Interface d'administration
├── Professeur/      # Espace professeur
├── Etudiant/        # Espace étudiant
├── CSS/            # Styles et thèmes
├── JS/             # Scripts JavaScript
├── vendor/         # Dépendances (TCPDF)
├── config. PHP      # Configuration BDD
├── auth.php        # Authentification
└── index. PHP       # Page d'accueil

---

## 🚀 Etapes pour lancer le projet

Étape 1 : Préparation de l'Environnement

1. **Installer Laragon** (si pas déjà fait)
   - Télécharger depuis : https://laragon.org/
   - Installer avec les options par défaut

2. **Vérifier les Extensions PHP**
   - Ouvrir Laragon
   - Aller dans Menu → PHP → Extensions
   - Vérifier que PDO, PDO_MySQL, GD sont activées

### Étape 2 : Configuration de la Base de Données

1. **Démarrer Laragon**
   - Lancer Laragon
   - Cliquer sur "Start All"

2. **Créer la Base de Données**
   - Ouvrir phpMyAdmin : http://localhost/phpmyadmin
   - Créer une nouvelle base de données nommée : `quiz_faculte`

3. **Importer la Structure** (si vous avez un fichier SQL)
   - Sélectionner la base `quiz_faculte`
   - Aller dans l'onglet "Importer"
   - Choisir le fichier SQL et importer

4. **Ou Créer les Tables Manuellement**
   ```sql
   -- Table des administrateurs
   CREATE TABLE users (
       id INT AUTO_INCREMENT PRIMARY KEY,
       nom VARCHAR(100) NOT NULL,
       prenom VARCHAR(100),
       email VARCHAR(255) UNIQUE NOT NULL,
       password VARCHAR(255) NOT NULL,
       role ENUM('admin', 'professeur', 'etudiant') DEFAULT 'admin'
   );

   -- Table des professeurs
   CREATE TABLE professeurs (
       id INT AUTO_INCREMENT PRIMARY KEY,
       nom VARCHAR(100) NOT NULL,
       email VARCHAR(255) UNIQUE NOT NULL,
       mot_de_passe VARCHAR(255) NOT NULL
   );

   -- Table des étudiants
   CREATE TABLE etudiants (
       id INT AUTO_INCREMENT PRIMARY KEY,
       nom VARCHAR(100) NOT NULL,
       email VARCHAR(255) UNIQUE NOT NULL,
       mot_de_passe VARCHAR(255) NOT NULL
   );

   -- Table des quiz
   CREATE TABLE quiz (
       id INT AUTO_INCREMENT PRIMARY KEY,
       titre VARCHAR(255) NOT NULL,
       matiere VARCHAR(100) NOT NULL,
       code_acces VARCHAR(10) UNIQUE NOT NULL,
       professeur_id INT NOT NULL,
       date_creation TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
       FOREIGN KEY (professeur_id) REFERENCES professeurs(id)
   );

   -- Table des questions
   CREATE TABLE questions (
       id INT AUTO_INCREMENT PRIMARY KEY,
       quiz_id INT NOT NULL,
       question TEXT NOT NULL,
       reponse_correcte VARCHAR(255) NOT NULL,
       reponse_incorrecte1 VARCHAR(255) NOT NULL,
       reponse_incorrecte2 VARCHAR(255) NOT NULL,
       reponse_incorrecte3 VARCHAR(255) NOT NULL,
       ordre INT DEFAULT 0,
       FOREIGN KEY (quiz_id) REFERENCES quiz(id) ON DELETE CASCADE
   );

   -- Table des résultats
   CREATE TABLE resultats (
       id INT AUTO_INCREMENT PRIMARY KEY,
       quiz_id INT NOT NULL,
       etudiant_id INT NOT NULL,
       score INT NOT NULL,
       date_passage TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
       FOREIGN KEY (quiz_id) REFERENCES quiz(id),
       FOREIGN KEY (etudiant_id) REFERENCES etudiants(id)
   );
   ```

### Étape 3 : Installation du Projet

1. **Placer le Projet**
   - Copier le dossier `Quiz Fasi test` dans : `C:\laragon\www\`
   - Le chemin final doit être : `C:\laragon\www\Quiz Fasi test\`

2. **Installer les Dépendances**
   - Ouvrir un terminal dans le dossier du projet
   - Exécuter : `composer install`
   - Cela installera TCPDF pour la génération de PDF

### Étape 4 : Configuration du Projet

1. **Vérifier la Configuration**
   - Ouvrir `config.php`
   - Vérifier que les paramètres correspondent à votre environnement :
   ```php
   $host = "localhost";
   $dbname = "quiz_faculte";
   $username = "root";
   $password = "";
   ```

2. **Permissions des Dossiers** (si nécessaire)
   - Dossier `vendor/` : lecture
   - Dossier `CSS/` : lecture
   - Dossier `JS/` : lecture

### Étape 5 : Test de l'Installation

1. **Accéder au Projet**
   - Ouvrir votre navigateur
   - Aller à : `http://localhost/Quiz%20Fasi%20test/`
   - Vous devriez voir la page d'accueil

2. **Tester l'Authentification**
   - Cliquer sur "S'inscrire"
   - Créer un compte professeur avec un email se terminant par `@prof.com`
   - Créer un compte étudiant avec un email se terminant par `@etudiant.com`

## 👥 CRÉATION DES COMPTES INITIAUX

### Compte Administrateur
```sql
INSERT INTO users (nom, prenom, email, password, role) 
VALUES ('Admin', 'Principal', 'admin@quizfasi.com', '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', 'admin');
-- Mot de passe : password
```

### Comptes de Test
1. **Professeur** : `professeur@prof.com` / `password123`
2. **Étudiant** : `etudiant@etudiant.com` / `password123`

## 📁 Structure du projet

```
📦 nom-du-repo
  ┣ 📂 codebase/                # Code source principal du projet
  ┣ 📂 docs/                   # Documentation
  ┃ ┗ 📄 cahier-de-charge.pdf  # Cahier des charges au format PDF
  ┣ 📄 README.md               # Présentation du projet
  ┗ 📄 .gitignore              # Fichier gitignore
```

## 🎥 Démonstration

Lien vers la démonstration vidéo : https://youtu.be/NrVsv7aLLvU
👉 



---


### Résumé des commandes possibles

| Commande               | Effet                                                   |
| ---------------------- | ------------------------------------------------------- |
| `git push origin main` | 🔐 Sauvegarde dans votre dépôt personnel                |
| `git push criagi main` | 🎓 Soumission officielle à CRIAGI                       |
| `git push both main`   | ✅ Soumet dans les **deux dépôts** en une seule commande |


--- 


### Conditions 

Pour que votre projet soit pris en compte, **merci de suivre scrupuleusement toutes les étapes décrites dans ce README**.

* Assurez-vous d’avoir accepté l’invitation GitHub Classroom avant de commencer.
* Copiez et ajoutez correctement le dépôt CRIAGI comme remote secondaire (`criagi` ou `both`).
* Poussez votre code dans le dépôt CRIAGI **avant la date limite**.
* Vérifiez que vos dernières modifications sont bien visibles sur GitHub.
* Tout dépôt non soumis conformément à ces consignes ne sera pas pris en compte.

En cas de difficulté, contactez votre la COMMISSION **avant la deadline**.


---


## 💡 Comprendre Git et GitHub

Cette vidéo vous explique les bases de Git et GitHub : création de dépôt, commits, push/pull, branches, etc.  
Utile pour bien démarrer avec le versioning collaboratif.

👉 [Regarder la vidéo sur YouTube](https://www.youtube.com/watch?v=V6Zo68uQPqE)

---
## 📄 Licence

Projet académique – Usage Strictement Pédagogique.
© 2025 – Université Protestante au Congo - CRIAGI

