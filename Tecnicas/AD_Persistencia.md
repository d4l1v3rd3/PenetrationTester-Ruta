# Persistencia en Active Directory

## Persistencia AD

Durante hagamos un ataque en AD, necesitaremos seguramente que haya una persistnecia, en caso de que nos echen o quiten crendenciales.

### Conectarnos a la red de dominio

```
systemd-resolve --interface persistad --set-dns $ip --set-domain za.tryhackme.loc
nslookup dominio
```

