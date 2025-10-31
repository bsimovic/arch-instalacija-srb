# 3. Multilib repozitorijum
`multilib` je dodatni oficijalni Arch repozitorijum koji sadrži 32-bit pakete.  
> ℹ️ **Multilib repozitorijum je neophodan za instalaciju grafičkih drivera.**  

## Uključivanje:

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
