


# Dokerizing Apps

## Installation Guide for Dockerizing Laravel Apps

Follow these steps to  Dockerize project

```
Download Docker on your local machince "https://docs.docker.com/desktop/install/windows-install/"

```

## Create a Dockerfile and paste code for only  server service 

```
# Use the official PHP image with version 8.2 as the base image
FROM php:8.2

# Set the working directory inside the container
WORKDIR /var/www/html

# Install system dependencies
RUN apt-get update && \
    apt-get install -y \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip

# Install PHP extensions required by Laravel
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# Install Composer (dependency manager for PHP)
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Copy the Laravel project files into the container
COPY . .

# Install project dependencies using Composer
RUN composer install --no-interaction --no-dev --prefer-dist

# Generate the application key
RUN php artisan key:generate

# Expose port 8000 for accessing the Laravel service
EXPOSE 8000

# Start the Laravel service
CMD php artisan serve --host=0.0.0.0 --port=8000

```

## Run this command below on terminal root directory of the laravel App rename serve. Note "Make sure to replace laravel-service with your desired image name"

```
docker build -t laravel-service .
```

## Finally, you can run the Laravel service in a Docker container:


```
 docker run -p 8080:8000 laravel-service
```

## To Create a Docker file to run this laravel service environment in php version 8.2 aslo create a mysql database service so it would be connect to the laravel project. Copy code below into your Dockerfile

```
# Use the official PHP image with version 8.2 as the base image
FROM php:8.2

# Set the working directory inside the container
WORKDIR /var/www/html

# Install system dependencies
RUN apt-get update && \
    apt-get install -y \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip

# Install PHP extensions required by Laravel
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# Install Composer (dependency manager for PHP)
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Copy the Laravel project files into the container
COPY . .

# Install project dependencies using Composer
RUN composer install --no-interaction --no-dev --prefer-dist

# Generate the application key
RUN php artisan key:generate

# Expose port 8000 for accessing the Laravel service
EXPOSE 8000

# Start the Laravel service
CMD php artisan serve --host=0.0.0.0 --port=8000


```
## To set up the MySQL database service, you can use a separate Docker container. Here's an example of docker-compose.yml file, copy code below;


```
version: '3'
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - 8080:8000
    depends_on:
      - db
    volumes:
      - ./Users/michaelmarvellous/dev/comment-app:/var/www/html
  db:
    image: mysql:8.0
    restart: always
    environment:
      MYSQL_DATABASE: laravel_db
      MYSQL_USER: laravel_user
      MYSQL_PASSWORD: 123456
      MYSQL_ROOT_PASSWORD: 123456
    volumes:
      - db-data:/var/lib/mysql

volumes:
  db-data:

```

## Save the above docker-compose.yml file in your project directory. Make sure to replace your_password and your_root_password with your desired passwords. Then, you can start both services using the following command:

```
docker-compose up
```

## To rebuild image service 

```
docker build -t <image_name> .  

```

## After rebuild image service  run command again

```
docker-compose up -d

```



## Congratulation Your laravel App is Dockerized Note to stop server running "Press Ctrl+C to stop the server" on terminal
