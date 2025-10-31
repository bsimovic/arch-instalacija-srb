# `pacman` package manager
`pacman` je podrazumevani package manager za Arch Linux, već je konfigurisan za pristup glavnim Arch repozitorijumima.
### Ažuriranje repozitorijuma i svih paketa
> ℹ️ Raditi ažuriranje s vremena na vreme   

```sh
sudo pacman -Syu
```

### Informacije o paketima
```sh
pacman -Sii naziv_paketa1 naziv_paketa2 ...
```

### Instalacija ("sinhronizacija") paketa
```sh
sudo pacman -S naziv_paketa1 naziv_paketa2 ...
```
`--needed` - Ne reinstaliraj već instalirane pakete

### Update paketa
```sh
sudo pacman -U naziv_paketa1 naziv_paketa2 ...
```

### Brisanje paketa
```sh
sudo pacman -R naziv_paketa1 naziv_paketa2 ...
```

### Listanje instaliranih paketa
```sh
sudo pacman -Qq
```

### Pretraga paketa
```sh
pacman -Ss termin_za_pretragu
```
Ili na sajtu: https://archlinux.org/packages/


