# COMANDOS SQL

SQL (Structured Query Language)

## SELECT

Esta consulta devuelve datos de la base de datos

```
select * from users;
```
![image](https://github.com/user-attachments/assets/9084d33c-b746-4533-9525-3614937eda58)

"Select" devuelve los datos de "*" todos los datos.

```
select username,password from users;
```

![image](https://github.com/user-attachments/assets/4782ea17-2eed-42e6-b1ab-7982a2fd4778)

La siguiente consulta, devuelve las columnas pero el sector cambia.

```
select * from users LIMIT 1;
```

![image](https://github.com/user-attachments/assets/4d9c81fa-fe30-46fb-8a46-a585170c101d)

```
select * from users where username='admin';
```

![image](https://github.com/user-attachments/assets/4b0681ab-ca21-4130-a919-bd38c47b0e2e)

```
select * from users where username != 'admin';
```

![image](https://github.com/user-attachments/assets/30267faa-11f8-4ac3-b225-d933701bceb9)

```
select * from users where username='admin' or username='jon';
```
![image](https://github.com/user-attachments/assets/2f9adc92-f36e-49bf-9d1f-c2fc6efd87f1)

```
select * from users where username='admin' and password='p4ssword';
```
![image](https://github.com/user-attachments/assets/63dd00c0-0f25-40f3-99d1-9fed80e23167)

```
select * from users where username line '%n';
```
![image](https://github.com/user-attachments/assets/396827e4-92f7-4db2-9860-3c15d3cfd517)

```
select * from users where username like '%mi%';
```
![image](https://github.com/user-attachments/assets/58cf38f5-6226-462a-aad9-a59c1fac030b)

## UNION

La consulta UNION devuelve el resultado de dos o mas selecciones o de multiples tablas

Imagina que tenemos dos contenidos: 

![image](https://github.com/user-attachments/assets/fd6fec74-f780-4ec5-a2e2-c60985bfebd0)

```
select name,address,city,postcode from customerrs UNION SELECT comany,address,city,postcode from suppliers;
```

![image](https://github.com/user-attachments/assets/87317837-b17c-4938-b0eb-f38e27439861)

## INSERT

La consulta INSERT crea nuevos datos dentro de la tabla "intro users" y añadiendo valores (username,password)

```
insert into users (username,password) values ('bob','password123');
```
![image](https://github.com/user-attachments/assets/4ec4098a-5f34-4a61-9c56-ae0aeaa4221b)

## UPDATE

La consulta UPDATE le dice a la base de datos que actualice usando "update %nombretabla% SET" y añadir archivos o datos o separados "username='root',password='pass123'"

```
update users SET username='root',password='pass123' where username='admin';
```
![image](https://github.com/user-attachments/assets/3f530197-b88c-4f5f-8c2d-a21f135fa384)

## DELETE

Esta consulta elimina datos, columnas o tablas.

```
delete from users where username='martin';
```
![image](https://github.com/user-attachments/assets/15e70d74-715f-40e0-91e6-35b75c2c61d6)

```
dele from users;
```
![image](https://github.com/user-attachments/assets/3f58136c-c584-453a-ab53-291514b0899a)


# EJEMPLO

```
https://website.tmg/blog?id=1
```

De esta URL vemos que apunta "id=1" en la base de dato ssería

```
SELECT * from blog where id=1 and private=0 LIMIT 1;
```

```
https://website.thm/blog?id=2;--
```

La "--" hace que acabe laconsulta haciendo que sea un comentario

```
SELECT * from blog where id=2;--
```

# IN-BAND SQLI

```
1 UNION SELECT 1
1 UNION SELECT 1,2
1 UNION SELECT 1,2,3
0 UNION SELECT 1,2,3
```

