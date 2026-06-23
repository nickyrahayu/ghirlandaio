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
 ## Percobaan Install #Kedua
 ```
spacman -S apache mariadb php-fpm unzip php-apache php-intl imagemagick
```
```
systemctl enable httpd
```
>Sampai sini ada tulisan yang berwarna merah tapi tetap dilanjut karena penasaran hasil akhirnya
```
systemctl start httpd
```
```
mariadb-install-db --user=mysql --basedir=/usr --datadir=/var/lib/mysql 
```
```
systemctl enable mariadb
```
```
systemctl start mariadb
```
```
mysql -u root -p
```
```
masukin password
```
>Di akhir penulisan command harus ada titik koma ;
```
create database omeka;
```
```
create user 'lunnns'@'localhost' identified by 'password'; 
```
```
grant all privileges on omeka.* to 'lunnns'@'localhost';
```
```
flush privileges;
quit
```
```
curl -L -O https://github.com/omeka/omeka-s/releases/download/v4.2.0/omeka-s-4.2.0.zip  
```
```
unzip omeka-s-4.2.0.zip -d /srv/http
```
```
mv /srv/http/omeka-s /srv/http/omekas
```
```
chown -R http:http /srv/http/omekas
```  
```
chmod -R 755 /srv/http/omekas
```
```
nvim /srv/http/omekas/config/database.ini
```
``` 
klik  i
user=kelompok11, pass=password, dbname=omeka, host=localhost 
klik esc
:wq
```
```
nvim /etc/php/php.ini
```
```
hapus ; yg exif-gd-iconv-mysqli-pdo mysql
klik esc :wq
```
```
nvim /etc/httpd/conf/httpd.conf
add tagar sama delete tagar, jadi -> #LoadModule mpm_event_module modules/mod_mpm_event.so                                                                                                                                                                                           LoadModule mpm_prefork_module modules/mod_mpm_prefork.so 
tambahin LoadModule php_module modules/libphp.so                                                                                                                                              AddHandler php-script .php 
posisinya setelah
#LoadModule asis_module modules/mod_asis.so
#LoadModule info_module modules/mod_info.so                                                    #LoadModule suexec_module modules/mod_suexec.so  
ganti AllowOverride none jadi AllowOverride ALL
setelah
Options Indexes FollowSymLinks                                                                 # AllowOverride controls what directives may be placed in .htaccess files.                      # It can be "All", "None", or any combination of the keywords:                                #   AllowOverride FileInfo AuthConfig Limit     
tambahin Include conf/extra/php_module.conf 
posisinya abis ini                                                                              <IfModule ssl_module>                                                                         SSLRandomSeed startup builtin                                                                  SSLRandomSeed connect builtin                                                                  </IfModule>    
klik esc :wq
```
```
systemctl restart httpd
```
```
systemctl status httpd
```
Ada tilisan merah gitu keknya error tapi ttp dilanjuitn 
```
ip a
```
```
exit
```
```
reboot
```
                                                                                                                                                                                   

