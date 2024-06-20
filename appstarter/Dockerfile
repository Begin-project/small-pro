# Utiliser l'image officielle PHP avec Apache
FROM php:8.1-apache

# Installer les extensions PHP nécessaires
RUN docker-php-ext-install mysqli pdo pdo_mysql

# Ajouter la directive ServerName pour Apache
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

# Copier les fichiers du projet dans le conteneur
COPY . /var/www/html/

# Configurer le dossier de travail
WORKDIR /var/www/html/public/

# Configuration d'Apache pour utiliser le dossier public
RUN sed -i 's|/var/www/html|/var/www/html/public|g' /etc/apache2/sites-available/000-default.conf /etc/apache2/apache2.conf

# Exposer le port 80
EXPOSE 80
