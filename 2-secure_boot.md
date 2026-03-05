# 2. Secure boot konfiguracija (systemd-boot)

> [!NOTE]
> Preskočiti ceo ovaj dokument ako secure boot nije potreban.

## Bootovanje u setup mode
Restartovati računar i u UEFI konfiguraciji ("u BIOS-u") uključiti ***secure boot setup mode***.  
Neki proizvođači matičnih ploča ne nude eksplicitno tu opciju, u tom slučaju setup mode se uključuje tako što se obrišu svi već postojeći ključevi.  

## Generisanje i enrollovanje ključeva
```sh
# root mode
sudo su
# instalacija sbctl
pacman -Sy sbctl
# Prikaz statusa, treba pisati "Setup Mode: Enabled"
sbctl status
# Kreiranje kljuceva
sbctl create-keys
# Uz kreirane, uvek treba da se enrolluju i Microsoft ključevi (-m), za svaki slučaj
sbctl enroll-keys -m
```

## Potpisivanje kernel modula
```sh
# Listanje fajlova za potpisivanje
sbctl verify
```
Potpisati sve fajlove sa izlistanog spiska:
```sh
sbctl sign -s [putanja do fajla]
```

## Automatsko potpisivanje `systemd-boot` bootloader-a
```sh
sbctl sign -s -o /usr/lib/systemd/boot/efi/systemd-bootx64.efi.signed /usr/lib/systemd/boot/efi/systemd-bootx64.efi
```

## Restart
Možete restartovati računar, **uključiti *secure boot*** i **isključiti *setup mode*** u UEFI konfiguraciji.
