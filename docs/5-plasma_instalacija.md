# 5. Instalacija Plasma desktop okruženja
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
- `bluedevil` - Bluetooth drajver
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

## Softver
### Opšti alat
Opcioni dodatani alat čiji ekvivalenti dolaze uz Windows
- `ark` - Menadžer arhiva
- `gwenview` - Pregledač slika
- `okular` - Pregledač PDF-ova
- `kamoso` - Kamera
- `spectacle` - Screenshot alativanju HD
- `kate` - GUI tekst editor
- `qalculate-gtk` - Kalkulator (`kcalc` ne valja...)
- `kolourpaint` - Paint
- `gparted` - GUI menadžer diskova i particija
- `kweather` - Vremenska prognoza

```sh
sudo pacman -S ark gwenview okular kamoso spectacle kate qalculate-gtk kolourpaint gparted kweather
```

### Screen share
Ukoliko želite da "delite ekran" preko Discord-a, Zoom-a, itd. neophodan je `xwaylandvideobridge` AUR paket.  

- **Naziv paketa**: `xwaylandvideobridge`
- **Git clone url:** https://aur.archlinux.org/xwaylandvideobridge.git

> ℹ️ **Za instalaciju AUR paketa pogledati `3-izvori_softvera.md`, odeljak *"Arch user repository (AUR)"***

> ⚠️ "Streamovanje" video igara na Discordu daje varirajuće performanse - **uglavnom je neupotrebljivo**!  
> ⚠️ Trenutno nažalost ne postoji rešenje/alternativa za ovaj problem.


### Web pregledač
Odabrati i instalirati željeni web pregledač, navešću najpopularnije:
- `firefox`
- `chromium`
- `google-chrome` (AUR)
- `vivaldi`
- `brave` (AUR)

### Media player
Postoji gomila - navešću dva:

- `vlc` - popularan GUI player, dostupan i za Windows
    - `vlc-plugins-all` - Neophodan paket sa kodecima
- `mpv` - CLI player, ne preterano user-friendly ali veoma lightweight
    - Obratiti se zvaničnoj dokumentaciji za upotrebu: https://mpv.io/manual/stable/

### Office paket
Dve opcije su:

- `libreoffice-fresh` - veoma dobar office paket - može sve što i najnovije verzije Microsoft Office-a
    - Ume biti malo nezgrapan za upotrebu, pogotovo ako ste navikli na moderan MS Office (UI LibreOffice-a podseća na MS Office 2003)
- Korisćenje nekih od web aplikacija (Microsoft 365, Google Docs, etc...)
> ℹ️ Moderne verzije MS Office-a su previše duboko integrisane sa raznim komponentama Windows-a - **neće raditi na Linuksu preko `wine`-a!**  



## Reboot
```sh
reboot
```

Pri sledećem boot-u pokrenuće se login screen u `sddm` session manager-u.  
Nakon logovanja pokrenuće se Plasma okruženje.

## Česta podešavanja
***Budući da Plasma pruža grafički interfejs, većina podešavanja su samoobjašnjavajuća - ovde ću navesti neka najčešća koja se mogu potencijalno obaviti nakon prve instalacije sistema.*** 

> ℹ️ **Nijedno podešavanje nije obavezno.**

### Isključivanje ubrzanja miša
- System Settings ➡️ Mouse & Touchpad ➡️ Enable pointer acceleration - OFF

### Isključivanje akcije na gornjem levom ćošku ekrana
- System Settings ➡️ Mouse & Touchpad ➡️ Screen Edges ➡️ (Gornji levi kvadratić) ➡️ No Action

### Maksimizacija prozora pri pomeranju na gornju ivicu ekrana
- System Settings ➡️ Mouse & Touchpad ➡️ Screen Edges ➡️ Maximize: Windows dragged to top edge - ON

### Automtasko uključivanje NumLock-a pri pokretanju sistema
- System Settings ➡️ Keyboard ➡️ Keyboard ➡️ NumLock on startup ➡️ Turn on

### Dodavanje jezika tastature
- System Settings ➡️ Keyboard ➡️ Keyboard ➡️ Layouts Enable - ON ➡️ Add...
    > ℹ️ Za instalaciju nestandardnih metoda unosa (japanski IME, kineski Pinyin, itd.) - obratiti se dokumentu `5A-dodatan_softver` odeljak **"Dodatni metodi unosa na tastaturi (`fcitx5`)"**

### Windows shortcut za System Monitor i promenu jezika tastature
- System Settings ➡️ Keyboard ➡️ Shortcuts ➡️ System Monitor ➡️ Launch ➡️ Add... ➡️ (CTRL+SHIFT+ESC) 
- System Settings ➡️ Keyboard ➡️ Shortcuts ➡️ Keyboard Layout Switcher ➡️ Switch to Next Keyboard Layout ➡️ Add... ➡️ (ALT+SHIFT)

### Podešavanje ekrana
- System Settings ➡️ Display & Monitor
    - Podesiti odgovarajući refresh rate
    - Podesiti pozicije i primarni ekran u slučaju korišćenja više od jednog ekrana
    - Podesiti HDR
    > ⚠️ Pažnja pri uključivanju HDR-a na NVIDIA grafičkim kartama!

### Isključivanje povećanja kursora pri brzom pomeranju
- System Settings ➡️ Accessibility ➡️ Shake Cursor ➡️ Enable - OFF

### Uključivanje dark teme i promena boje kursora na belu (zašto bih hteo crni kursor na crnoj pozadini???)
- System Settings ➡️ Colors & Themes ➡️ Global Theme ➡️ Breeze Dark
- System Settings ➡️ Colors & Themes ➡️ Cursors ➡️ Breeze Light

### Promena SDDM teme
- System Settings ➡️ Colors & Themes ➡️ Login Screen (SDDM) ➡️ Breeze
- System Settings ➡️ Colors & Themes ➡️ Login Screen (SDDM) ➡️ Apply Plasma Settings

### Regionalni formati
- System Settings ➡️ Region & Language
    - **Currency, Measurements, Paper Size, Address, Name Style, Phone Numbers** promeniti na **srpski**
    - **Time**
        - Prilikom promene na srpski, promeniće se i nazivi dana u nedelji na ćirilicu
        - Umesto toga, možete odabrati **British English (United Kingdom)** i promeniti prvi dan u nedelji na ponedeljak (sledeća sekcija)

### Promena prvog dana u nedelji na ponedeljak
- Desni klik na vreme u taskbar-u ➡️ Configure Digital Clock.. ➡️ Calendar ➡️ First day of the week: Monday

### Podešavanje automatskog ulaska u Sleep mode
- System Settings ➡️ Power Management
    - Podesiti proizvoljno
