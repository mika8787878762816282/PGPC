# Plateforme de Gestion de Projets Collaborative (PGPC)
## Vue d'overview du Projet
Ce projet est une **Plateforme de Gestion de Projets Collaborative (PGPC)** conçue pour démontrer une expertise complète en développement Full Stack, en particulier pour les rôles nécessitant une maîtrise de **Node.js, React.js, MySQL, des architectures Microservices et du déploiement sur Google Cloud Platform (GCP)**. Il simule un environnement de travail où les équipes peuvent créer, gérer et suivre des projets, des tâches et des membres.
L'objectif principal de ce projet est de servir de vitrine technique, mettant en évidence la capacité à concevoir et à implémenter des solutions robustes, évolutives et maintenables, en ligne avec les exigences d'un poste de Développeur Full Stack expérimenté.
## Fonctionnalités Clés
* **Gestion des Utilisateurs :** Inscription, connexion, gestion des profils.
* **Gestion des Projets :** Création, modification, suppression de projets. Attribution de membres aux projets.
* **Gestion des Tâches :** Création, attribution, suivi de l'état (à faire, en cours, terminé), priorisation des tâches au sein d'un projet.
* **Collaboration :** Commentaires sur les tâches, notifications basiques.
* **Tableau de Bord :** Vue d'ensemble de l'avancement des projets et des tâches.
## Architecture Technique
Le projet est structuré autour d'une **architecture Microservices** pour garantir la modularité, la scalabilité et la résilience. Chaque service est indépendant et communique via des API RESTful. Une **API Gateway** centralise les requêtes et gère l'authentification.
* **API Gateway (Node.js/Express.js) :** Point d'entrée unique pour toutes les requêtes client, gère l'authentification JWT et route les requêtes vers les microservices appropriés.
* **Service d'Authentification et d'Utilisateurs (Node.js/Express.js) :** Gère l'inscription, la connexion, les profils utilisateurs et l'authentification JWT.
* **Service de Gestion de Projets (Node.js/Express.js) :** Gère la logique métier liée aux projets (création, modification, attribution).
* **Service de Gestion de Tâches (Node.js/Express.js) :** Gère la logique métier liée aux tâches (création, suivi, commentaires).
## Technologies Utilisées
* **Frontend :**
* **React.js :** Bibliothèque JavaScript pour la construction d'interfaces utilisateur dynamiques.
* **HTML5 / CSS3 :** Structure et style de l'application.
* **JavaScript (ES6+) :** Logique côté client.
* **Backend :**
* **Node.js / Express.js :** Environnement d'exécution JavaScript côté serveur et framework web pour les API RESTful.
* **MySQL :** Base de données relationnelle pour la persistance des données.
* **JWT (JSON Web Tokens) :** Pour l'authentification sécurisée.
* **Architecture :**
* **Microservices :** Conception modulaire et distribuée.
* **API Gateway :** Centralisation des requêtes et gestion de l'authentification.
* **Déploiement & Infrastructure :**
* **Google Cloud Platform (GCP) :** Déploiement des microservices (ex: Cloud Run, Compute Engine), gestion de la base de données (Cloud SQL for MySQL).
* **Docker :** Conteneurisation des microservices pour un déploiement cohérent.
* **Git :** Système de contrôle de version.
* **Linux (Ubuntu) :** Environnement de développement et de déploiement.
## Prérequis
Avant de commencer, assurez-vous d'avoir installé les éléments suivants :
* Node.js (version 14 ou supérieure)
* npm ou Yarn
* MySQL (version 8 ou supérieure)
* Docker
* Compte Google Cloud Platform (GCP) avec le SDK gcloud configuré

## Installation et Lancement (Local)
### 1. Cloner le dépôt
```bash
git clone https://github.com/[VotreNomUtilisateur]/PGPC.git
cd PGPC
```

### 2. Configuration de la Base de Données MySQL
* Créez une base de données MySQL nommée `Aqualara`.
* Exécutez le script SQL fourni dans `backend/database/schema.sql` pour créer les tables.

### 3. Configuration des Microservices Backend et API Gateway
Chaque microservice et l'API Gateway nécessitent un fichier `.env` avec les variables d'environnement. Copiez les fichiers `.env.example` et remplissez-les.

```bash
# Pour chaque service (auth-service, projects-service, tasks-service, api-gateway)
cd backend/[nom-du-service]
cp .env.example .env # Remplissez les variables d'environnement
npm install
```
Pour lancer tous les services backend et l'API Gateway :
```bash
npm run dev # Lance tous les services backend et le frontend
```

### 4. Configuration du Frontend
```bash
cd frontend
npm install
cp .env.example .env # Configurez REACT_APP_API_URL pour pointer vers votre API Gateway (http://localhost:3000 en local)
npm start
```
L'application frontend sera accessible sur `http://localhost:3000`.

## Déploiement sur Google Cloud Platform (GCP)
Ce projet est conçu pour être déployé sur GCP en utilisant Docker et Cloud Run pour les microservices, et Cloud SQL pour la base de données MySQL.

### 1. Créer une instance Cloud SQL for MySQL
* Suivez la documentation GCP pour créer une instance MySQL et configurez-la avec la base de données `Aqualara` et le schéma `schema.sql`.
* Mettez à jour les fichiers `.env` de chaque service backend avec les informations de connexion à votre instance Cloud SQL.

### 2. Conteneuriser et déployer les Microservices avec Cloud Run
Pour chaque microservice (auth-service, projects-service, tasks-service, api-gateway) :
```bash
# Naviguez dans le répertoire de chaque service, par exemple :
cd backend/auth-service
# Construire l'image Docker
docker build -t gcr.io/msinstanceprojet/[SERVICE_NAME]:latest .
# Pousser l'image vers Google Container Registry
docker push gcr.io/msinstanceprojet/[SERVICE_NAME]:latest
# Déployer sur Cloud Run
gcloud run deploy [SERVICE_NAME] --image gcr.io/msinstanceprojet/[SERVICE_NAME]:latest --platform managed --region us-central1-c --allow-unauthenticated # --allow-unauthenticated pour l'API Gateway, les autres services peuvent être internes
```
Informations à compléter par vos soins pour le déploiement GCP :
* `[SERVICE_NAME]` : Le nom du service (ex: `auth-service`, `projects-service`, `tasks-service`, `api-gateway`).

### 3. Déployer le Frontend
Le frontend peut être déployé sur un service de stockage statique comme Google Cloud Storage ou Firebase Hosting, ou conteneurisé et déployé sur Cloud Run.
Exemple de déploiement sur Cloud Storage :
```bash
cd frontend
npm run build # Génère les fichiers statiques dans le dossier 'build'
gsutil cp -r build gs://buckket87 # Remplacez par le nom de votre bucket Cloud Storage
gsutil web set -m index.html -e index.html gs://buckket87
```
Informations à compléter par vos soins pour le déploiement du Frontend :
* Mettez à jour `REACT_APP_API_URL` dans le fichier `.env` du frontend pour pointer vers l'URL de votre API Gateway déployée sur Cloud Run.

## Contribution
Les contributions sont les bienvenues ! Veuillez suivre les étapes suivantes :
1. Fork le dépôt.
2. Créez une nouvelle branche (`git checkout -b feature/ma-nouvelle-fonctionnalite`).
3. Effectuez vos modifications et commitez-les (`git commit -am 'Ajout d'une nouvelle fonctionnalité'`).
4. Poussez vers la branche (`git push origin feature/ma-nouvelle-fonctionnalite`).
5. Créez une Pull Request.

## Licence
Ce projet est sous licence MIT. Voir le fichier LICENSE pour plus de détails.

