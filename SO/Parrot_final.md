Instalar vspwm y xshkd

bspwm: Gestor de ventanas tipo "tiling" (organiza ventanas automáticamente).
sxhkd: Permite configurar atajos de teclado personalizados para controlar bspwm

```
sudo apt update && sudo apt install bspwm sxhkd -y
```

Error : Firmas

```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 7A8286AF0E81EE4A
```

Error : Entradas duplicadas

```
sudo nano /etc/apt/sources.list
```

Eliminar lineas repetidas

```
sudo nano /etc/apt/sources.list.d/parrot.list
```

lo mismo

Limpiar y actualizar paquetes 

```
sudo apt clean && sudo apt update && sudo apt upgrade -y
```

Crear la estructura de configuración

0.1 bspwm
¿Qué es?

Es un gestor de ventanas en mosaico (tiling window manager) que delega la asignación de teclas a sxhkd.
Permite un control muy fino del posicionamiento de ventanas y una personalización extrema.
0.2 sxhkd
¿Qué es?

Es un daemon para gestionar atajos (hotkeys) en X.
Se usa junto con bspwm para asignar funciones a combinaciones de teclas (por ejemplo, abrir una terminal, mover ventanas, etc.).
0.3 picom
¿Qué es?

Es un compositor para Xorg (antes se conocía como Compton).
¿Para qué sirve?
Proporciona efectos visuales como transparencias, sombras y animaciones suaves.
Es útil para emular la apariencia “suave” de macOS.


```
mkdir -p ~/.config/bspwm
mkdir -p ~/.config/sxhkd
```

```
nano ~/.config/bspwm/bspwmrc
```
```
#!/bin/bash
sxhkd &   # Habilita los atajos de teclado
nitrogen --restore &   # Configura el fondo de pantalla (cambia por feh si prefieres)
picom -b &   # Agrega transparencias y mejora gráficos
exec bspwm   # Inicia BSPWM
```

```
chmod +x ~/.config/bspwm/bspwmrc
```
Configurar sxhkd (atajos de teclado básicos)

```
nano ~/.config/sxhkd/sxhkdrc
```
```
# Reiniciar bspwm
super + Escape
    bspc wm -r

# Abrir terminal
super + Return
    alacritty

# Cerrar ventana activa
super + q
    bspc node -c

# Moverse entre ventanas
super + {h,j,k,l}
    bspc node -f {west,south,north,east}

# Cambiar de escritorio
super + {1-9}
    bspc desktop -f {1-9}

# Mover ventana a otro escritorio
super + shift + {1-9}
    bspc node -d {1-9}

# Alternar modo flotante
super + space
    bspc node -t floating
```

```
pkill -USR1 -x sxhkd
sxhkd &
```

Super = Windows o Cmd

Probar bspwm

Simplemente nos salimos y elegimos la opción de bspwm a la hora de poner la contraseña

![image](https://github.com/user-attachments/assets/05efc0cc-6562-4604-9164-8af303dbd396)

```
sudo apt install alacritty polybar feh picom rofi -y
```

Fallo:

```
sudo apt --fix-broken install
sudo apt update --fix-missing
sudo dpkg --configure -a
```

```
sudo apt install alacritty polybar feh rofi -y
```

```
sudo apt install aptitude -y
sudo aptitude install picom
```

Si no pues compilarlo vosotros mismos

```
sudo apt install meson ninja-build cmake libxext-dev libxcb1-dev libxcb-damage0-dev \
libxcb-xfixes0-dev libxcb-shape0-dev libxcb-render-util0-dev libxcb-render0-dev \
libxcb-randr0-dev libxcb-composite0-dev libxcb-image0-dev libxcb-present-dev \
libxcb-xinerama0-dev libxcb-glx0-dev libpixman-1-dev libdbus-1-dev \
libconfig-dev libegl-dev libpcre2-dev libev-dev uthash-dev libx11-xcb-dev -y
```

```
git clone https://github.com/yshui/picom.git
cd picom
meson setup --buildtype=release build
ninja -C build
sudo ninja -C build install
```

Es muy probable que haya problemas con dependencias recomiendo utilizar chat gptp (me ha dado muchisimos problemas picom)

0.4 Polybar
¿Qué es?

Es una barra de estado altamente configurable que puede mostrar información del sistema, el estado de la batería, red, etc.
La configuraremos con un tema que se asemeje a macOS.
0.5 zsh
¿Qué es?

Es una shell avanzada y muy configurable.
¿Por qué zsh?
Ofrece autocompletado avanzado, templatización y personalizaciones (por ejemplo, con Oh My Zsh o powerlevel10k).
¿Cómo asegurarte de usarla en bspwm?
bspwm no “abre” una shell de forma predeterminada, sino que tú inicias terminales. Configurando zsh como shell por defecto, cada terminal que abras (por ejemplo, con Alacritty, Kitty o cualquier emulador de terminal) arrancará con zsh.

Fondo de pantalla
Para evitar ver una pantalla negra, usaremos un programa como feh o nitrogen para asignar un fondo de pantalla.
Instalación (usando feh):
bash
Copiar

sudo apt install feh






