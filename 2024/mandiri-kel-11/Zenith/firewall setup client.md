### Menjalankan Perintah Sebagai User Root
```
sudo su
```

### Menampilkan Semua Zona Firewall
```
firewall-cmd --list-all-zone
```

### Membuat Zona Baru Bernama "Wi-fi"
```
firewall-cmd --perment --new-zone=wifi
```

### Membuka Port HTTP (80/TCP)
```
firewall-cmd --perment --new-zone=wifi --add-port=80/tcp
```

### Membuka Port HTTPS (443/TCP)
```
firewall-cmd --perment --new-zone=wifi --add-port=443/tcp
```

### Keluar dari Root
```
exit
```
