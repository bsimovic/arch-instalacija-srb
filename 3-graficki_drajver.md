# 4. Grafički drajver
Instalirati skup paketa koji odgovara proizvođaču grafičke karte.  
Ukoliko ista mašina ima dva grafička adaptera različitih proizvođača (uglavnom slučaj kod laptopova), instalirati pakete za oba.

> [!TIP]
> Za instalaciju paketa koristiti `pacman`.
> Pogledati dodatak `A-izvori_softvera.md` odeljak *"pacman package manager"*.
.

## NVIDIA
### Novije serije (GTX 16xx, RTX 20xx, 30xx, 40xx, 50xx...)
- `nvidia-open`
- `nvidia-utils`
- `lib32-nvidia-utils` (multilib)
- `nvidia-settings`
- `vulkan-icd-loader`
- `lib32-vulkan-icd-loader` (multilib)

### Starije serije (Od GTX 7xx do GTX 10xx)
- `nvidia-580xx-dkms` (AUR)
- `nvidia-580xx-utils` (AUR)
- `lib32-nvidia-580xx-utils` (AUR)
- `nvidia-settings`
- `vulkan-icd-loader`
- `lib32-vulkan-icd-loader` (multilib)

> [!TIP]
> Paketi obeleženi sa (AUR) se nalaze na Arch user repository i **ne mogu se instalirati preko `pacman`-a**.  
> Za uputstvo za instalaciju AUR paketa pogledati dodatak `A-izvori_softvera.md`, odeljak *"Arch user repository (AUR)"*.

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

### Veoma stare Intel grafičke karte
Pogledati wiki: https://wiki.archlinux.org/title/Intel_graphics

