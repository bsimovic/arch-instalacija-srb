# 1.4 Grafički drajver
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

**`nvidia-open`:**
```sh
sudo pacman -S nvidia-open nvidia-utils lib32-nvidia-utils nvidia-settings vulkan-icd-loader lib32-vulkan-icd-loader nvtop
```

**`nvidia`:**
```sh
sudo pacman -S nvidia nvidia-utils lib32-nvidia-utils nvidia-settings vulkan-icd-loader lib32-vulkan-icd-loader nvtop
```


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

```
sudo pacman -S mesa lib32-mesa vulkan-radeon lib32-vulkan-radeon vulkan-icd-loader lib32-vulkan-icd-loader amdgpu_top
```


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

```
sudo pacman -S mesa lib32-mesa vulkan-intel lib32-vulkan-intel vulkan-icd-loader lib32-vulkan-icd-loader nvtop
```
