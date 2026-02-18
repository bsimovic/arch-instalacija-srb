# 5.  Instalacija i konfiguracija Plasma desktop okruženja

> [!NOTE]
> Ukoliko želite neko drugo desktop okruženje (ili ne želite nijedno), možete preskočiti ovaj dokument.

## Paketi
### Osnovni paketi
Počećemo od minimalne instalacije za svakodnevnu upotrebu:

- `plasma-desktop` - Plasma dekstop okruženje
- `plasma-nm` - Mrežni konfigurator
- `plasma-pa` - Audio konfigurator
- `plasma-firewall` - Firewall konfigurator
- `plasma-systemmonitor` - Task manager
- `plasma-login-manager`- Session manager
- `kscreen` - Konfigurator displeja
- `xdg-desktop-portal-kde` - Desktop portal funkcionalnost (deljenje fajlova, otvaranje URL-ova, stampanje, itd.)
- `powerdevil` - Power profile konfigurator
- `power-profiles-daemon` - Servis za upravljanje power profilima
- `kwalletmanager` - Konfigurator "KWallet-a" (pogledati odeljak "KWallet" dole)
- `kinfocenter` - Prikaz informacija o sistemu
- `ksystemlog` - Event log
- `partitionmanager` - GUI alat za rad sa diskovima i particijama
- `breeze-gtk` - KDE Breeze tema za GTK
- `kde-gtk-config` - Sinhronizuje sistemsku temu sa GTK aplikacijama
- `kdeplasma-addons` - Dodatne sitnice za plazmu (dodatni widgeti, picture of the day, itd.)
- `konsole` - Terminal
- `dolphin` - File explorer
- `ark` - Menadžer kompresovanih fajlova
- `7zip` - 7z podrška
- `unrar` - rar podrška (samo otvaranje `rar` arhiva, algoritam za kompresiju je patentiran i ne može se koristiti van WinRar softvera)
- `kcalc` - Kalkulator
- `kclock` - Tajmer, štoperica, itd.
- `kate` - GUI editor teksta
- `gwenview` - Pregledač slika
- `spectacle` - Printscreen alat
- `noto-fonts-cjk` - Noto fontovi
- `ttf-liberation` - Liberation fontovi
- `ttf-droid` - Droid fontovi

```sh
sudo pacman -S plasma-desktop plasma-nm plasma-pa plasma-firewall plasma-systemmonitor plasma-login-manager kscreen xdg-desktop-portal-kde powerdevil power-profiles-daemon kwalletmanager kinfocenter ksystemlog partitionmanager breeze-gtk kde-gtk-config konsole dolphin ark 7zip unrar kcalc kclock kate gwenview spectacle noto-fonts-cjk ttf-liberation ttf-droid
```

Uključiti session manager:
```sh
sudo systemctl enable plasmalogin
```

### Dodatni paketi
Ovi paketi su opcioni u zavisnosti od potreba i želje korisnika:

#### Štampanje (neophodno za rad štampača)
- `cups` - Servis za štampanje
- `cups-filters` - Filteri za štampanje, kao i drajveri za veoma stare štampače
- `system-config-printer` - Sistemski drajver za štampače
- `print-manager` - Konfigurator štampača

```sh
sudo pacman -S cups cups-filters system-config-printer print-manager
```

Uključiti cups servis:

```sh
sudo systemctl enable --now cups
```

#### Bluetooth (neophodno za bluetooth adaptere)
- `bluedevil`

```sh
sudo pacman -S bluedevil
```

Uključiti bluetooth servis:

```sh
sudo systemctl enable --now bluetooth
```

### Kamera (nije neophodno za rad kamere)
- `kamoso`

```sh
sudo pacman -S kamoso
```

### Web pregledač
Odabrati i instalirati željeni web pregledač, navešću najpopularnije:
- `vivaldi` (moja preporuka)
- `brave` (AUR)
- `firefox`
- `chromium`
- `google-chrome` (AUR)

> [!TIP]
> Za uputstvo za instalaciju AUR paketa pogledati dodatak `B-dodatni_izvori_softvera.md`, odeljak ***"Arch user repository (AUR)"***.

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

> [!NOTE]
> Moderne verzije MS Office-a su previše duboko integrisane sa raznim komponentama Windows-a - **neće raditi na Linuksu preko `wine`-a.**  

## Reboot
```sh
reboot
```

Pri sledećem boot-u pokrenuće se grafički intefejs.  

## KWallet
KWallet je "secret store" koji služi za kriptovanje tajnih podataka na sistemu (sačuvane WiFi šifre, šifre sačuvane u browseru, itd.). Neophodan je za neometan rad desktop okruženja.  
Pokrenite "KWalletManager" i započnite kreiranje novog wallet-a ili sačekajte da sistemu prvi put zatreba KWallet, automatski će se otvoriti dijalog za pravljenje novog wallet-a.  

1. Za tip odabrati "blowfish file"
2. Kao lozinku **obavezno staviti istu loziku koju koristite za linux user login**! Ukoliko to ne uradite, wallet se neće moći automatski otvarati.
3. Instalirati paket `kwallet-pam`. (služi za automatsko otvaranja wallet-a nakon logovanja)

```sh
sudo pacman -S kwallet-pam
```

> [!WARNING]
> KWallet nije zamena za password manager! On služi samo da automatski čuva sistemske tajne. 
