# Multi Factor Authentificator

# Objetivos

- Entender las principales operaciones de MFA y fortalecer aplicaciones en posturas de seguridad
- Explotar diferentes tipos de factores de autentificación MFA
- Ganar en escenarios prácticos conocimiento dE MFA e implementar protecciones de datos sensibles y sistemas

# Como funciona MFA

MFA añade un extra de procección a las cuentas requiriendo que haya dos o mas factores de verificación. Haciendo que el acceso sea un reto para los actores maliciosos.

Es importantes que las 2FA este bajo MFA.

## Tipos de Factores de Autentificación

MFA normalmente ocmbina dos o mas tipos de credenciales de categorias: Algo que sabes, algo que tienes, algo que eres, donde estas y algo que haces

![image](https://github.com/user-attachments/assets/fdee5651-e225-49fb-9ba9-dc3f761ef8b3)

## Tipos de 2FA

2FA utiliza varios mecanimos para asegurarse que la autentificacón es robusta en capas.

- Temporalidad de contaseñas que cambian een 30 segundos como Google Autentificator o Microsoft Authentificator.
- Aplicaciones como Google PRompt que mandan consultas de login a tu movil. Dando acceso o no directamnete al dispositivo
- Vía SMS
- Via Hardware

## Accesos condicionales

Se usa normalmente en compañias que ajustan los requerimientos de autentificacion basado en diferentes contextos.

- Basado en donde estas (localizacion)
Basado en horas regulares que te metes
Basado en cambios o acceso a datos que normalmente no usas
Dipositivo en especifico

# Implementaciones y aplicaciones

- MFA en bancos
- MFA en hospitales
- MFA en IT

# Vulnerabilidades comunes

## Algoritmos fragiles

Un algoritmo robusto, que no sea predecible, random seeds, etc.

## Aplicaciones lekeadas al token de 2FA

Endpoints inseguros en APIs, tokens, etc.

Codigo inseguro, respuestas

## Fuerza bruta al OTP

## Usar Evilginx

![image](https://github.com/user-attachments/assets/9ab03ca4-e702-4c55-a61f-92ca8d46d2d8)

# Práctica

![image](https://github.com/user-attachments/assets/40790416-e211-486f-bb37-75b8f08b9c8a)

![image](https://github.com/user-attachments/assets/b2c8aa78-3f2d-438b-a310-1c528c110797)

![image](https://github.com/user-attachments/assets/4d5431f6-0eec-4915-b8bc-0c5fd3f1c767)

![image](https://github.com/user-attachments/assets/06368c28-c1b6-4d1f-9bf7-60e8924b442f)

# Práctica - Codigo inseguro
