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

***WIP***

## Crn ekran nakon uključivanja HDR-a

***WIP***