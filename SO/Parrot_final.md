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

