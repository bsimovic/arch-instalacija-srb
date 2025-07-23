# 5. Instalacija Plasma desktop okruženja
> ℹ️ **Ukoliko želite neko drugo desktop okruženje (ili ne želite nijedno), možete preskočiti ovaj dokument.**

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
- `power-profiles-daemon` - Servis za upravljanje power profilima
- `kde-gtk-config` - Sinhronizuje sistemsku temu sa GTK aplikacijama
- `breeze-gtk` - KDE Breeze tema za GTK
- `cups` - Servis za štampanje
- `print-manager` - Menadžer štampača
- `konsole` - Terminal emulator
- `dolphin` - File explorer
- `noto-fonts-cjk` - Noto fontovi
- `ttf-liberation` - Liberation fontovi
- `ttf-droid` - Droid fontovi

```sh
sudo pacman -S plasma-desktop plasma-nm plasma-pa plasma-firewall plasma-systemmonitor kscreen kinfocenter ksystemlog kwalletmanager sddm sddm-kcm xdg-desktop-portal-kde bluedevil powerdevil power-profiles-daemon kde-gtk-config breeze-gtk cups print-manager konsole dolphin noto-fonts-cjk ttf-liberation ttf-droid
```

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
    > ℹ️ Za instalaciju nestandardnih metoda unosa (japanski IME, kineski Pinyin, itd.) - obratiti se dokumentu `5B-dodatan_softver` odeljak **"Dodatni metodi unosa na tastaturi (fcitx5)"**

### Keyboard shortcut za System Monitor i promenu jezika tastature
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
