# Evasión De Firmas

# Objetivos de aprendizaje

- Entender los origenes de las firmas y como observan/detectan código malicioso
- Implementes documentos ofuscados para romper firmas
- Aprovechar la no-ofuscación para romper non-function orientadas a firmas

 # Identificación de firmas

 Antes de meternos en las firmas, deberemos identificar y entender que son y como se ven. 

 Cuando identificamos firmas, ya sea automatico o manual, debemos saber en que byte empieza dcha firma. Recursivamente vemos el binario completo y lo testemos, nosotros estimamos el rango de bytes.

 Nosotros usamos utilizaes nativas como "head", "dd" o "split" a un binario compilado.

