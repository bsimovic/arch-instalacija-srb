# 5.  Instalacija i konfiguracija Plasma desktop okruženja

## Paketi
### Osnovni paketi (plasma okružeje + neke opšte stvari koje Windows ima po defaultu)
- `plasma-desktop` - Plasma dekstop okruženje
- `plasma-nm` - Mrežni konfigurator
- `plasma-pa` - Audio konfigurator
- `plasma-firewall` - Firewall konfigurator
- `plasma-systemmonitor` - Task manager
- `plasma-login-manager`- Session manager
- `kscreen` - Konfigurator displeja
- `xdg-desktop-portal-kde` - Desktop portal funkcionalnost
- `powerdevil` - Power profile konfigurator
- `power-profiles-daemon` - Servis za upravljanje power profilima
- `kwalletmanager` - Konfigurator "KWallet-a" (pogledati odeljak "KWallet" dole)
- `kinfocenter` - Prikaz informacija o sistemu
- `ksystemlog` - Event log
- `partitionmanager` - GUI alat za rad sa diskovima i particijama
- `isoimagewriter` - Alat za upisivanje ISO fajlova na USB
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
- `kolourpaint` - Paint
- `kweather` - Vremenska prognoza
- `kate` - GUI editor teksta
- `gwenview` - Pregledač slika
- `spectacle` - Printscreen alat
- `noto-fonts-cjk` - Noto fontovi
- `ttf-liberation` - Liberation fontovi
- `ttf-droid` - Droid fontovi

### Web pregledač
Odabrati i instalirati željeni web pregledač, navešću najpopularnije:
- `vivaldi` (moja preporuka)
- `brave` (AUR)
- `firefox`
- `chromium`
- `google-chrome` (AUR)

> [!TIP]
> Za uputstvo za instalaciju AUR paketa pogledati dodatak `A-izvori_softvera.md`, odeljak *"Arch user repository (AUR)"*.

### Office paket
Dve opcije su:

- `libreoffice-fresh` - veoma dobar office paket - može sve što i najnovije verzije Microsoft Office-a
    - Ume biti malo nezgrapan za upotrebu, pogotovo ako ste navikli na moderan MS Office (UI LibreOffice-a podseća na MS Office 2003)
- Korisćenje nekih od web aplikacija (Microsoft 365 Online, Google Docs, etc...)

### Dodatni paketi
Ovi paketi su opcioni u zavisnosti od potreba i želje korisnika:

#### Media player
Navešću dva (od mnogih):

- `vlc` - popularan GUI player
    - `vlc-plugins-all` - neophodan paket sa kodecima za vlc
- `mpv` - CLI player, veoma lightweight
    - `mpv-mpris` - za integraciju sa operativnim sistemom

#### E-Mail klijent
Navešću dva (od mnogih):

- `thunderbird`
- `kmail`

#### Štampanje
Svi paketi su neophodni:

- `cups` - Servis za štampanje
- `cups-filters` - Filteri za štampanje, kao i drajveri za veoma stare štampače
- `system-config-printer` - Sistemski drajver za štampače
- `print-manager` - Konfigurator štampača

Uključiti cups servis nakon instalacije paketa:
```sh
sudo systemctl enable --now cups
```

#### Bluetooth (neophodno za bluetooth adaptere)
- `bluedevil`

Uključiti bluetooth servis nakon instalacije paketa:

```sh
sudo systemctl enable --now bluetooth
```
#### Skeniranje
- `skanpage`

#### Rezanje diskova
- `k3b`

#### Snimanje zvuka
- `krecorder`

#### Organizator
- `korganizer`

#### Kamera (nije neophodno za rad kamere)
- `kamoso`

#### Password manager
- `org.keepassxc.KeePassXC` (flatpak)

> [!TIP]
> Za uputstvo za instalaciju `flatpak` paketa pogledati dodatak `A-izvori_softvera.md`, odeljak *"Flathub"*.

#### Discord
- `discord`
Ili
- `com.discordapp.Discord` (flatpak)

#### Telegram
- `org.telegram.desktop` (flatpak)

#### Viber
- https://www.viber.com/en/download/ (AppImage)

> [!TIP]
> Za uputstvo za upotrebu AppImage-a pogledati dodatak `A-izvori_softvera.md`, odeljak *"AppImage"*.

#### Visual Studio Code
- `visual-studio-code-bin` (AUR)

#### Torrent klijent
- `qbittorrent`

## Finalizacija
Uključiti session manager i uraditi reboot:
```sh
sudo systemctl enable plasmalogin
reboot
```

## KWallet
KWallet je "secret store" koji služi za kriptovanje tajnih podataka na sistemu (sačuvane WiFi šifre, šifre sačuvane u browseru, itd.). Neophodan je za neometan rad desktop okruženja.  
Pokrenite "KWalletManager" i započnite kreiranje novog wallet-a ili sačekajte da sistemu prvi put zatreba KWallet, automatski će se otvoriti dijalog za pravljenje novog wallet-a.  

1. Za tip odabrati "blowfish file"
2. Kao lozinku **obavezno staviti istu loziku koju koristite za linux user login**!
3. Instalirati paket `kwallet-pam`. (služi za automatsko otvaranja wallet-a nakon logovanja)

> [!WARNING]
> KWallet nije zamena za password manager! On služi samo da automatski čuva sistemske tajne. 
