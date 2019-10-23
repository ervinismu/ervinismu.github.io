---
layout: post
title: "Menjalankan Wordpress, Phpmyadmin dan Mysql menggunakkan docker-compose"
author: "Ervin"
---

# Setup docker-compose
  - Buat folder `mkdir Wordpress` dan masuk kedalam folder `cd Wordpress`
  - Buat file dengan nama `docker-compose.yaml` masukkan script sesuai yang dibawah ini :

```yaml
version: '3.3'

services:
  # DATABASE
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    networks:
      - wpsite

  # PHPMYADMIN
  phpmyadmin:
    depends_on:
      - db
    image: phpmyadmin/phpmyadmin
    restart: always
    ports:
      - '8080:80'
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: password
    networks:
      - wpsite

  # WORDPRESS
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - '8000:80'
    restart: always
    volumes: ['./:/var/www/html']
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
    networks:
      - wpsite

networks:
  wpsite:
volumes:
  db_data:

```
  - Jalankan aplikasi dengan perintah `docker-compose up`

## Mengecek hasil instalasi
  - `http://localhost:8000` ~> membuka wordpress
  - `http://localhost:8080` ~> membuka dashboard phpmyadmin

## Referensi
  - [Docker](https://docs.docker.com/install/) - install docker
  - [Docker Compose](https://docs.docker.com/compose/install/) - install docker-compose
  - [Traversy Media](https://www.youtube.com/watch?v=pYhLEV-sRpY) - Quick Wordpress Setup With Docker




