# Enumerar Active Directory

Una vez que tenemos las credenciales y la forma de autentificarnos dentro de la misma red, se nos abre un nuevo mundo de posiblidades que se nos abren! Podemos enumerar varios detalles de AD y su estructura y como funciona el acceso para autentificarse con pocos privilegios.

Durante un analisis de red team, es normal que se utilice una escala de privilegios o movimiento lateral ganando acceso adicional con privilegios suficientes para ejecutar nuestras herramientas.

# Objetivos de aprendizaje

- Snap-ins AD de Microsoft Management Console
- Los comandos Net del Command Prompt
- Los AD-RSAT cmdlest de PowerShell
- Bloodhound

# Meterse dentro de un dominio por comandos

```
systemd-resolve --interface enumad --set-dns $THMDCIP --set-domain za.tryhackme.com
```

# Injección de credenciales

