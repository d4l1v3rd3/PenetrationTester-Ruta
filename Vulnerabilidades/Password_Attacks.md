# PASSWORD ATTACKS

# INTRODUCCIÓN

- Contraseñas en perfiles
- Tecnicas de ataques de contraseñas
- Ataques de contraseñas online

## ¿Qué es una contraseña?

Bueno la contraseñas se utilizan como método de autentificación individual ya sea para acceder al ordenador o a una aplicación. 

Una colleción de contraseñas se refiere a un diccionario o "wordlist" lista de palabras. Contraseñas con baja complejidad que son fáciles y comunes mayoritariamente recoletadas por "data breaches". Por ejemplo "password, 123456, 1111"

[Contraseñas más usadas](https://en.wikipedia.org/wiki/Wikipedia:10,000_most_common_passwords)

## ¿Como de segura son las contraseñas?

Eso depende de varios factores. Las contraseñas usualmente se guardan en archivos desistema o bases de datos, y que esten seguras es esencial. En muchos casos hay contraseñas que se guardan en texto plano (Sony en 2014 por ejemplo)

Ahora las contraseñas se guardan usando sistemas y varias tecnicas con funciones hasheadas o encriptación de algoritmos para hacerlas mas seguras. Si un atacante accede al sistema, es complicado crakearlas.

# TÉCNICAS

## Crackeo contraseñas vs Adivinar contraseñas

Esta sección discutiremos el crackeo de contraseñas la terminologia y la perspectiva en ciberseguridad. Primero herramientas como "hashcat" o "John the Rippet" 

El crackeo de contraseñas son tecnicos usadas para descubrir contraseñas que estan encriptadas o haseadas para un texto plano. Los atacantes obtienen el encriptado del ordenador comprometido y una vez obtenido lo crackean.

Se considera una tecnica tradiciona. Primeramente el atacante debe ganar acceso en el ordenador y escalar privilegios. Una vez hecho debera pasarselo a su ordenador local. A la diferencia que el metodo de adivinar se puede utilizan protocolos online y servicios basados en diccionarios.

- Adivinación de contraseñas: Es una tecnica que se usa en targets online con protocolos y servicios. Se considera tiempo consumido y abre la oportunidad para generar logs para fallo por intento. Este tipo de técnica se usa para web, requiriendo hacer nuevas consultar en cada intento, siendo fácil de detectar y de que nos bloqueen.

- El crackeo de contraseñas es local.

# PERFILES DE CONTRASEÑAS

Tener una buena wordlist es critico para hacer un buen ataque. Es importante saber coomo se generar la listas de usuarios y contraseñas. Aquí aprenderemos a hacerlo y diferenciar tipos.

## CONTRASEÑAS POR DEFECTO

Antes de hacer un ataque, es muy rentable probar contraseñas por defecho, buscar en google sobre el creador del servicio o del mismo servicio, o switches, firewalls, routers, siempre te los dan con un default y mucha gente no lo cambia, siendo un sistema vulnerable.

Es una buena práctica utilizar un "admin:admin" o "admin:123456" Por ejemplo nos encontramos un Tomcat pues buscariamos sus contraseñas por defecto "tomcat:admin"

- https://cirt.net/passwords
- https://default-password.info/
- https://datarecovery.com/rd/default-passwords/

## CONTRASEÑAS DÉBILES

Los profesionales recolectan y generan contraseñas débiles combinando wordlist muuuuy grandes. Estas listas se generan en base a otras experiencias de pentesters.

- https://wiki.skullsecurity.org/index.php?title=Passwords
- [Seclist](https://github.com/danielmiessler/SecLists/tree/master/Passwords)

## CONTRASEÑAS LIKEADAS

Datos sensibles como contraseñas o hashes se venden o se dan como resultado de una brecha de seguridad. Pueden ser publicas o privadas dependiendo de la disponibilidad del contenido, los atacantes normalmente extraen las contraseñas, y dumpean los hashes.

- [Seclist_Leaked](https://github.com/danielmiessler/SecLists/tree/master/Passwords/Leaked-Databases)

## WORDLIST COMBINADAS

Imagina que queremos mas de una wordlist. Esto es muy simple: 

```
cat fichero1.txt fichero2.txt fichero3.txt > listacombinada.txt
```

Y dirás pero entonces hay duplicados, tranquilo:

```
sort listacombinada.txt | uniq -u > listalimpia.txt
```

## WORDLISTS PERSONALIZADOS

Personalizar listas de contraseñas es la mejor forma para incrementar las posibilidades de encontrar credenciales validas. Podemos crear listar personalizadas para una web o compañia, incluyendo emails o nombres de empleados, con herramientas como "Cewl" que se utiliza para extraer palabras o cosas importantes de una web.

```
cewl -w list.txt -d 5 -m5 http://www.nodo313.net
```

- -w escribe el contenido del fichero que le hemos puestro
- -m 5 busca palabras de no mas de 5 caracteres
- -d 5 el nivel que queremos de ataque el básico es el 2
- la url

Como resultado nos dara bastantes buenas wordlist con nombres localizaciones etc.

## WORDLIST POR USUARIOS

Buscar información por ejemplo de empleados enumerando es algo mu bueno.

Asumiendo por ejemplo el apellido, nombre, segundo nombre etc.

Hay una herramienta muy buena para todo esto

```
git clone https://github.com/therodri2/username_generator.git
cd username_generator
python3 username_generatoy.py -h
usage: username_generator.py [-h] -w wordlist [-u]

Python script to generate user lists for bruteforcing!

optional arguments:
  -h, --help            show this help message and exit
  -w wordlist, --wordlist wordlist
                        Specify path to the wordlist
  -u, --uppercase       Also produce uppercase permutations. Disabled by default
echo "John Smith" > usuarios.lst
python3 username_generator.py -w user.lst
usage: username_generator.py [-h] -w wordlist [-u]
john
smith
j.smith
j-smith
j_smith
j+smith
jsmith
smithjohn
```

## Técnica de espacio de teclas

Otra forma de crear wordlist es usando este tipo de tecnicas y utilizando la herramienta "crunch"

```
crunch -h
crunch 2 2 01234abcd -o crunch.txt
```
Crea una wordlist conteniendo todas las posibles combinaciones con 2 letras, incluyendo 0-4 a-d usando el argumento -o
```
cat crunch.txt
00
01
02
03
04
0a
0b
0c
0d
10
.
.
.
cb
cc
cd
d0
d1
d2
d3
d4
da
db
dc
dd
```

Esto es rentable para cuando generamos largos textos y queremos especificar combinaciones, por ejemplo si quisieramos crear caracteres minimos y maximos de 0-9 y a-f

```
crunch 8 8 01234589abcdefABCDEF -o crunch.txt
crunch 6 6 -t pass%%
```

## CUPP 

Cupp es una herramienta interactiva de python para crear wordlists, con nombre etc.

```
git clone https://github.com/Mebus/cupp.git
python 3 cupp.py
python3 cupp.py -i
python3 cupp.py -l
python3 cupp.py -a
```

# ATAQUES OFFLINE

## ATAQUES POR DICCIONARIO

EN este caso usaremos hascat y para dar un ejemplo utilizaremos el hash "f806fc5a2a0d5ba2471600758452799c"

1- Que tipo de hash es
2- Que wordlist vamos a usar o que tipo de ataque debo usar.

Para identificar el hash utilizaremos "hashid o hash-identifier" En este caso es md5

```
hashcat -a 0 -m 0 f806fc5a2a0d5ba2471600758452799c /usr/share/wordlists/rockyou.txt
```

- -a 0: Pone en modo diccionario
- -m 0: Pone el modo hash crackeo md5 para mas info "hashcat -h"

```
hashcat -a 0 -m 0 F806FC5A2A0D5BA2471600758452799C /usr/share/wordlists/rockyou.txt --show
f806fc5a2a0d5ba2471600758452799c:rockyou
```

## ATAQUES DE FUERZA BRUTA

Este ataque es el mas usado para entrar en lugares o persnoal no autorizado.

```
hashcat --help
```

```
hascat -a 3 ?d?d?d?d --stdout
```

- -a 3 : pone el modo de ataque a fuerza bruta
- ?d?d?d? : Pone un digito en cada caso empezando de 0000 a 9999
- --stdout: Nos printea el resultado en terminal

```
hashcat -a 3 -m 0 05A5CF06982BA7892ED2A6D38FE832D6 ?d?d?d?d
05a5cf06982ba7892ed2a6d38fe832d6:2021
```

## ATAQUES BASADOS EN REGLAS

Estos ataques se denominan hibridos. Se asume que el atacante tiene conocimiento de las politicas de contraseñas

Utilizaremos "haschat" o "John"

```
john --wordlist=/tmp/single-password-list.txt --rules=best64 --stdout | wc -l
```

- --wordlist= especificamos la wordlis
- --rules Especificamos las reglas a seguir
- --stdout printeamos el terminal
- | wc -l: cuenta cuantas lineas produce john





