# Grafički drajver
## NVIDIA
Glavni paket u zavisnosti od graficke karte:
- `nvidia-open` - GTX 1650 i novije serije (GTX 20, 30, 40, 50...)
- `nvidia` - GTX 750 do GTX 10 serije

**Za veoma stare NVIDIA grafičke karte, pogledati wiki: https://wiki.archlinux.org/title/NVIDIA**  
Dodatni paketi:
- `nvidia-utils`
- `lib32-nvidia-utils` (multilib)
- `nvidia-settings`
- `vulkan-icd-loader`
- `lib32-vulkan-icd-loader` (multilib)
- `nvtop`

Instalirati jedan glavni paket i sve dodatne pakete putem `pacman`-a.

## AMD
**Za veoma stare AMD/ATI grafičke karte, pogledati wiki: https://wiki.archlinux.org/title/AMDGPU**  

Paketi:
- `mesa`
- `lib32-mesa` (multilib)
- `vulkan-radeon`
- `lib32-vulkan-radeon` (multilib)
- `vulkan-icd-loader`
- `lib32-vulkan-icd-loader` (multilib)
- `amdgpu_top`

Instalirati sve pakete putem `pacman`-a.

## Intel
**Za veoma stare Intel grafičke karte, pogledati wiki: https://wiki.archlinux.org/title/Intel_graphics**  

Paketi:
- `mesa`
- `lib32-mesa` (multilib)
- `vulkan-intel`
- `lib32-vulkan-intel` (multilib)
- `vulkan-icd-loader`
- `lib32-vulkan-icd-loader` (multilib)
- `nvtop`

Instalirati sve pakete putem `pacman`-a.