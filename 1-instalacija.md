# Instalacija
## Boot sa USB uređaja
Pre svega, boot-ovati instalacioni USB.
## Internet za vreme instalacije
### Wi-Fi (Preskočiti ako je računar povezan na internet preko kabla)
#### Listanje dostupnih SSID-a i povezivanje
```sh
iwctl
station wlan0 get-networks
station wlan0 connect naziv_mreze
exit
```

### Provera povezanosti na internet
```sh
ping google.com
```

## Diskovi, particije i fajl-sistemi
### `fdisk`
#### Listanje diskova i odabir diska
```sh
fdisk -l
fdisk dev_file_diska
```
#### Komande
Izmene se ne upisuju na disk pre nego što se izda `w` komanda.
- `p` - Listanje particija
- `g` - Nova GPT tabela (reset tabele particija)
- `n` - Nova particija
    - Krajnji sektor particije (veličina particije) se može zadati sa `+1GB`, `+500MB`, itd.
- `t` - Definisanje tipa particije
    - `uefi` - EFI particija
    - `swap` - Swap particija
    - `linux` - Linux filesystem particija (default)
- `w` - Upisivanje izmena na disk i izlaz iz `fdisk`

#### Opšti layout
- **1GB** - `uefi`
- **Veličina RAM-a** - `swap`
- **Proizvoljna veličina** - `linux` (ovo će biti root particija)
- **Ostale particije po želji** - `linux` (/home, /var, itd...)

ostale `linux` mogu biti na različitim diskovima

#### Formatiranje
```sh
mkfs.fat -F 32 dev_file_EFI_particije
mkswap dev_file_swap_particije
mkfs.ext4 dev_file_root_particije
```
Sve ostale `linux` particije formatirati sa `mkfs.ext4`

### Mount-ovanje
```sh
mount dev_file_root_particije /mnt
mount --mkdir dev_file_EFI_particije /mnt/boot
swapon dev_file_swap_particije
```
Ako postoje dodatne particije, montirati ih na odgovarajuće lokacije unutar `/mnt`, npr:
```sh
mount dev_file_home_particije /mnt/home
```

## Instalacija sistema i alata
```sh
pacman -Syu
```
#### Neophodni paketi:
- `base`, `linux`, `linux-firmware` - Sam operativni sistem 
- `sudo` - Standardni alat za izdavanje komandi kao `root`
- `networkmanager` - Drajver za mrežu
- `ufw` - Firewall
- `pipewire`, `pipewire-pulse`, `pipewire-alsa`, `pipewire-jack`, `wireplumber` - Drajver za zvuk i dodatni adapteri za njega
- `nano` - CLI editor teksta
- `man` - Linux manual

#### Update za mikrokod, odabrati jedno u zavisnosti od CPU:
- Intel: `intel-ucode`
- AMD: `amd-ucode`

#### Celokupna AMD instalacija
```sh
pacstrap -K /mnt base linux linux-firmware sudo networkmanager ufw pipewire pipewire-pulse pipewire-alsa pipewire-jack wireplumber nano man amd-ucode
```

#### Celokupna Intel instalacija
```sh
pacstrap -K /mnt base linux linux-firmware sudo networkmanager ufw pipewire pipewire-pulse pipewire-alsa pipewire-jack wireplumber nano man intel-ucode
```

## Preliminarna konfiguracija sistema
### `fstab`
```sh
genfstab -U /mnt >> /mnt/etc/fstab
nano /mnt/etc/fstab
```
Očitati generisan fajl u svrhu provere da li je sve u redu i izmeniti `fmask` i `dmask` za EFI particiju na `0077`.  
***EFI particija nije nužno prva na listi!***

### Promena konteksta u instaliran sistem
```sh
arch-chroot /mnt
```

### Ukljucivanje mrežnog servisa i firewalla
```
systemctl enable NetworkManager
systemctl stop iptables
systemctl disable iptables
systemctl enable ufw
```

### Podešavanje vremenske zone i lokala
```sh
ln -sf /usr/share/zoneinfo/Europe/Belgrade /etc/localtime
nano /etc/locale.gen

# Ukloniti `#` ispred:
#   en_US.UTF-8 UTF-8
#   en_GB.UTF-8 UTF-8
#   sr_RS UTF-8
#   sr_RS@latin UTF-8

locale-gen

nano /etc/locale.conf
```
Uneti sledeće:
```conf
LANG=en_US.UTF-8
LC_TIME=en_GB.UTF-8
```

### Ukljucivanje `resume` hook-a za hibernaciju sistema
```sh
nano /etc/mkinitcpio.conf
```
Naći `HOOKS` property i dodati `resume` hook tako da linija izgleda ovako:
```
HOOKS=(base udev autodetect microcode modconf kms keyboard keymap consolefont block filesystems resume fsck)
```
Regenerisati initramfs
```sh
mkinitcpio -P
```

### Definisanje hostname-a
```sh
echo tvoj_hostname > /etc/hostname
nano /etc/hosts
```
Uneti sledeće:
```Finalni restart
127.0.0.1 tvoj_hostname
```

### Dodavanje korisnika
```sh
# Promena root lozinke
passwd
# Dodavanje novog korisnika
useradd -m -G wheel,storage,optical,audio,video tvoj_username
passwd tvoj_username
EDITOR=nano visudo
```
Ukloniti `#` sa linije `%wheel ALL=(ALL) ALL`
## Podešavanje `systemd-boot` bootloader-a
```sh
bootctl install
nano /boot/loader/loader.conf
```
Izmeniti sadržaj fajla na sledeće:
```
default arch.conf
timeout 3
console-mode max
editor no
```
Izvršiti `cat /etc/fstab`, prikazaće se spisak mountovanih particija na ekranu sa UUID-evima. Obratiti pažnju na **UUID** EFI particije (**nije nužno prva na listi!**), biće potreban kasnije.
```sh
nano /boot/loader/entries/arch.conf
```
Uneti sledeći sadržaj ***u zavisnosti od procesora*** i `x`-eve zameniti prethodno prikazanim UUID-jem EFI particije:   
AMD:
```
title   Arch Linux
linux   /vmlinuz-linux
initrd  /amd-ucode.img
initrd  /initramfs-linux.img
options root=UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx rw
```
Intel:
```
title   Arch Linux
linux   /vmlinuz-linux
initrd  /intel-ucode.img
initrd  /initramfs-linux.img
options root=UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx rw
```
Sačuvati fajl i ukljuciti auto-update servis:
```sh
systemctl enable systemd-boot-update
```

## Unmount i prvi boot sistema
```sh
exit
umount -R /mnt
reboot
```
