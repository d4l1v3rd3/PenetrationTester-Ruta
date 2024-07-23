#by d4l1

<h1 align="center">HYDRA</h1>

# INTRODUCCIÓN

Hydra es un programa de crackeo de contraseñas por fuerza bruta.

Puede funcionar con una lista "Fuerza bruta". Es como imaginar meterse a un servicio (SSH, web, FTP) y probarlo, usamos hydra para que lo haga utomaticamente.

## INSTALAR HYDRA

```
apt install hydra
```
o
```
dnf install hydra
```

# COMANDOS

Las opciones que tneemos en hydra son por ejemplo 

```
hydra -l user -P pass.txt ftp://
```

para FTP

### SSH

```
hydra -l user -P pass.txt ip -t 4 ssh
```

![image](https://github.com/user-attachments/assets/e1c295f7-5944-44d2-bf04-9f98cf14a5bd)

Por ejemplo:

```
hydra -l root -p pass.txt ip -t 4 ssh
```
- Usamos el root como usario
- pass.txt como lista
- 4 intentos -t4

### POST WEB FORM

Podemos usar hydra en las páginas. Para hacer peticiones GET o POST comunmentes usados.

```
sudo hydar usuario pass.txt ip http-post-form "ruta:login:invalidresponse"
```

![image](https://github.com/user-attachments/assets/50f2880c-829f-4054-903c-84a0ad7ce2d6)

```
hydra -l <username> -P <wordlist> MACHINE_IP http-post-form "/:username=^USER^&password=^PASS^:F=incorrect" -V
```

- La login page es /
- usuario es username
- especificamos usuario USER
- La contraseña esta en el archivo que se usara
- Remplaza la pass por la real
- F=incorrect para volver a intentar cuando no sea ese.

