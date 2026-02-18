# 4. Grafički drajver
Instalirati skup paketa koji odgovara proizvođaču grafičke karte.  
Ukoliko imate grafički hardver dva (ili više) različita proizvođača na istoj mašini (uglavnom slučaj kod laptopova), instalirati pakete za oba.

## NVIDIA
Odabrati skup paketa u zavisnosti od specifične NVIDIA grafičke karte.

### Novije serije (GTX 16xx, RTX 20xx, 30xx, 40xx, 50xx...)
- `nvidia-open`
- `lib32-nvidia-utils` (multilib)
- `nvidia-settings`
- `vulkan-icd-loader`
- `lib32-vulkan-icd-loader` (multilib)

```sh
sudo pacman -S nvidia-open lib32-nvidia-utils nvidia-settings vulkan-icd-loader lib32-vulkan-icd-loader
```

### Starije serije (Od GTX 7xx do GTX 10xx)
- `nvidia-580xx-dkms` (AUR)
- `lib32-nvidia-580xx-utils` (AUR)
- `nvidia-settings`
- `vulkan-icd-loader`
- `lib32-vulkan-icd-loader` (multilib)

> [!TIP]
> Paketi `nvidia-580xx-dkms` i `lib32-nvidia-580xx-utils` se nalaze na AUR (Arch user repository) i **ne mogu se instalirati preko `pacman`-a**!!  
> Za uputstvo za instalaciju AUR paketa pogledati dodatak `B-dodatni_izvori_softvera.md`, odeljak *"Arch user repository (AUR)"*.

```sh
sudo pacman -S nvidia-settings vulkan-icd-loader lib32-vulkan-icd-loader
```

### Serije starije od GTX 7xx
Pogledati wiki: https://wiki.archlinux.org/title/NVIDIA

## AMD
Paketi:
- `mesa`
- `lib32-mesa` (multilib)
- `vulkan-radeon`
- `lib32-vulkan-radeon` (multilib)
- `vulkan-icd-loader`
- `lib32-vulkan-icd-loader` (multilib)
- `amdgpu_top`

```sh
sudo pacman -S mesa lib32-mesa vulkan-radeon lib32-vulkan-radeon vulkan-icd-loader lib32-vulkan-icd-loader
```

### Veoma stare AMD grafičke karte
Pogledati wiki: https://wiki.archlinux.org/title/AMDGPU

## Intel
Paketi:
- `mesa`
- `lib32-mesa` (multilib)
- `vulkan-intel`
- `lib32-vulkan-intel` (multilib)
- `vulkan-icd-loader`
- `lib32-vulkan-icd-loader` (multilib)

```sh
sudo pacman -S mesa lib32-mesa vulkan-intel lib32-vulkan-intel vulkan-icd-loader lib32-vulkan-icd-loader
```

### Veoma stare Intel grafičke karte
Pogledati wiki: https://wiki.archlinux.org/title/Intel_graphics

