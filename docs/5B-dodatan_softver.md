# 5.B Dodatan softver
***Softver opšte ili specifične namene koji možda želite a možda i ne.***

## Bolji shell (`zsh`)
```sh
sudo pacman -S zsh
```

Promena default shell-a:
```
chsh -s $(which zsh)
```

Pri prvom pokretanju, pratite uputstvo na ekranu za konfiguraciju `zsh`-a.  

Za lepši prompt i obojeni prikaz defaultnih linux alata, dodati u `~/.zhsrc`:
```
PROMPT='%F{green}%T%f %B%F{lightblue}%d%f%F{green}>%f%b '

alias ls='ls --color=auto'
alias dir='dir --color=auto'
alias vdir='vdir --color=auto'
alias grep='grep --color=auto'
alias fgrep='fgrep --color=auto'
alias egrep='egrep --color=auto'
```

## Discord
**Preporučujem korišćenje flathub paketa**, zbog toga što je on jedini zvanično verifikovan i održavan od strane Discord developera.  

> ⚠️ Zbog flatpak sandbox-a, "rich presense" funkcionalnost (prikaz trenutno pokrenute igre) neće raditi.  
> ⚠️ Ako vam je to neophodno, instalirajte `discord` paket preko pacman-a. 

```sh
flatpak install discord
```

## Telegram
```
sudo pacman -S telegram-desktop
```

## Dodatni metodi unosa na tastaturi
```
sudo pacman -S fcitx5
```

***WIP***