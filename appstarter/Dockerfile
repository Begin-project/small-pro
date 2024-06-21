# Utiliser l'image officielle PHP avec Apache
FROM php:8.1-apache

# Installer les extensions PHP nécessaires
RUN apt-get update && apt-get install -y libicu-dev \
    && docker-php-ext-install intl mysqli pdo pdo_mysql \
    && docker-php-ext-enable intl

# Ajouter la directive ServerName pour Apache
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf

# Copier les fichiers du projet dans le conteneur
COPY . /var/www/html/

# Configurer le dossier de travail
WORKDIR /var/www/html/

# Configuration d'Apache pour utiliser le dossier public
RUN sed -i 's|/var/www/html|/var/www/html/public|g' /etc/apache2/sites-available/000-default.conf /etc/apache2/apache2.conf

# Exposer le port 8080 (puisque php spark serve utilise ce port par défaut)
EXPOSE 8080

# Commande pour exécuter l'application
CMD ["php", "spark", "serve", "--host", "0.0.0.0", "--port", "8080"]
