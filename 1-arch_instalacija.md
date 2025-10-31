# 1 Arch Linux instalacija
## Boot sa eksternog uređaja
Najpre, bootovati Arch Linux instalacioni uređaj (USB drive, CD, itd.)
## Internet za vreme instalacije
### Wi-Fi
> ℹ️ **Preskočiti ako je računar povezan na internet preko kabla**
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

Ukoliko veza sa internetom ne radi, moguće je da na mreži nema DHCP servera. U tom slučaju pogledati wiki za uputstvo kako podesiti statičku IP adresu: https://wiki.archlinux.org/title/Network_configuration#Static_IP_address.

## Diskovi, particije i fajl-sistemi
> ⚠️ **UPOZORENJE: Pogrešan korak sa `fdisk` alatom ili pri formatiranju particija može dovesti do *nepovratnog gubitka podataka*!**  
> ⚠️ **Nisam odgovoran za načinjenu štetu. Nastavite samo ako ste sigurni da znate šta radite!** 
### `fdisk`
#### Listanje diskova i odabir diska
```sh
fdisk -l
```
Obratiti pažnju na **dev file *diska*** na kome želite instalirati sistem.  
Primer ispisa iz `fdisk -l`:
```
Disk /dev/nvme1n1: 465.76 GiB, 500107862016 bytes, 976773168 sectors
```
- Dev file ovog ***diska*** je: `/dev/nvme1n1`
- Ostali dev fajlovi nabrojani ***ispod*** su dev fajlovi ***particija*** na tom disku
    
Odabrati **disk** na kojem ćete instalirati sistem:
```
fdisk dev_file_diska
```

#### Kreiranje particija
*Pogledati spisak komandi ispod.*  
Ukoliko je potrebno, napraviti novu tabelu particija (`g`). Ovo će obrisati ceo disk i napraviti novu tabelu.  
Nakon toga, za svaku particiju:
- `n` - Nova particija
- `t` - Tip particije
Na kraju, izvršiti `w`.

Potrebne su vam minimum 3 particije: uefi boot particija, swap particija i root particija. (*Pogledajte primer layout-a dole*)

#### Komande
Izmene se ne upisuju na disk pre nego što se izda `w` komanda.
- `p` - Listanje particija
- `g` - Nova GPT tabela (reset tabele particija)
    > ⚠️ **Kreiranje nove GPT tabele briše sve podatke na odabranom disku!**
- `n` - Nova particija
    - Uvek odabrati ponuđen početni sektor particije (samo `ENTER`)
    - Krajnji sektor particije (veličina particije) se može zadati sa `+1G`, `+500M`, itd.
- `t` - Definisanje tipa particije
    - `uefi` - EFI particija
    - `swap` - Swap particija
    - `linux` - Linux filesystem particija (default)
- `w` - Upisivanje izmena na disk i izlaz iz `fdisk`

#### Primer layout-a particija
- **1GB** - `uefi`
- **Veličina RAM-a** - `swap`
- **Proizvoljna veličina** - `linux` (ovo će biti root particija)
- **Ostale particije po želji** - `linux` (/home, /var, itd...)

Ostale `linux` particije mogu, po želji, biti i na različitim diskovima

### Formatiranje

> ⚠️ **Formatiranje particije briše sve podatke na njoj!**  
> ⚠️ **Formatirati samo novokreirane particije!**

```sh
mkfs.fat -F 32 dev_file_EFI_particije
mkswap dev_file_swap_particije
mkfs.ext4 dev_file_root_particije
```
Ostale `linux` particije formatirati sa `mkfs.ext4`.  

### Mount-ovanje
```sh
mount dev_file_root_particije /mnt
mount --mkdir dev_file_EFI_particije /mnt/boot
swapon dev_file_swap_particije
```
Ostale `linux` particije montirati na odgovarajuće lokacije unutar `/mnt`, npr:
```sh
mount dev_file_home_particije /mnt/home
```
Već postojeće storage particije montirati u novim proizvoljnim folderima unutar `/mnt`, npr `/mnt/data` ili `/mnt/hdd1`.

## Instalacija sistema i alata
#### Neophodni paketi
- `base`, `linux`, `linux-firmware` - Sam operativni sistem 
- `sudo` - Standardni alat za izdavanje komandi kao `root`
- `networkmanager` - Drajver za mrežu
- `ufw` - Firewall
- `pipewire`, `pipewire-pulse`, `pipewire-alsa`, `pipewire-jack`, `wireplumber` - Drajver za zvuk i dodatni adapteri za njega
- `nano` - CLI editor teksta
- `man` - Linux manual

#### Update za mikrokod procesora, odabrati jedno u zavisnosti od proizvođača CPU:
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
>⚠️ **EFI particija nije nužno prva na listi!**

> ℹ️ **Upotreba `nano`-a:**
> - `CTRL+o` - **save**
> - `CTRL+x` - **exit**
> - `CTRL+f` - **find**
> - `CTRL+g` - **help**

### chroot u instaliran sistem
```sh
arch-chroot /mnt
```

### Uključivanje mrežnog servisa i firewalla
```
systemctl enable NetworkManager
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
#   ...ostali lokali po želji

locale-gen
echo "LANG=en_US.UTF-8" >> /etc/locale.conf
echo "LC_TIME=en_GB.UTF-8" >> /etc/locale.conf
```

### Uključivanje `resume` hook-a za hibernaciju sistema
```sh
nano /etc/mkinitcpio.conf
```

Naći `HOOKS` property i dodati `resume` hook tako da linija izgleda ovako:
```
HOOKS=(base udev autodetect microcode modconf kms keyboard keymap consolefont block filesystems resume fsck)
```
Regenerisati initramfs:
```sh
mkinitcpio -P
```

### Definisanje hostname-a
```sh
echo tvoj_hostname > /etc/hostname
echo "127.0.0.1 tvoj_hostname" >> /etc/hosts
```

### Dodavanje korisnika
```sh
# Promena root lozinke
passwd
# Dodavanje novog korisnika
useradd -m -G wheel tvoj_username
passwd tvoj_username

EDITOR=nano visudo
```
Ukloniti `#` sa linije `%wheel ALL=(ALL) ALL` i sačuvati fajl.

## Podešavanje `systemd-boot` bootloader-a
> ℹ️ **Ja koristim `systemd-boot` jer već dolazi uz sistem i prilično je jednostavan za razumevanje (ne baš i za konfiguraciju).**  
> ℹ️ **Ukoliko planirate da koristite drugi bootloader (npr. GRUB), preskočite ovu sekciju i potražite uputstvo na internetu.**


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
Izvršiti `cat /etc/fstab`, prikazaće se spisak mountovanih particija na ekranu sa UUID-evima. Obratiti pažnju na **UUID** root (`/`) particije, biće potreban kasnije.
> ⚠️ **Root particija nije nužno prva na listi!**
```sh
nano /boot/loader/entries/arch.conf
```
Uneti sledeći sadržaj ***u zavisnosti od procesora*** i `x`-eve zameniti prethodno prikazanim UUID-jem root particije:   
**AMD:**
```
title   Arch Linux
linux   /vmlinuz-linux
initrd  /amd-ucode.img
initrd  /initramfs-linux.img
options root=UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx rw lsm=landlock,lockdown,yama,integrity,apparmor,bpf 
```
**Intel:**
```
title   Arch Linux
linux   /vmlinuz-linux
initrd  /intel-ucode.img
initrd  /initramfs-linux.img
options root=UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx rw lsm=landlock,lockdown,yama,integrity,apparmor,bpf
```
> ℹ️ `lsm` opcija je potrebna za `apparmor` servis, koji je potreban za `snapd` package manager

Sačuvati fajl i uključiti auto-update i apparmor servise:
```sh
systemctl enable systemd-boot-update
systemctl enable apparmor
```

## Unmount i prvi boot sistema
```sh
exit
umount -R /mnt
reboot
```

Bootovati sa diska na kome je instaliran sistem.
