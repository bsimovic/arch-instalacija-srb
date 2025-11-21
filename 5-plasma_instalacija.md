# 5.  Instalacija i konfiguracija Plasma desktop okruženja
> ℹ️ **Ukoliko želite neko drugo desktop okruženje (ili ne želite nijedno), možete preskočiti ovaj dokument.**

## Paketi
### Osnovni paketi

- `plasma-desktop` - Plasma dekstop okruženje
- `plasma-nm` - Mrežni konfigurator
- `plasma-pa` - Audio konfigurator
- `plasma-firewall` - Firewall konfigurator
- `plasma-systemmonitor` - Task manager
- `kscreen` - Konfigurator displeja
- `kinfocenter` - Prikaz informacija o sistemu
- `ksystemlog` - Event log
- `kwalletmanager` - Konfigurator "KWallet-a" (pogledati odeljak "KWallet" dole)
- `kwallet-pam` - Automatsko otvaranje wallet-a
- `sddm` - Menadžer sesija
- `sddm-kcm` - Konfigurator sddm-a
- `xdg-desktop-portal-kde` - Potrebno za razne desktop funkcionalnosti (file picker, drag-and-drop, itd.)
- `bluedevil` - Bluetooth konfigurator
- `powerdevil` - Power profile konfigurator
- `power-profiles-daemon` - Servis za upravljanje power profilima
- `kde-gtk-config` - Sinhronizuje sistemsku temu sa GTK aplikacijama
- `breeze-gtk` - KDE Breeze tema za GTK
- `cups` - Servis za štampanje
- `print-manager` - Menadžer štampača
- `konsole` - Terminal emulator
- `dolphin` - File explorer
- `ark` - Menadžer arhiva
- `kclock` - Tajmer, štoperica, itd.
- `gwenview` - Pregledač slika
- `kamoso` - Kamera
- `spectacle` - Screenshot alat (Po uzoru na "Snipping Tool" iz Windowsa)
- `kate` - GUI tekst editor
- `kcalc` - Kalkulator
- `noto-fonts-cjk` - Noto fontovi
- `ttf-liberation` - Liberation fontovi
- `ttf-droid` - Droid fontovi
- `kdeplasma-addons` - Dodatne sitnice

```sh
sudo pacman -S plasma-desktop plasma-nm plasma-pa plasma-firewall plasma-systemmonitor kscreen kinfocenter ksystemlog kwalletmanager kwallet-pam sddm sddm-kcm xdg-desktop-portal-kde bluedevil powerdevil power-profiles-daemon kde-gtk-config breeze-gtk cups print-manager konsole dolphin ark kclock gwenview kamoso spectacle kate kcalc noto-fonts-cjk ttf-liberation ttf-droid kdeplasma-addons
```

## Web pregledač
Odabrati i instalirati željeni web pregledač, navešću najpopularnije:
- `firefox`
- `chromium`
- `google-chrome` (AUR)
- `vivaldi`
- `brave` (AUR)

> ℹ️ **Za uputstvo za instalaciju AUR paketa pogledati dodatak `B-dodatni_izvori_softvera.md`, odeljak *"Arch user repository (AUR)"***

## Media player
Postoji gomila - navešću dva:

- `vlc` - popularan GUI player
    - `vlc-plugins-all` - neophodan paket sa kodecima za vlc
- `mpv` - CLI player, veoma lightweight
    - `mpv-mpris` - za integraciju sa operativnim sistemom (playing status u task manageru)
## Office paket
Dve opcije su:

- `libreoffice-fresh` - veoma dobar office paket - može sve što i najnovije verzije Microsoft Office-a
    - Ume biti malo nezgrapan za upotrebu, pogotovo ako ste navikli na moderan MS Office (UI LibreOffice-a podseća na MS Office 2003)
- Korisćenje nekih od web aplikacija (Microsoft 365, Google Docs, etc...)
> ℹ️ Moderne verzije MS Office-a su previše duboko integrisane sa raznim komponentama Windows-a - **neće raditi na Linuksu preko `wine`-a!**  

## Uključivanje sddm-a i reboot
```sh
systemctl enable sddm
reboot
```

Pri sledećem boot-u pokrenuće se login screen u `sddm` session manager-u.  
Nakon logovanja pokrenuće se Plasma okruženje.

## KWallet
KWallet je "secret store" koji služi za kriptovanje tajnih podataka na sistemu (sačuvane WiFi šifre, šifre sačuvane u browseru, itd.). Nije neophodan ali se preporučuje.  
Kada vam prvi put zatreba KWallet, automatski će se otvoriti dijalog za pravljenje novog wallet-a.  

- Za tip odabrati "blowfish file"
- Kao lozinku **obavezno staviti istu loziku koju koristite za linux user login**! Ukoliko to ne uradite, wallet se neće moći automatski otvarati.

> ⚠️ KWallet nije zamena za password manager! On služi samo da automatski čuva sistemske tajne. 

## Prebacivanje `sddm-a` na Wayland
Ovaj korak je opcion, ali pomaže ako se lock screen nalazi na porešnom ekranu ili ima pogrešan format datuma.

- **System Settings** ➡️ **Colors & Themes** ➡️ **Login Screen (SDDM)** ➡️ **Apply Plasma Settings**

Ovo će kreirati fajl `/etc/sddm.conf.d/kde_settings.conf`

U tom fajlu, pod `[General]` izmeniti/dodati sledeće dve linije:
```
DisplayServer=wayland
GreeterEnvironment=QT_WAYLAND_SHELL_INTEGRATION=layer-shell
```

Na kraju fajla dodati:

```
[Wayland]
CompositorCommand=kwin_wayland --drm --no-lockscreen --no-global-shortcuts --locale1
```

Sačuvati fajl i ponoviti:
- **System Settings** ➡️ **Colors & Themes** ➡️ **Login Screen (SDDM)** ➡️ **Apply Plasma Settings**