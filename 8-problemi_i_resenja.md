# 6. Problemi i rešenja

***Rešenja za probleme na koje sam lično naišao pri upotrebi operativnog sistema.***

## Povremeno pucketanje iz zvučnika
```sh
echo "options snd_hda_intel power_save=0" | sudo tee -a /etc/modprobe.d/audio_disable_powersave.conf
```
## Hibernacija ne radi
Pogledati dokument `1-arch_instalacija.md`, odeljak **"Uključivanje resume hook-a za hibernaciju sistema"**.

## `sddm` na pogrešnom ekranu ili pogrešan format vremena i datuma 
U nastavku postupak prebacivanja sddm-a na Wayland.  
Ukoliko nije već urađeno:  
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

## Greška pri pokušaju mount-ovanja NTFS particije
Ukoliko se javi greška:
```
wrong fs type, bad option, bad superblock on dev_file, missing codepage or helper program, or other error
```

Uraditi sledeće:
```sh
sudo pacman -S --needed ntfs-3g
sudo ntfsfix -d dev_file_diska
```

## Sistem ne prepoznaje zvučni uređaj koji podržava virtuelni Surround

Napraviti novu Pipewire konfiguraciju:
```sh
mkdir ~/.config/pipewire
cp /usr/share/pipewire/pipewire.conf ~/.config/pipewire/pipewire.conf
```

Kopirati sadržaj `assets/surround.conf` datoteke sa ovog git repozitorijuma u `~/.config/pipewire/pipewire.conf` pod `context.modules`, dakle:

```conf
# Pronaći liniju context.modules
...
context.modules = [
...

# Kopirati sadžaj surround.conf ovde
]
...
```

Preuzeti odgovarajući HeSuVi `.wav` fajl sa https://airtable.com/appayGNkn3nSuXkaz/shruimhjdSakUPg2m/tbloLjoZKWJDnLtTc.   
Npr. ako imate Razer slušalice, kliknite na "*Download WAV*" u "*razer*" redu.
Sačuvajte ga na lokaciji: `~/.config/pipewire` sa imenom `hesuvi.wav`.

Unutar `~/.config/pipewire/pipewire.conf` uradite find and replace (CTRL+h) - zameniti string `hrir_hesuvi/hrir.wav` sa ***apsolutnom putanjom do `hesuvi.wav` fajla***, dakle: `/home/VAŠ_USERNAME/.config/pipewire/hesuvi.wav`.

Restartovati računar, i pri sledećem boot-u promeniti sound output device na **Virtual Surround Sink**.
## Crn ekran nakon uključivanja HDR-a

***WIP***