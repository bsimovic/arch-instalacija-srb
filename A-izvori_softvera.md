# 5.A Izvori softvera
## pacman package manager
Podrazumevani alat za instalaciju softvera, automatski povezan sa zvaničnim Arch repozitorijumima

### Ažuriranje repozitorijuma, svih paketa i operativnog sistema
> [!NOTE]
> Raditi ažuriranje s vremena na vreme

```sh
sudo pacman -Syu
```

### Informacije o paketima
```sh
pacman -Sii [naziv paketa 1] [naziv paketa 2] [...]
```

### Instalacija paketa
```sh
sudo pacman -S [naziv paketa 1] [naziv paketa 2] [...]
```
`--needed` - Ne reinstaliraj već instalirane pakete

### Brisanje paketa
```sh
sudo pacman -R [naziv paketa 1] [naziv paketa 2] [...]
```

### Listanje instaliranih paketa
```sh
sudo pacman -Qq
```

### Pretraga paketa
```sh
pacman -Ss [termin za pretragu]
```
Ili na sajtu: https://archlinux.org/packages/


## Arch user repository (AUR)
Sadrži neoficijalne pakete dodate od strane korisnika.  

> [!NOTE]
> Korišćenje "pomagača za AUR", npr. `yay`, se ne preporučuje od strane Arch Linux developera.  
> Ovde ću objasniti ručni proces instalacije paketa sa AUR-a.

### Konfiguracija
#### Alat
- `base-devel` - kompajleri, linkeri, itd. - između ostalog i `makepkg` alat koji je potreban za AUR
- `git` - git source control CLI

#### Odabir foldera za čuvanje specifikacije paketa
Napraviti proizvoljan folder unutar `~` (home) foldera, ja ću ga nazvati `.aur`.  
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
git clone [git clone URL]
# Buildovanje i instalacija
makepkg -si -D [naziv paketa]
```
> [!WARNING]
> Ne brisati specifikaciju paketa, potrebna je za ažuriranje!

### Ažuriranje paketa
```sh
# Preći u folder sa specifikacijom paketa
cd ~/.aur/[naziv paketa]
# Ažurirati specifikaciju
git pull

# Ukoliko pull povuče neku izmenu, uradi ponovo makepkg:
makepkg -si
```

### Deinstalacija paketa
```sh
sudo pacman -R [naziv paketa]
```

## Flathub
Package manager koji pruža pristup flathub-u se zove `flatpak`.  
Softver možete pretraživati na sajtu: https://flathub.org

> [!NOTE]
> Oficijalne distribucije softvera imaju ✅ (Verified) oznaku na flathub sajtu.

### Instalacija flatpak-a:
```sh
sudo pacman -S flatpak
```

### Instalacija paketa:
```sh
flatpak install [naziv paketa]
```
Paketi se identifikuju punim kvalifikovanim nazivom, npr: `com.discordapp.Discord`.  
Naziv paketa može biti i parcijalan (npr samo `discord`), u tom slučaju ćete dobiti rezultat pretrage i moći ćete da odaberete odgovarajući paket.

### Ažuriranje svih paketa:
```sh
flatpak update
```

### Listanje instaliranih paketa:
```sh
flatpak list --app
```

#### Brisanje paketa:
```sh
flatpak uninstall [naziv paketa]
```

## AppImage
Ekvivalentno *exe* fajlu ili instalacionom fajlu na Windows-u.  
Preuzimati isključivo sa verodostojnih sajtova!  

### Upotreba
Preuzete AppImage-e čuvati u predefinisanom folderu unutar home direktorijuma (npr. `~/Applications`).  
Svaki preuzeti AppImage fajl morate obeležiti kao executable:
```sh
chmod +x [putanja do AppImage fajla]
```

### Desktop fajl
Da bi se AppImage program pojavio u Launcheru (start meniju), neophodno je kreirati "desktop fajl", primer desktop fajla:
```desktop
[Desktop Entry]
Name=Naziv programa
Comment=Opis programa
Exec=Putanja do AppImage fajla
Icon=Putanja do ikone (pogledati dole)
Terminal=true/false (da li je konzolna aplikacija ili ne?)
Type=Application
Categories=Spisak kategorija odvojenih tačka-zarezom (;) (pogledati dole)

```
Desktop fajlove čuvati u `~/.local/share/applications/` sa `.desktop` ekstenzijom.

#### Ikona
Ikonu možete povući sa interneta. Neki AppImage fajlovi sadrže ikone u sebi, u tomu slučaju treba privremeno otpakovati AppImage:
```sh
[putanja do AppImage fajla] --appimage-extract
```
Kreiraće se folder `squashfs-root` u kome treba potražiti ikonu. Nakon kopiranja ikone obrisati folder.

#### Kategorije
Spisak mogućih kategorija:
- AudioVideo
- Development
- Education
- Game
- Graphics
- Network
- Office
- Science
- Settings
- System
- Utility




