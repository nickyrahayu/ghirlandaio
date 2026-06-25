## Install Nginx

```
sudo pacman -S nginx
```
```
sudo systemctl enable --now nginx
```
```
sudo systemctl status nginx
```
```
sudo systemctl start nginx
```
```
sudo su
```
```
mkdir -p /etc/nginx/sites.available
```
```
mkdir -p /etc/nginx/sites.enabled
```
```
nvim /etc/nginx.conf
```
lalu masukin syntax diakhir 
```
include /etc/nginx/sites-enabled/;*
```
esc :wq
```
nvim /etc/nginx/nginx
```
```
ln -sf /etc/nginx/sites-available/slims.conf /etc/nginx/sites-enabled/
