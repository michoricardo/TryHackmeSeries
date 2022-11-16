## Escalamiento de privilegios rootme, dificultad fácil en tryhackme

Después de deployear la máquina vulnerable a escalamiento de privilegios: 

# Reconocimiento: 

- Escanear los puertos y servicios corriendo en la máquina: 
Se puede utilizar el siguiente cheatsheet: https://www.stationx.net/nmap-cheat-sheet/

Corremos el comando nmap $IP -sV -T5 -n para donde sV nos trae la información de los servicios corriendo en cierto puerto, T5 para acelerar el escaneo al máximo y -n para evitar la resolución DNS y así hacer aún más rápido el escaneo.

<img width="676" alt="image" src="https://user-images.githubusercontent.com/44788583/201807952-c96f008a-6185-4ca9-b727-b3e6a48cb916.png">

Vemos 2 puertos abiertos y en uno de ellos vemos apache 2.4.29 corriendo en el puerto 80 y SSH corriendo en el puerto 22


Para enumeración de directorios, utilizamos una herramienta llamada gobuster, utilizando el siguiente comando, donde se pasa un wordlist con paths comunes: 
gobuster dir -u $IP -w /usr/share/wordlists/dirb/common.txt

<img width="885" alt="image" src="https://user-images.githubusercontent.com/44788583/201809123-0255d2a3-0319-477c-bfc3-90d5f69a11ef.png">

Un path que llama la atención es el de /panel


accedemos a ese path por medio de $IP/panel

<img width="702" alt="image" src="https://user-images.githubusercontent.com/44788583/202015350-cb0054ad-010c-4fa6-9043-3dd28d77fb21.png">


usar https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php para tener un código de shell de reversa en php

<img width="704" alt="image" src="https://user-images.githubusercontent.com/44788583/202014774-992a3686-6df9-44a8-9033-abc58188e437.png">

crear un archivo para poder utilizarlo (puede ser con nano) y se sube a la página como extensión .php5

<img width="519" alt="image" src="https://user-images.githubusercontent.com/44788583/202015553-a623122a-afeb-4c0a-9825-d9ca52d9ece4.png">
 
para verificar que se subió, ir a: $IP/uploads
<img width="524" alt="image" src="https://user-images.githubusercontent.com/44788583/202016289-0aeebbf6-f2f4-4e85-937e-d0f609f72a9f.png">

Ahora para comunicarse, debemos poner a la escucha un puerto por medio de netcat y a su vez hacer una llamada al archivo recién subido, cada uno en una terminal diferente: 

terminal 1: 
nc -nlvp 9999 (puerto especificado en el .php5)

terminal 2: 
curl http://$IP/uploads_reverse_shell.php5

Tenemos una shell!!

![image](https://user-images.githubusercontent.com/44788583/202302116-12ea2164-cd71-410f-acf3-9c9aef0a3474.png)


ya que tenemos una shell, la pista en tryhackme nos dice que busquemos el archivo user.txt


lo podemos buscar con el comando find ./ -name user.txt y lo buscamos en todo el servidor. 
![image](https://user-images.githubusercontent.com/44788583/202302445-b7a64194-eb1f-44a3-b8bf-829be7727863.png)

![image](https://user-images.githubusercontent.com/44788583/202302522-c50eecaa-7daa-4260-a971-3f78779c47b0.png)

Ahora que sabemos que esta en /var/www/user.txt podemor ir a ver el contenido de esa flag con cat /var/www/user.txt

![image](https://user-images.githubusercontent.com/44788583/202302777-8b935d64-8ad3-4e70-9af1-4b536681825c.png)




