# 1.A Upotreba `pacman`-a
Koristiti `sudo`
### Update repozitorijuma i svih paketa
```sh
pacman -Syu
```

### Instalacija ("sinhronizacija") paketa
```sh
pacman -S naziv_paketa1 naziv_paketa2 ...
```
`--needed` - Ne reinstaliraj već instalirane pakete

### 

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
pacman -Ss search_string
```