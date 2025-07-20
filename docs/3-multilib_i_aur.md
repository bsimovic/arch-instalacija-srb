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

> ⚠️ **Biti oprezan pri instalaciji paketa sa AUR-a!**  
> ⚠️ **Dešavalo se da sadrži malware:**  
>  https://www.bleepingcomputer.com/news/security/arch-linux-pulls-aur-packages-that-installed-chaos-rat-malware/

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
# Instalacija paketa - BEZ SUDO!
yay -S naziv_paketa

# Deinstalacija paketa se vrši putem pacman-a
sudo pacman -R naziv_paketa
```
