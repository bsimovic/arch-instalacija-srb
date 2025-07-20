# 5.A Dodatni repozitorijumi
***Korisni dodatni izvori za preuzimanje softvera.***

## Flathub
Package manager koji pruža pristup flathub-u se zove `flatpak`.  
Flathub ima široku upotrebu i mnogi poznat softver se može naći na njemu.  
Softver možete pretraživati na zvaničnom sajtu: https://flathub.org

> ℹ️ **Oficijalne distribucije softvera imaju ✅ (Verified) oznaku na flathub sajtu.**

> ⚠️ **Softver pribavljen sa flathub-a je automatski *sandbox-ovan* tj. izolovan od ostatka sistema, tako da je generalno sigurniji od AUR-a. Uprkos tome, svakako treba biti oprezan!**    
> ⚠️ **Nikako ne menjati default repozitorijum!**  

  

### Instalacija flatpak-a:
```sh
sudo pacman -S flatpak
```

### Upotreba:
#### Instalacija paketa:
```sh
flatpak install naziv_paketa
```
Paketi se identifikuju punim kvalifikovanim nazivom, npr: `com.discordapp.Discord`.  
`naziv_paketa` može biti i parcijalan (npr samo `discord`), u tom slučaju ćete dobiti rezultat pretrage i moći ćete da odaberete odgovarajući paket.

#### Ažuriranje svih paketa:
```sh
flatpak update
```

#### Listanje instaliranih paketa:
```sh
flatpak list --app
```

#### Brisanje paketa:
```sh
flatpak uninstall naziv_paketa
```

## Snapcraft
Package manager koji pruža pristup snapcraft-u se zove `snap`.  
Snap je na puno načina sličan "flatpak"-u. Mnogi softver se nalazi na oba, doduše, **često se zvanična distribucija nekog softvera nalazi na samo jednoj od ove dve platforme!**  
Softver možete pretraživati na zvaničnom sajtu: https://snapcraft.io

> ℹ️ **Oficijalne distribucije softvera imaju ✅ (Verified) oznaku na snapcraft sajtu.**

> ⚠️ **Slično flatpak-u, softver pribavljen preko snap-a radi u *sandbox-u*. Uprkos tome, svakako treba biti oprezan!**  
> ⚠️ **Nikako ne menjati default repozitorijum!**  

  

### Instalacija snap-a:
Snap se nalazi na AUR-u. Za postupak instalacije, pogledati odeljak **"Arch user repository (AUR)"** u ovom dokumentu.

- **Naziv paketa**: `snapd`
- **Git clone url:**: https://aur.archlinux.org/snapd.git

Snap za sandboxing koristi `apparmor`, testirati da li je podešen:
```sh
aa-enabled
# Treba ispisati: Yes
```

> ⚠️ **Ukoliko `apparmor` ne radi, niste podesili odgovarajući kernel parametar ili niste pokrenuli `apparmor` servis.**  
> ⚠️ **Kernel parametar**: `lsm=landlock,lockdown,yama,integrity,apparmor,bpf`  
> ⚠️ **Startovanje servisa**: `systemctl enable --now apparmor`

> ℹ️ **Ako koristite `systemd-boot`, pogledajte dokument `1-arch_instalacija.md`, odeljak *"Podešavanje systemd-boot bootloader-a"***  
> ℹ️ **Ako koristite drugi bootloader, pogledajte Arch wiki za uputstvo definicije kernel parametara: https://wiki.archlinux.org/title/Kernel_parameters#Boot_loader_configuration**

Nakon instalacije neophodno je uključiti `snapd.socket` i `snapd.apparmor.service` servise:
```sh
sudo systemctl enable --now snapd.socket
sudo systemctl enable --now snapd.apparmor.service
```


### Upotreba:
#### Instalacija paketa:
```sh
sudo snap install naziv_paketa
```

#### Ažuriranje paketa:
Snap paketi se automatski ažuriraju putem `snapd` servisa.  
Moguće je i ručno ažurirati paket:  
```sh
sudo snap refresh naziv_paketa
```

#### Listanje instaliranih paketa:
```sh
snap list
```

#### Brisanje paketa:
```sh
sudo snap remove naziv_paketa
```

## Direktno preuzimanje softvera ("Windows način") *(Opciono)*
> ⚠️ Direktno preuzimanje softvera sa oficijalnog sajta je alternativa za AUR, preporučljivo je samo ***ako nema traženog softvera u glavnim Arch repozitorijumima ili kao verifikovan paket na flathub-u ili snap-u***.  
> ⚠️ Mana u poređenju sa AUR-om je nemogućnost automatskog ažuriranja.  
> ⚠️ Kao i kod Windows-a, preuzimajte softver samo ako ste sigurni da je izvor siguran!

Postoji nekoliko mogućnosti direktne distribucije:
- Pre-kompajlovana izvršna datoteka (često upakovana sa dodatnim fajlovima u `.tar.gz` arhivi)
- `.appimage` paket
- `.deb` paket

### Pre-kompajlovane izvršne datoteke

Ekstraktovati arhivu u proizvoljni folder u `~` (home) direktorijumu :
```sh
tar -xvzf naziv_fajla.tar.gz -C ~/naziv_foldera
cd ~/naziv_foldera
```

Naći izvršni fajl, dodati mu execute permisiju i napraviti simboličku vezu u `/usr/local/bin`:
```sh
sudo chmod +x izvrsni_fajl
sudo ln -sf izvrsni_fajl /usr/local/bin/izvrsni_fajl
```

### `.appimage` paketi

Ekvivalentno portable `.exe` fajlu na windows-u.

Dodati execute permisiju i napraviti simboličku vezu u `/usr/local/bin`:
```sh
# U folderu sa paketom
sudo chmod +x naziv_fajla.appimage
sudo ln -sf naziv_fajla.appimage /usr/local/bin/naziv_fajla
```


### `.deb` paketi

> ⚠️ **Instalirajte `.deb` paket samo ako nemate drugu alternativu**

Dodati execute permisiju i napraviti simboličku vezu u `/usr/local/bin`:

Alat:
```sh
sudo pacman -S dpkg
```

Instalacija:
```sh
# U folderu sa paketom
dpkg -i naziv_paketa.deb
```
