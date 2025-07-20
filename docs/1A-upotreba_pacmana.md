# 1.A Upotreba `pacman`-a
Koristiti `sudo`
### Update repozitorijuma i svih paketa
```sh
pacman -Syu
```

### Informacije o paketima
```sh
pacman -Sii naziv_paketa1 naziv_paketa2 ...
```

### Instalacija ("sinhronizacija") paketa
```sh
pacman -S naziv_paketa1 naziv_paketa2 ...
```
`--needed` - Ne reinstaliraj već instalirane pakete

### Update paketa
```sh
pacman -U naziv_paketa1 naziv_paketa2 ...
```

### Brisanje paketa
```sh
pacman -R naziv_paketa1 naziv_paketa2 ...
```

### Listanje instaliranih paketa
```sh
pacman -Qq
```

### Pretraga paketa
```sh
pacman -Ss termin_za_pretragu
```
