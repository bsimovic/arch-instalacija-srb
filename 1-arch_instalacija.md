# 1. Arch Linux instalacija

> [!NOTE]
> Ovaj dokument podrazumeva da instalirate sistem na računaru koji podržava UEFI boot, ukoliko imate veoma star računar koji podržava samo legacy boot, pogledati dokument `D-arch_instalacija_legacy.md`.

Za kreiranje instalacionog USB-a, potreban vam je prazan USB drive i računar koji već ima operativni sistem.  
Najpre ubaciti USB drive u računar.

## Preuzimanje Arch Linuxa
Preuzeti najnoviji Arch Linux ISO fajl sa: https://archlinux.org/download/

## Kreiranje instalacionog uređaja i live boot
### Na Windowsu:
Preuzeti i instalirati **Balena Etcher** sa: https://etcher.balena.io  
Pokrenuti Balena Etcher i pratiti uputstva na ekranu za upisivanje ISO fajla na USB.

### Na Linuksu:
```sh
sudo fdisk -l
```
Naći **dev fajl** USB uređaja (npr. `/dev/sdb`).

> [!WARNING]
> **Odabir pogrešnog dev fajla može dovesti do katastrofalnog gubitka podataka!**
> **Budite sigurni da ste našli tačan dev fajl pre izvršavanja naredne komande!**

Upis ISO fajla na USB:
```sh
sudo su
cat [putanja do ISO fajla] > [dev file USB uređaja]
```
Sačekati da se izvrši upis.

### Boot sa USB-a
Ubaciti USB u računar na kojem želite instalirati Arch Linux.  
Startovati (ili restartovati) taj računar. Pri pokretanju računara otvoriti meni za odabir boot uređaja.  

> [!TIP]
> Taster za otvaranje menija zavisi od specifične matične ploče.
> Uglavnom je neki od ovih: **F2**, **F8**, **F10**., **F11** ili **F12**.

Iz menija odabrati USB uređaj.

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
>  **Pogrešan korak sa `fdisk` alatom ili pri formatiranju particija može dovesti do *nepovratnog gubitka podataka*!**  
>  **Pročitajte *celo ovo poglavlje* pre nego što krenete sa radom.**  
>  **Nisam odgovoran za načinjenu štetu. Nastavite samo ako ste sigurni da znate šta radite!** 

### fdisk
#### Listanje diskova i odabir diska

```sh
fdisk -l
```
Obratiti pažnju na **dev file *diska*** na kome želite instalirati sistem. Npr. `/dev/nvme0n1` ili `/dev/sda`.  
Odabrati disk na kojem ćete instalirati sistem:
```sh
fdisk [dev file diska]
```

#### Listanje particija i kreiranje nove tabele
Nakon otvaranja diska, možete izlistati trenutne particije komandom `p`.  
Ukoliko je potrebno, napraviti novu tabelu particija (komanda: `g`).

#### Preporučeni layout particija
1. Veličina: **1GB** - tip: `uefi`
2. Veličina: **Veličina RAM-a** - tip: `swap`
3. Veličina: **Proizvoljna ili ceo ostatak diska** - tip: `linux` (root particija)
4. (Opciono) **Ostale particije po želji** - tip: `linux` (/home, /var, itd...)

Ostale particije mogu, po želji, biti i na različitim diskovima

#### Procedura kreiranja particije
- `n` - kreiranje nove particije
    - za indeks particije odabrati podrazumevani (samo `ENTER`)
    - za početni sektor uvek odabrati ponuđeni (samo `ENTER`)
    - **krajnji sektor particije određuje veličinu particije i može se zadati sa `+1G`, `+500M`, itd.** Ako ne unesete vrednost za poslednji sektor (samo `ENTER`), kreiraće se particija veličine preostalog praznog prostora na disku.
- `t` - definisanje tipa particije
    - biće ponuđen indeks poslednje kreirane particije (samo `ENTER`)
    - uneti naziv tipa u zavisnosti od uloge particije:
        - `uefi` - EFI particija
        - `swap` - Swap particija
        - `linux` - Linux filesystem particija (default)
        
Ponoviti celu proceduru za svaku particiju.

#### Finalizacija
Izdavanjem komande `w` se izmene **nepovratno upisuju na disk**.  
Izvršiti `w` nakon što ste zadovoljni stanjem diska.
    
### Formatiranje
> [!WARNING]
> **Formatiranje particije briše sve podatke na njoj!**  
> **Formatirati samo novokreirane particije!**

```sh
mkfs.fat -F 32 [dev file EFI particije]
mkswap [dev file swap particije]
mkfs.ext4 [dev file root particije]
```
Ostale `linux` particije formatirati sa `mkfs.ext4`.  

### Mount-ovanje
```sh
mount [dev file root particije] /mnt
mount --mkdir [dev file EFI particije] /mnt/boot
swapon [dev file swap particije]
```
Ostale `linux` particije (ako postoje) montirati na odgovarajuće lokacije unutar `/mnt`, npr:
```sh
mount [dev file home particije] /mnt/home
```
Postojeće storage particije (ako postoje) montirati u novim proizvoljnim folderima unutar `/mnt`, npr `/mnt/data` ili `/mnt/hdd1`.

## Instalacija sistema i alata
### Paketi
- `base`, `linux`, `linux-firmware` - Sam operativni sistem 
- `sudo` - Standardni alat za izdavanje komandi kao `root` korisnik
- `networkmanager` - Mrežni drajver
- `ufw` - Firewall
- `pipewire`, `pipewire-pulse`, `pipewire-alsa`, `pipewire-jack`, `wireplumber` - Drajver za zvuk i dodatni adapteri za njega
- `nano` - CLI editor teksta
- `man` - (*opciono*) Linux manual

#### Update za mikrokod procesora, odabrati jedno u zavisnosti od proizvođača CPU:
- Intel: `intel-ucode`
- AMD: `amd-ucode`

#### Instalacija
Inicijalna instalacija se vrši `pacstrap` alatom. Navešću kompletne komande za intel i za AMD, odaberite jednu:  

**AMD:**
```sh
pacstrap -K /mnt base linux linux-firmware sudo networkmanager ufw pipewire pipewire-pulse pipewire-alsa pipewire-jack wireplumber nano man amd-ucode
```

**Intel**:
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
> [!WARNING]
> **EFI particija nije nužno prva na listi!**

> [!TIP]
> Upotreba `nano`-a:
> - `CTRL+o` - save
> - `CTRL+x` - exit
> - `CTRL+f` - find

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

# Ukloniti `#` ispred:
#   en_US.UTF-8 UTF-8
#   sr_RS UTF-8
#   sr_RS@latin UTF-8
#   ...ostali lokali po želji
#
#   Sačuvati fajl!!!
```

Nakon toga:
```
locale-gen
echo "LANG=en_US.UTF-8" >> /etc/locale.conf
echo "LC_TIME=sr_RS.UTF-8" >> /etc/locale.conf
```

### Uključivanje `resume` hook-a za hibernaciju sistema
```sh
nano /etc/mkinitcpio.conf
```

Naći `HOOKS` property i dodati `resume` hook **između `block` i `filesystems`!**  
Npr. ovako:
```
HOOKS=(base systemd autodetect microcode modconf kms keyboard keymap sd-vconsole block resume filesystems fsck)
```

Sačuvati fajl i regenerisati initramfs:
```sh
mkinitcpio -P
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
passwd [tvoj username]
```

### `sudo` konfiguracija
```sh
EDITOR=nano visudo
```
Ukloniti `#` sa linije `%wheel ALL=(ALL) ALL` i sačuvati fajl.

## Podešavanje `systemd-boot` bootloader-a
> [!TIP]
> Ja koristim `systemd-boot` jer već dolazi uz sistem i prilično je jednostavan za razumevanje (ne baš i za konfiguraciju).  
> Ukoliko planirate da koristite drugi bootloader (npr. GRUB), preskočite ovu sekciju i potražite uputstvo na internetu.


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
Izvršiti `cat /etc/fstab`, prikazaće se spisak mountovanih particija na ekranu sa UUID-evima. Obratiti pažnju na **UUID** root (`/`) particije, biće potreban kasnije.
> [!WARNING]
> **Roor particija nije nužno prva na listi!**

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
options root=UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx rw
```
**Intel:**
```
title   Arch Linux
linux   /vmlinuz-linux
initrd  /intel-ucode.img
initrd  /initramfs-linux.img
options root=UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx rw
```

Sačuvati fajl i uključiti auto-update servis:
```sh
systemctl enable systemd-boot-update
```

## Unmount, prvi boot sistema i prvi update
```sh
exit
umount -R /mnt # Pažnja! Nije "unmount" nego "umount"!
reboot
```

Možete izvući USB uređaj iz računara i bootovati sa diska na kojem je instaliran sistem.

### Uključivanje multilib repozitorijuma i update
Multilib repozitorijum sadrži 32-bit pakete i neophodan je za određene drajvere.

```sh
sudo nano /etc/pacman.conf
```

Ukloniti `#` ispred linija:
```
[multilib]
Include = /etc/pacman.d/mirrorlist
```
Sačuvati fajl.

### Prvi update repozitorijuma i sistema
```sh
sudo pacman -Syu
```

## Sledeći koraci
Sistem je instaliran i spreman za upotrebu.  
Kao sledeći korak možete podesiti *secure boot* ukoliko je neophodno (`2-secure_boot.md`) ili odmah preći na instaliranje grafičkog drajvera (`3-graficki_drajver.md`) i grafičkog okruženja (`4-plasma_instalacija`).
