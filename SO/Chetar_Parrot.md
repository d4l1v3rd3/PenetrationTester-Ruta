# Descargar Parrot

Nos descargamos la iso en la web oficial : parrotsec.org

Elegimos la versión Parrot Security (Pentesting) o Parrot Home (Personalización)

Voy a explicar paso a paso mi proceso de instalación, errores que me han salido, soluciones, aprendizaje que he hecho en este camino. Espero que os guste

# Instalación en VM

Esta parte no la voy a explicar porque realmente nos vamos a centrar en lo posterior pero simplemente montarte una máquina en VM e instalarlo, empezamos con el sistema ya instalado.

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

En mi caso estas aplicaciones ya vienen con parrot, pero por si os habeis instalado la HOME

```
sudo apt install -y curl git neofetch htop tmux python3-pip
```

# Instalar ZSH + Oh My ZHS + Powerlevel10k

```
sudo apt install -y zsh
chsh -s $(which zsh)
```

Reiniciamos la máquina

![image](https://github.com/user-attachments/assets/369ee89c-2ce3-4629-be7b-c744758e18fa)

Nos saldrá esto cuando reniciemos, diciendo que zsh está configurado para iniciar un asistnete de configuración, cuando no encuentra el archivo .zshrc. 

q- Salir sin hacer nada, y cuando volvamos a abrir saldra el mensjae
0- Salir y crear un arhivo
1- Continuar el menu princial  y permite configurar varios aspectos
2- CRea un archivo con configuraciones por defecto (Este vamos a utilizar por ahora)

## Instalar Oh my zsh

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

![image](https://github.com/user-attachments/assets/86b22e9c-9f53-4e2c-af82-99200b214549)

## Instalar Powerlevel10K (tema avanzado para ZSH)

```
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Vamos a editar el archivo .zshrc

```
nano ~/.zshrc
```

Y cambiamos la linea por el tema que nosotros queramos, en mi caso como he elegido el powerlevel10k quedaría a sí

```
ZSH_THEME="powerlevel10k/powerlevel10k"
```

Volvemos a reiniciar y ejecutamos el asistente gráfico

```
exec zsh
```

Ahora iremos configurando PowerLevel10K

## Preguntas

- Sobre el símbolo de diamante

y (si tu terminal quiere mostrar caracteres especiales)
n (problemas con caracteres especiales)

yo eligo y

- La siguiente y

- y

- 1 (para seleccionar los del grupo 1)

- En la siguiente verifica si los íconos caben correctamente entre los cruces, en mi caso estan overlap con lo cual n

![image](https://github.com/user-attachments/assets/d5847ca2-05d0-463a-a0d8-89b4a54795d1)

Esto nos llevara a ajustes alternativos

Aquí elegiremos la opción que nosotros creamos que nos viene mejor oen micaso he elegido 3 

y luego eliges entre unicode o asscii en mi caso he elegido la primera

Luego nos dice si queremos ver la hora, para mi me gusta verla.

Luego el separado si quremos en mi caso he elegido slanted, posterior elegimos la opción que mas nos guste en mi caso vuelvo a legir slanted

Y bueno como veis la dinámica vais configurando como os va gustando, si no os gusta algo podeis restablecer con la r

La mia por ahora queda así, se ve fea pero vamos a aprender a mejorarla

![image](https://github.com/user-attachments/assets/12d4a80f-bb2a-421f-ae50-8fd94e6c6a30)

Si en cualquier caso la queremos volver a configurar ponemos el comando

```
p10k configure
```

# Añadir plugins útiles a la Zsh

Archivo de configuración

```
nano ~/.zshrc
```

VOy a instalar:

- zsh-autosuggestions: Sugiere comandos basados en el historia
- zsh-syntax-highiligthing: Colorea y valida comandos que escribimos
- zsh-completions: Mejora el autocompletado

```
sudo apt install zsh-autosuggestions zsh-syntax-highlighting
echo "source /usr/share/zsh-autosuggestions/zsh-autosuggestions.zsh" >> ~/.zshrc
echo "source /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ~/.zshrc
```

Para recargar la configuracion

```
source ~/.zshrc
```

Y como veís después de esto cuando vayamos poniendo comandos se nos autocompletaran

![image](https://github.com/user-attachments/assets/981a22ab-37de-45b5-8599-7af3e918633d)

Sabiendo como funciona y como añadirlo podeis ver plugins de la zsh y instarlar los que querais

# Herramientas esenciales de hacking

```
sudo apt install metasploit-framework nmap nikto gobuster john hashcat hydra sqlmap aircrack-ng burpsuite
```

Normalmente todas estas ya están pero son las básicas

Por aquí os dejo las mejores herramientas consideradas por hacker101 divididas en categorías 

https://www.hacker101.com/resources

Nos vamos a coger también herramientas guapas como impacket o linPEAS que quien lo sepa pues que aprenda que son obligatorias coño

```
git clone https://github.com/SecureAuthCorp/impacket.git
cd impacket
pip install .
```

```
git clone https://github.com/carlospolop/PEASS-ng.git
```

# Crear scripts y alias personalizas

SI queremos crear comandos personalizados para nosotros vamos a ello

Gracias a la zshrc podemos poner por ejemplo

```
alias ll='ls -la'
alias recon='nmap -sC -sV -oN nmap_scan'
alias gobust='gobuster dir -u http://target -w /usr/share/wordlists/dirb/common.txt'
alias myip='ip a | grep inet'
```

Cada uno de eso se va a quedar guardado como una variable por ejemplo si ahora ponemos ll nos hara el comando definido

![image](https://github.com/user-attachments/assets/b8ba832f-9330-4d44-af7d-3178346b5d76)

Para reiniciar si no funciona el comando anterior "source ruta"

# Personalizar entorno gráfico

Yo en mi caso instalare gnome

```
sudo apt install gnome-tweaks
```

Y si queremos una terminal más personalizable tilix

```
sudo apt install tilix
```

Vamos a ahacerlo mas chulo

```
sudo apt install gnome-session gnome-shell gnome-control-center gdm3 -y
```

Yo he tenido problemas con dependencias, solución

```
sudo apt install aptitude
```

EN mi caso esta dependencia fallaba y en zed de instalar con apt he instalado con aptitude

```
sudo aptitude install gstreamer1.0-pipewire
```

También otro fallo que tenía era que tenía clonados repositorios en 

```
sudo nano /etc/apt/sources.list.d/parrot.list
```

Fijaros que no teneis los mismos repositorios que en /etc/apt/sources.list

Otros errores que me han salido ha sido que la libreria libpipewire daba errores solución:

```
sudo autoremove
```
```
sudo apt install libpipewire-0.3-0=0.3.65-3+deb12u1
sudo apt install libpipewire-0.3-0 --fix-missing
```

Los he instalado manualmente y todo ha ido bien una vez podemos seguir

![image](https://github.com/user-attachments/assets/ca08b6cd-0f5f-4596-9589-619f2e13e7df)

Elegimos como principal el gdm3

y reiniciamos el sistema reboot

Una vez entreis os vais a rallar porque no vais a ver ningún icono de escritorio y vais a decir guauuu bueno buscaos la vida para encontrar de nuevo la terminal que no es complicado (Ctrl + Alt + T)

Walpapers guapos: https://wallhaven.cc

```
firefox
```

Ya sabeis como crear atajos por ejemplo alias moz='firefox'

Una vez tengamos todo vamos a intalar los temas e iconos gracias a GNOME

```
sudo apt install gnome-tweaks -y
```

Os dejo unos temas por aqui si no vosotros buscaros la vida jaja

```
sudo apt install materia-gtk-theme -y
sudo apt install arc-theme -y
```

Abrimos 

```
gnome-tweaks
```

![image](https://github.com/user-attachments/assets/0bfd45b5-7561-422e-ad4a-9676e0d2a04e)

Vais probando y el que mas os guste señores

# Configurar Terminal

```
sudo apt install tilix -y
```

SI queremos con GNOME que es mi caso

```
sudo apt install gnome-shell-extensions chrome-gnome-shell -y
```

extensions.gnome.org

























