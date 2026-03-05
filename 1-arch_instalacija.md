# 1. Arch Linux instalacija (UEFI)

## Internet za vreme instalacije
### Wi-Fi
> [!NOTE]
> Preskočiti ako je računar povezan na internet preko kabla

#### Listanje dostupnih mreža i povezivanje
```sh
iwctl
station wlan0 get-networks
station wlan0 connect [naziv mreže]
```

### Provera povezanosti na internet
```sh
ping google.com
```

## Diskovi, particije i fajl-sistemi
> [!WARNING]
> **Pogrešan korak sa `fdisk` alatom ili pri formatiranju particija može dovesti do *nepovratnog gubitka podataka*!**  
> **Nisam odgovoran za načinjenu štetu. Nastavite samo ako ste sigurni da znate šta radite!** 

### fdisk
#### Listanje diskova i odabir diska

```sh
fdisk -l
fdisk [dev file diska]
```

#### Komande
- `p` - listanje particija
- `g` - nova GPT tabela
- `n` - nova particija
- `t` - izmena tipa particije
- `w` - upis izmena na disk

#### Preporučeni layout particija
1. Veličina: **1GB** - tip: `uefi`
2. Veličina: **Veličina RAM-a** - tip: `swap`
3. Veličina: **Proizvoljna ili ceo ostatak diska** - tip: `linux` (root particija)
4. (Opciono) **Ostale particije po želji** - tip: `linux` (/home, /var, itd...)
    
### Formatiranje
> [!WARNING]
> **Formatiranje particije briše sve podatke na njoj!**  
> **Formatirati samo novokreirane particije!**

```sh
mkfs.fat -F 32 [dev file EFI particije]
mkswap [dev file swap particije]
mkfs.ext4 [dev file root particije]
# ILI
mkfs.btrfs -L [proizvoljan naziv particije] [dev file root particije]
```

> [!NOTE]
> Za btrfs i specifične funkcionalnosti btrfs-a (kompresija, snapshot), pogledati `E-btrfs.md`.
> U suprotnom, koristiti *ext4*.

Ostale `linux` particije formatirati sa `mkfs.ext4`.  

### Mount-ovanje
```sh
mount [dev file root particije] /mnt
mount --mkdir [dev file EFI particije] /mnt/boot
swapon [dev file swap particije]
```
Ostale linux filesystem particije (`/home`, `/var`, itd.), ako postoje, montirati na odgovarajuće lokacije unutar `/`.  
Postojeće storage particije (ako postoje) montirati u novim proizvoljnim folderima unutar `/mnt`, npr `/mnt/data` ili `/mnt/hdd1`.

## Instalacija sistema i alata
### Paketi
- `base`, `linux`, `linux-firmware` - Sam operativni sistem 
- `sudo` - Alat za izdavanje komandi kao drugi user
- `networkmanager` - Mrežni drajver
- `ufw` - Firewall
- `pipewire`, `pipewire-pulse`, `pipewire-alsa`, `pipewire-jack`, `wireplumber` - Drajver za zvuk i dodatni adapteri za njega
- `nano` - CLI editor teksta
- `man` - Manual pages

#### Update za mikrokod procesora, odabrati jedno u zavisnosti od proizvođača CPU:
- Intel: `intel-ucode`
- AMD: `amd-ucode`

#### Instalacija
```sh
pacstrap -K /mnt [spisak paketa odvojenih razmakom]
```

## Preliminarna konfiguracija sistema
### `fstab`
```sh
genfstab -U /mnt >> /mnt/etc/fstab
nano /mnt/etc/fstab
```

Očitati generisan fajl u svrhu provere da li je sve u redu i izmeniti `fmask` i `dmask` za EFI particiju na `0077`.

### chroot u instaliran sistem
```sh
arch-chroot /mnt
```

### Uključivanje mrežnog servisa i firewalla
```sh
systemctl enable NetworkManager
systemctl disable iptables
systemctl enable ufw
ufw enable
```

### Podešavanje vremenske zone i lokala
```sh
ln -sf /usr/share/zoneinfo/Europe/Belgrade /etc/localtime
nano /etc/locale.gen

#######################################################
# Ukloniti `#` ispred lokala po želji i sačuvati fajl #
#######################################################

locale-gen

# ukoliko nije generisan sr_RS ili en_US, staviti lokale po želji
echo "LANG=en_US.UTF-8" >> /etc/locale.conf
echo "LC_TIME=sr_RS.UTF-8" >> /etc/locale.conf
```

### Uključivanje `resume` hook-a za hibernaciju sistema, definisanje keymape
```sh
nano /etc/mkinitcpio.conf
```

Naći `HOOKS` property i dodati `resume` hook **između `block` i `filesystems`** i sačuvati fajl.  
Npr. ovako:
```
HOOKS=(base systemd autodetect microcode modconf kms keyboard keymap sd-vconsole block resume filesystems fsck)
```

```sh
nano /etc/vconsole.conf
```

Uneti i sačuvati fajl:

```
KEYMAP=us
```

Regenerisati initramfs:
```sh
mkinitcpio -P
```

### Uključivanje multilib repozitorijuma

```sh
sudo nano /etc/pacman.conf
```

Ukloniti `#` ispred linija i sačuvati fajl:
```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

### Definisanje hostname-a
```sh
echo [tvoj hostname] > /etc/hostname
echo "127.0.0.1 [tvoj hostname]" >> /etc/hosts
```

### Dodavanje korisnika
```sh
# Promena root lozinke
passwd
# Dodavanje novog korisnika
useradd -m -G wheel [tvoj username]
# Promena lozinke novog korisnika
passwd [tvoj username]
```

### `sudo` konfiguracija
```sh
EDITOR=nano visudo
```
Ukloniti `#` sa linije `%wheel ALL=(ALL) ALL` i sačuvati fajl.

## `systemd-boot` bootloader
```sh
bootctl install
nano /boot/loader/loader.conf
```
Izmeniti sadržaj fajla na sledeće:
```
default arch.conf
timeout 2
console-mode max
editor no
```

Sačuvati fajl.
```sh
# napraviti novi fajl koji sadrži UUID root particije
blkid -s UUID -o value [dev file root particije] > /boot/loader/entries/arch.conf
# otvoriti fajl i izmeniti ga
nano /boot/loader/entries/arch.conf
```
Uneti sledeći sadržaj ***u zavisnosti od procesora*** i `x`-eve zameniti UUID-jem root particije, sačuvati fajl:   
**AMD:**
```
title   Arch Linux
linux   /vmlinuz-linux
initrd  /["amd" ili "intel"]-ucode.img
initrd  /initramfs-linux.img
options root=UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx rw
```

```sh
systemctl enable systemd-boot-update
```
## Unmount, prvi boot sistema i prvi update
```sh
exit
umount -R /mnt
reboot
```

Izvući live USB i bootovati sistem sa particije na kojoj je instaliran.  
Ulogovati se sa prethodno kreiranim nalogom.
### Update repozitorijuma
```sh
sudo pacman -Syu
```
