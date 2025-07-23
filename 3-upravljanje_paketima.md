# 3. Repozitorijumi i upravljanje paketima
## pacman
`pacman` je podrazumevani package manager za Arch Linux, već je konfigurisan za pristup glavnim Arch repozitorijumima.

### Upotreba pacman-a
#### Update repozitorijuma i svih paketa
> ℹ️ Raditi ažuriranje s vremena na vreme   
> ℹ️ Uraditi sada ako već nije urađeno  

```sh
sudo pacman -Syu
```

#### Informacije o paketima
```sh
pacman -Sii naziv_paketa1 naziv_paketa2 ...
```

#### Instalacija ("sinhronizacija") paketa
```sh
sudo pacman -S naziv_paketa1 naziv_paketa2 ...
```
`--needed` - Ne reinstaliraj već instalirane pakete

#### Update paketa
```sh
sudo pacman -U naziv_paketa1 naziv_paketa2 ...
```

#### Brisanje paketa
```sh
sudo pacman -R naziv_paketa1 naziv_paketa2 ...
```

#### Listanje instaliranih paketa
```sh
sudo pacman -Qq
```

#### Pretraga paketa
```sh
pacman -Ss termin_za_pretragu
```
Ili na sajtu: https://archlinux.org/packages/

## Multilib repozitorijum
`pacman` je po default-u konfigurisan samo za pristup `core` i `extra` repozitorijumima.  
`multilib` je dodatni oficijalni Arch repozitorijum koji sadrži 32-bit pakete.  
> ℹ️ **Multilib repozitorijum je neophodan za instalaciju grafičkih drivera.**  

### Uključivanje:

```sh
sudo nano /etc/pacman.conf
```

Ukloniti komentar sa linija:
```
[multilib]
Include = /etc/pacman.d/mirrorlist
```

Sačuvati fajl i uraditi upgrade:
```sh
sudo pacman -Syu
```

Instalacija paketa se vrši standardno putem `pacman`-a.

## Arch user repository (AUR)
Sadrži neoficijalne pakete dodate od strane korisnika.  

> ⚠️ **Oprezno pri instalaciji paketa sa AUR-a!**  
> ⚠️ **Dešavalo se da sadrže malware:**  
>  https://www.bleepingcomputer.com/news/security/arch-linux-pulls-aur-packages-that-installed-chaos-rat-malware/

> ℹ️ **Korišćenje "pomagača za AUR", npr. `yay`, se ne preporučuje od strane Arch Linux developera. Tako da ću ovde objasniti ručni proces instalacije paketa sa AUR-a.**

### Konfiguracija
#### Alat
- `base-devel` - kompajleri, linkeri, itd. - između ostalog i `makepkg` alat koji je potreban za AUR
- `git` - git source control CLI
```sh
sudo pacman -S --needed base-devel git
```

#### Odabir foldera za čuvanje specifikacije paketa
Napraviti proizvoljan folder unutar `~` (home) foldera, ja ću ga nazvati `.aur`.  
U tom folderu ćemo čuvati `PKGBUILD` specifikacije **svih** AUR paketa.
```sh
mkdir ~/.aur
```

### Pretraga paketa
Paketi se mogu pretražiti na zvaničnom sajtu: https://aur.archlinux.org/packages  
Sa stranice paketa, kopirati ***Git Clone URL***, biće potreban pri instalaciji.

### Preuzimanje i instalacija paketa:
```sh
# Preći u prethodno kreiran .aur folder
cd ~/.aur
# Preuzeti specifikaciju paketa
git clone git_clone_url
# Buildovanje i instalacija
makepkg -si -D naziv_paketa
```

> ⚠️ **Ne brisati specifikaciju paketa, potrebna je za ažuriranje!**  

### Ažuriranje paketa
```sh
# Preći u folder sa specifikacijom paketa
cd ~/.aur/naziv_paketa
# Ažurirati specifikaciju
git pull
# Buildovanje i instalacija
makepkg -si
```

### Deinstalacija paketa
```sh
sudo pacman -R naziv_paketa
```