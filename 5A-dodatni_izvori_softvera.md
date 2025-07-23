# 5.A Dodatni izvori softvera
***Opcioni dodatni izvori za preuzimanje softvera.***

## Flathub
Package manager koji pruža pristup flathub-u se zove `flatpak`.  
Flathub ima široku upotrebu i mnogi poznat softver se može naći na njemu.  
Softver možete pretraživati na sajtu: https://flathub.org

> ℹ️ **Oficijalne distribucije softvera imaju ✅ (Verified) oznaku na flathub sajtu.**

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

## Direktno preuzimanje softvera ("Windows način") *(Opciono)*
> ⚠️ Direktno preuzimanje softvera sa oficijalnog sajta je alternativa za AUR, preporučljivo je samo ***ako nema traženog softvera u Arch repozitorijumima ili na flathub-u***.  
> ⚠️ Kao i kod Windows-a, preuzimajte softver samo ako ste sigurni da je izvor siguran!

Postoji nekoliko mogućnosti direktne distribucije:
- Pre-kompajlovana izvršna datoteka (često upakovana sa dodatnim fajlovima u `.tar.gz` arhivi)
- `.appimage` paket
- `.deb` ili `.rpm` paket (**Tražiti alternativu**)

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