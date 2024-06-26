
# Small Pro

## Introduction

Ce projet est réalisé par Eddin Adou et Vanessa Quillet. Nos profils GitHub sont respectivement [EddinAdou](https://github.com/EddinAdou) et [studentsvq](https://github.com/studentsvq). Ce dépôt contient un projet basé sur le framework CodeIgniter 4.

## Étapes de création du projet

### Création de l'organisation et du dépôt GitHub

1. Nous avons commencé par créer une organisation sur GitHub.
2. Ensuite, nous avons créé un dépôt GitHub appelé `small-pro`.

### Initialisation du dépôt local

Dans le répertoire `Small pro` sur le bureau, nous avons exécuté les commandes suivantes pour initialiser le dépôt Git local et le lier au dépôt distant :

```bash
echo "# small-pro" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/Begin-project/small-pro.git
git push -u origin main
```

### Résolution des problèmes de connexion SSH

Initialement, nous avons rencontré un problème de connexion SSH, nous avons donc modifié l'URL du dépôt distant pour utiliser HTTPS :

```bash
git remote remove origin
git remote add origin https://github.com/Begin-project/small-pro.git
```

### Installation de CodeIgniter

Nous avons utilisé Composer pour créer un nouveau projet CodeIgniter dans un sous-répertoire `appstarter` :

```bash
composer create-project codeigniter4/appstarter
```

### Synchronisation des modifications avec GitHub

Nous avons ajouté les fichiers générés par Composer au dépôt Git, puis nous avons résolu un conflit en récupérant d'abord les modifications du dépôt distant avant de pousser nos modifications :

```bash
git add .
git commit -m "creation du projet avec codeigniter"
git pull
git push
```

## Structure du projet

Le projet contient les fichiers et répertoires suivants :

- `.idea/` : Configuration du projet pour l'IDE
- `appstarter/` : Répertoire contenant le projet CodeIgniter
- `README.md` : Documentation du projet (ce fichier)
- `composer.json` : Dépendances gérées par Composer

## Configuration et utilisation

Pour configurer et utiliser ce projet, suivez les étapes ci-dessous :

1. Clonez le dépôt :

```bash
git clone https://github.com/Begin-project/small-pro.git
```

2. Installez les dépendances via Composer :

```bash
cd small-pro/appstarter
composer install
```

3. Configurez votre environnement en copiant le fichier `env` et en le renommant `.env` puis en ajustant les configurations nécessaires.

4. Lancez le serveur de développement CodeIgniter :

```bash
php spark serve
```

Accédez à votre application dans un navigateur à l'adresse `http://localhost:8080`.
## Corrections

Conflit de ports

Nous avons rencontré un conflit de ports lors de la configuration de Docker. Le port 3306 était déjà utilisé par une instance locale de MySQL, ce qui empêchait le conteneur MySQL de démarrer.

Solution :
Nous avons modifié le port pour MySQL dans le fichier docker-compose.yml pour utiliser le port 3307 à la place: 
```db:
  image: mysql:5.7
  volumes:
    - db_data:/var/lib/mysql
  restart: always
  environment:
    MYSQL_ROOT_PASSWORD: root
    MYSQL_DATABASE: bdd
  ports:
    - "3308:3306"  # Utilisation du port 3308 pour éviter les conflits
    ```


Problèmes de configuration dans docker-compose.yml et Dockerfile

Il y avait des problèmes de configuration dans les fichiers docker-compose.yml et Dockerfile, notamment l'absence de certaines extensions PHP nécessaires et des conflits de ports entre les services.

Problèmes de permissions et de contexte de build:

Solution :
```# Utiliser l'image officielle PHP avec Apache
FROM php:8.1-apache

# Installer les extensions PHP nécessaires
RUN apt-get update && apt-get install -y libicu-dev \\
    && docker-php-ext-install intl mysqli pdo pdo_mysql \\
    && docker-php-ext-enable intl

# Ajouter la directive ServerName pour Apache
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

# Copier les fichiers du projet dans le conteneur
COPY . /var/www/html/

# Configurer le dossier de travail
WORKDIR /var/www/html/

# Exposer le port 8080
EXPOSE 8080

# Commande pour exécuter l'application
CMD ["php", "spark", "serve", "--host", "0.0.0.0", "--port", "8080"]
```

```services:
  web:
    build: .
    ports:
      - "8080:8080"
    volumes:
      - .:/var/www/html
    environment:
      CI_ENVIRONMENT: development
    depends_on:
      - db
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: bdd
    ports:
      - "3308:3306"
  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    restart: always
    ports:
      - "8081:80"
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: root

volumes:
  db_data:
```

# Problèmes de permissions et de contexte de build

Certains fichiers avaient des permissions incorrectes ou spéciales qui causaient des erreurs lors du build.

Solution :
Nous avons utilisé un fichier .dockerignore pour exclure les fichiers non nécessaires et éviter les erreurs lors du build:

```# Ignorer les dossiers spécifiques
.git
node_modules
.vscode

# Ignorer les fichiers spécifiques
Dockerfile
docker-compose.yml
*.md
```

## Contributions

Les contributions à ce projet sont les bienvenues. Veuillez suivre le processus de demande de tirage (pull request) pour soumettre vos modifications.

## Auteurs

- **Eddin Adou** - [EddinAdou](https://github.com/EddinAdou)
- **Vanessa Quillet** - [studentsvq](https://github.com/studentsvq)

## Licence

Ce projet est sous licence MIT. Consultez le fichier `LICENSE` pour plus de détails.
