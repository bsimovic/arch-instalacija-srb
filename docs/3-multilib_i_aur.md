# 3. Dodatni repozitorijumi
## Multilib
Multilib repozitorijum sadrži 32-bit pakete. **Potreban je prilikom instalacije grafičkih drivera.**

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

## Arch user repository (AUR)
Sadrži neoficijalne pakete dodate od strane korisnika.  
Pakete je potrebno kompajlovati pomoću `makepkg` i instalirati putem `pacman`-a - koristićemo alat koji olakšava ovaj posao - `yay`.  

```sh
cd ~
sudo pacman -S git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
cd ..
rm -r yay
```
`yay` se koristi slično kao `pacman`:
```sh
# Instalacija paketa
yay -S naziv_paketa

# Deinstalacija paketa se vrši putem pacman-a
pacman -R naziv_paketa
```
> ⚠️ **Biti oprezan pri instalaciji paketa sa AUR-a!**