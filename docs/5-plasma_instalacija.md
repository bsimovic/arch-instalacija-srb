# 5 Instalacija Plasma desktop okruženja
## Paketi
### Neophodni paketi

- `plasma-desktop` - Plasma dekstop okruženje
- `plasma-nm` - Mrežni konfigurator
- `plasma-pa` - Audio konfigurator
- `plasma-firewall` - Firewall konfigurator
- `plasma-systemmonitor` - Task manager
- `kscreen` - Konfigurator displeja
- `kinfocenter` - Prikaz informacija o sistemu
- `ksystemlog` - Event log
- `kwalletmanager` - Konfigurator "KWallet" password managera - potrebno iako se ne koristi jer se neki moduli (npr. WiFi) bune ako ga nema
- `sddm` - Menadžer sesija
- `sddm-kcm` - Konfigurator sddm-a
- `xdg-desktop-portal-kde` - Potrebno za razne desktop funkcionalnosti (file picker, drag-and-drop, itd.)
- `bluedevil` - Bluetooth konfigurator
- `powerdevil` - Power profile konfigurator
- `power-profiles-daemon` - Integriše se sa `powerdevil`
- `kde-gtk-config` - Sinhronizuje sistemsku temu sa GTK aplikacijama
- `breeze-gtk` - KDE Breeze tema za GTK
- `cups` - Server za štampanje
- `print-manager` - Menadžer štampača
- `konsole` - Terminal emulator
- `dolphin` - File explorer
- `noto-fonts-cjk` - Fontovi za azijske jezike

```sh
sudo pacman -S plasma-desktop plasma-nm plasma-pa plasma-firewall plasma-systemmonitor kscreen kinfocenter ksystemlog kwalletmanager sddm sddm-kcm xdg-desktop-portal-kde bluedevil power-profiles-daemon kde-gtk-config breeze-gtk cups print-manager konsole dolphin noto-fonts-cjk
```

### Dodatni alat
Opcioni dodatani alat čiji ekvivalenti dolaze uz Windows
- `ark` - Menadžer arhiva
- `spectacle` - Screenshot alat
- `kate` - GUI tekst editor
- `kcalc` - Kalkulator
- `kolourpaint` - Paint
- `gwenview` - Pregledač slika
- `gparted` - GUI menadžer diskova i particija
- `kweather` - Vremenska prognoza

```sh
sudo pacman -S ark spectacle kate kcalc kolourpaint gwenview gparted kweather
```
### Web browser
Odabrati i instalirati željeni web pregledač, navešću najpopularnije:
- `firefox`
- `chromium`
- `google-chrome` (AUR)
- `vivaldi`
- `brave` (AUR)

Za ostale potražiti po repozitorijumima ili skinuti sa oficijalnog sajta.

## Reboot
```sh
sudo reboot
```

Pri sledećem boot-u, pokrenuće se default `sddm` login screen.  
Nakon logovanja pokrenuće se Plasma okruženje.