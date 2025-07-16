# 1.2 Secure boot konfiguracija
**Preskočiti ceo ovaj dokument ako secure boot nije potreban.**
## Bootovanje u setup mode
Restartovati računar i u UEFI konfiguraciji ("u BIOS-u") uključiti ***secure boot setup mode***.  
Neki proizvođači matičnih ploča ne nude eksplicitno tu opciju, u tom slučaju setup mode se uključuje tako što se obrišu svi već postojeći ključevi.  
## Generisanje i enrollovanje ključeva
```sh
sudo pacman -S sbctl
sudo sbctl status
# Prikazaće da li je setup mode uključen ili nije
sudo sbctl create-keys
sudo sbctl enroll-keys -m
# Uz kreirane, uvek treba da se enrolluju i Microsoft ključevi (-m), za svaki slučaj
```

## Potpisivanje kernel modula
```sh
sudo sbctl verify
```
Izlistaće se fajlovi koje treba potpisati, ignorisati linije sa `invalid_pe_header` greškom.  
Uglavnom su na spisku sledeći fajlovi:
```
/boot/EFI/BOOT/BOOTX64.EFI
/boot/EFI/systemd/systemd-bootx64.efi
/boot/vmlinuz-linux
```
Svaki od njih potpisati sa:
```sh
sudo sbctl sign -s putanja_fajla
```

## Automatsko potpisivanje `systemd-boot` bootloader-a
Ukoliko se koristi `systemd-boot` bootloader, neophodno je potpisati i njega sledećom komandom:
```sh
sudo sbctl sign -s -o /usr/lib/systemd/boot/efi/systemd-bootx64.efi.signed /usr/lib/systemd/boot/efi/systemd-bootx64.efi
```

## Restart
Možete restartovati računar i uključiti ***secure boot*** u UEFI konfiguraciji.