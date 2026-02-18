# `pacman` package manager
`pacman` je podrazumevani package manager za Arch Linux, već je konfigurisan za pristup glavnim Arch repozitorijumima.
### Ažuriranje repozitorijuma i svih paketa
> [!NOTE]
> Raditi ažuriranje s vremena na vreme

```sh
sudo pacman -Syu
```

### Informacije o paketima
```sh
pacman -Sii [naziv paketa 1] [naziv paketa 2] [...]
```

### Instalacija ("sinhronizacija") paketa
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


