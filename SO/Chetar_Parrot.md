# Descargar Parrot

Nos descargamos la iso en la web oficial : parrotsec.org

Elegimos la versión Parrot Security (Pentesting) o Parrot Home (Personalización)

Voy a explicar paso a paso mi proceso de instalación, errores que me han salido, soluciones, aprendizaje que he hecho en este camino. Espero que os guste

# Instalación en VM

Esta parte no la voy a explicar porque realmente nos vamos a centrar en lo posterior pero simplemente montarte una máquina en VM e instalarlo, empezamos con el sistema ya instalado

# Configurar parrot

## Actualizar sistema

Abrimos terminal

```
sudo apt update && sudo apt full-upgrade -y
```

A mi me salio un error de fallo de claves GPG

Solucion:

```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 7A8286AF0E81EE4A
```

```
sudo apt-key adv --refresh-keys --keyserver keyserver.ubuntu.com
```

## Agregar nuestro usuario al grupo sudo

Esto no es obligatorio, pero nos aseguramos.

```
sudo usermod -aG sudo tu_usuario
```

## Instalar paquetes senciales

```
sudo apt install -y curl git neofetch htop tmux python3-pip
```





