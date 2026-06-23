## Setup podman dan install apk

# server1
```
sudo systemctl enable –global podman
```
```
sudo nvim /etc/containers/registrai
```
```
sudo nvim /etc/sysctl.d/[custome].conf
```
**setelah masuk ke tampilan itu kosong pencet “I” lalu ketik :**

```
kernel.unprevillaged_userns_clone = 1
```
*Untuk kembali ke tampilan klik “esc” kemudian ketik :wq*

```
sudo sysctl –system
```
## Membuat folder untuk apk

```
sudo su
```
**Membuat folder menyimpan apk**
```
mkdir [namafolder]
```
## Mindahin file
```
cd [namafolder]
```

**Bisa ke browser ketik atom compose 2.9 buat docker compose yang commandnya:**

```
git clone -b qa/2.X https://github.com/artefactual/atom.git atom
```
**mengecek file yang sudah didownload tadi dengan menggunakan command**

```
ls
ls atom/
```
```
cd atom
```
```
export COMPOSE_FILE="$PWD/docker/docker-compose.dev.yml"
```
```
podman compose -f docker /docker-compose.dev.yml up -d
```
```
podman ps -a
```
*buat ngecek atom nya ada isinya atau engga*
```
systemctl stop firewalld
```
```
masuk kebrowser, masukin http://[ip server:63001]
```

# admin
```
ssh (username)@(ip server) 
```

## Status firewall
```
sudo systemctl status firewalld
```
## Liat service apa aja yang udah di daftarin sama firewall
```
sudo firewall-cmd –list-all-zone
```
