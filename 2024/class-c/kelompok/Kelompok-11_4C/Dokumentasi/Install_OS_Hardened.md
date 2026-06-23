## Installasi OS Hardened

## Sambungkan ke WIFI
```
iwctl
```
## Mengecek Wifi 

```
station wlan0 get-networks
```
## Menyambungkan Wifi 
```
station wlan0 connect (nama wifi) 
```
```
exit
```

## Tes Jaringan

```
ping google.com
```

## Membuat partisi
```
cfdisk /dev/[nama partisi]
```
## Membagi partisi sesuai penyimpanan yang ada

```
boot = 3G [EFI System]
root = 47G (menyesuaikan sisa penyimpanan yang ada) [Linux filesystem] 
```

## Set up luks
```
cryptsetup luksFormat  /dev/ [partisi root]
```
```
cryptsetup luksOpen /dev/[partisi_root] [nama_grup]
```
## Membuat physical volume (PV)

```
pvcreate /dev/mapper/[nama_grup]
```
## Membuat volume grub (VG)

```
vgcreate [nama_volume_grup] /dev/mapper/[nama_grup]
```
## cek partisi
```
lsblk
```

## SET UP LVM 

```
lvcreate -L 15G (nama_volume_grup) -n root
```
```
lvcreate -L 10G ((nama_volume_grup) -n vars
```
```
lvcreate -L 1G ((nama_volume_grup) -n vlog
```
```
lvcreate -L 1G ((nama_volume_grup) -n vtmp
```
```
lvcreate -L 1G (nama volume_grup) -n vaud
```
```
lvcreate -L 10G (nama_volume_grup) -n home
```
```
lvcreate -L 9G (nama_volume_grup) -n podman
```

## Membuat Format Partisi
```
mkfs.vfat -F32 -n BOOT /dev/partisi_boot
```
```
mkfs.ext4 /dev/(nama_volume_grup)/root
```
```
mkfs.ext4 /dev/(nama_volume_grup)/vars
```
```
mkfs.ext4 /dev/(nama_volume_grup)/vlog
```
```
mkfs.ext4 /dev/(nama_volume_grup)/vtmp
```
```
mkfs.ext4 /dev/(nama_volume_grup)/vaud
```
```
mkfs.ext4 /dev/(nama_volume_grup)/home
```
```
mkfs.ext4 /dev/(nama_volume_grup)/podman
```

## Mounting partisi dilakukan secara berurutan

```
mount /dev/nama grup/root /mnt
```
```
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/partisi_boot /mnt/boot
```
```
mount --mkdir -o rw,nodev,nosuid,relatime /dev/namavolumegrup/vars /mnt/var
```
```
mount --mkdir -o rw,nosuid,noexec,relatime /dev/namavolumegrup/vlog /mnt/var/log
```
```
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/namavolumegrup/vtmp /mnt/var/tmp
```
```
mount --mkdir -o rw,nosuid,noexec,relatime /dev/namavolumegrup/vaud /mnt/var/log/audit
```
```
mount --mkdir -o rw,nosuid,relatime /dev/namavolumegrup/home /mnt/home 
```
```
mount --mkdir -o rw,nosuid,noexec,relatime /dev/namavolumegrup/podman /mnt/var/lib/containers
```

## periksa kembali partisi

```
lsblk
```

## Instal package linux hardened

```
pacstrap /mnt base intel-ucode linux-hardened linux-hardened-headers linux-firmware mkinitcpio lvm2 cryptsetup sudo curl neovim iwd firewalld pacman podman
```

## Membuat file Fstab

```
genfstab -U /mnt > /mnt/etc/fstab
```

## Menyalin konfigurasi jaringan

```
cp /etc/systemd/network/* /mnt/etc/systemd/network
```

## Menambahkan partisi tmpfs

```
echo "tmpfs /tmp tmpfs defaults,nosuid,noexec,size=1G 0 0" >> /mnt/etc/fstab 
```

## Periksa kembali

```
cat /mnt/etc/fstab
```

## Masuk ke arch chroot

```
arch-chroot /mnt
```
## Membuat Hostname

```
echo "namahostname" > /etc/hostname (kel11)
```

## Time/sinkronisasi waktu

```
ln -sf /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
```
```
hwclock --systohc
```

```
nvim /etc/locale.gen
```

**Setelah masuk ketik “I” kemudian cari dua (en_US) dengan menghapus tagar untuk mengatur bahasa dan lokalisasi**
*Untuk kembali ke tampilan klik “esc” kemudian ketik :wq*

## Melakukan generate lokasi

```
locale-gen 
```
```
locale > /etc/locale.conf
```
```
nvim /etc/locale.conf
```

**Setelah masuk ketik “I” pada bagian paling atas "C" diubah menjadi en_US dan pada bagian paling bawah tambahkan en_US.UTF-8**
*Untuk kembali ke tampilan klik “esc” kemudian ketik :wq*

## Membuat user

```
useradd -m “namauser” (sebelas) 
```

```
passwd “namauser” (sebelas)
```

```
echo "sebelas ALL=(ALL:ALL) ALL" >  /etc/sudoers.d/sebelas
```

## Atur parameter

```
mkdir /etc/cmdline.d
```

```
touch /etc/cmdline.d/{01-boot.conf,02-misc.conf}
```

```
echo "rd.luks.name=$(blkid -s UUID -o value /dev/partisi root)=namagrup root=/dev/namavolumegrup/root" > /etc/cmdline.d/01-boot.conf
```

```
echo "rw" > /etc/cmdline.d/02-misc.conf
```

## Mengatur mkinitcpio

```
nvim /etc/mkinitcpio.conf
```

**cari bagian Hooks yang aktif (tidak memiliki tagar) dan posisinya di atas kata “COMPRESSION” di file itu, kemudian dibaris “Hooks” cari kata “block” lalu ketik "sd-encrypt" setelahnya, dan dilanjutkan dengan mengetik "lvm2"**
*Untuk keluar dari mode insert, klik “esc” kemudian ketik :wq*

```
nvim /etc/mkinitcpio.d/linux-hardened.preset
```

**sesuaikan**
```
# mkinitcpio preset file for the 'linux-hardened' package

ALL_config="/etc/mkinitcpio.conf"
ALL_kver="/boot/kernel/vmlinuz-linux-hardened"
ALL_kerneldest="/boot/kernel/vmlinuz-linux-hardened"

PRESETS=('default')
#PRESETS=('default' 'fallback')

#default_config="/etc/mkinitcpio.conf"
#default_image="/boot/initramfs-linux-hardened.img"
default_uki="/boot/efi/linux/arch-linux-hardened.efi"
#default_options="--splash /usr/share/systemd/bootctl/splash-arch.bmp"

#fallback_config="/etc/mkinitcpio.conf"
#fallback_image="/boot/initramfs-linux-hardened-fallback.img"
#fallback_uki="/efi/EFI/Linux/arch-linux-hardened-fallback.efi"
#fallback_options="-S autodetect"

```
*Untuk keluar dari mode insert, klik “esc” kemudian ketik :wq*

## Menginstall bootctl
```
bootctl --path=/mnt/boot install 
```

```
mkinitcpio -P
```

**Mengaktifkan**

```
systemctl enable systemd-networkd
```
```
systemctl enable systemd-resolved 
```
```
systemctl enable iwd 
```

```
exit
```
```
umount -R /mnt
```
```
reboot
```

## Selesai





