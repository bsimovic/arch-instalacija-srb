# 6. Problemi i rešenja

***Rešenja za probleme na koje sam lično naišao pri upotrebi operativnog sistema.***

## Povremeno pucketanje iz zvučnika
```sh
echo "options snd_hda_intel power_save=0" | sudo tee -a /etc/modprobe.d/audio_disable_powersave.conf
```
## Hibernacija ne radi
Pogledati dokument `1-arch_instalacija.md`, odeljak **"Uključivanje resume hook-a za hibernaciju sistema"**.

## Problemi sa NTFS particijama
**Ukoliko ne možete bootovati sistem zbog loše definisane NTFS particije, privremeno je zakomentarisatu u `/etc/fstab` fajlu (dodati `#` ispred linije).**

Najpre instalirati paket `ntfs-3g`:
```sh
sudo pacman -S --needed ntfs-3g
```

Sada, ispraviti `fstab`, otkomentarisati liniju NTFS particije i zameniti `ntfs` drajver sa `ntfs-3d`.  
Primer ispravnog `fstab` unosa za NTFS:
```
UUID=xxxxxxxxxxxxxxxxx  /hdd0   ntfs-3g     rw,nosuid,nodev,noexec,uid=0,gid=0,umask=0000,allow_other,noatime,windows_names   0 0
```

### Wrong fs type 
Ukoliko se javi greška:
```
wrong fs type, bad option, bad superblock on dev_file, missing codepage or helper program, or other error
```

Uraditi sledeće:
```sh
sudo ntfsfix -d dev_file_diska
```

## Ekran neće da se uključi nakon sleep mod-a na Nvidia grafičkoj karti
```sh
echo "options nvidia NVreg_PreserveVideoMemoryAllocations=1" | sudo tee -a /etc/modprobe.d/nvidia.conf
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
