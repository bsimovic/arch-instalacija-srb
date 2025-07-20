# 3. Repozitorijumi i upravljanje paketima
## pacman
`pacman` je podrazumevani package manager za Arch Linux, već je konfigurisan za pristup glavnim Arch repozitorijumima.

### Upotreba pacman-a
#### Update repozitorijuma i svih paketa
> ℹ️ Uraditi ovo sada ako već nije urađeno  

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
Multilib je dodatni oficijalni Arch repozitorijum koji sadrži 32-bit pakete.  
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

### Potreban alat
- `base-devel` - kompajleri, linkeri, itd.
- `git` - CLI za git source control sistem
```sh
sudo pacman -S --needed base-devel git
```

### Pretraga paketa
Paketi se mogu pretražiti na zvaničnom sajtu: https://aur.archlinux.org/packages  
Sa stranice paketa, kopirati ***Git Clone URL***, biće potreban pri instalaciji.

### Preuzimanje i instalacija paketa
```sh
# Preuzimanje
cd ~
git clone git_clone_url
# Napraviće se novi folder sa nazivom paketa
cd naziv_paketa
# Buildovanje i instalacija
makepkg -si
# Nakon instalacije možete obrisati folder
cd ..
rm -rf naziv_paketa
```

### Deinstalacija paketa
```sh
sudo pacman -R naziv_paketa
```