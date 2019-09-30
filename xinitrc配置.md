```
xset s off -dpms  
xrandr --output eDP-1 --scale 0.7x0.7

export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export ECORE_IMF_MODULE=xim
export XMODIFIERS=@im=fcitx
fcitx -r -d

[[ -f ~/.Xmodmap ]] && xmodmap ~/.Xmodmap

exec awesome
```
