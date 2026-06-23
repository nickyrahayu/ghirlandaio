## Percobaan Install #Pertama

## 1. Deploy omeka pada podman
```
sudo pacman -S  podman-compose
```

# Compose
```
mkdir omeka-project
```

```
nvim compose.yml
```

# Config compose.yml Yyya
```
version: '3'

services:
  db:
    image: docker.io/library/mariadb:10.11
    container_name: omeka-db
    environment:
      MYSQL_ROOT_PASSWORD: 11_root_pass
      MYSQL_DATABASE: omeka
      MYSQL_USER: omeka
      MYSQL_PASSWORD: omeka 
    volumes:
      - db_data:/var/lib/mysql:Z
    restart: always

  omeka:
    image: docker.io/omeka/omeka-s:latest
    container_name: omeka-app
    ports:
      - "3306:80"
    volumes:
      - files_data:/var/www/html/files:Z
    depends_on:
      - db
    restart: always

volumes:
  db_data:
  files_data:
```
# Aktifkan container 
```
podman-compose up -d
```
Sampai step ini sudah eror 
```
podman ps -a
```
GAGAL TOTAL :((


                                                                                                                                                                                   

