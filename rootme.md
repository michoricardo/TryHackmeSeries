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

crear un archivo para poder utilizarlo (puede ser con nano)

<img width="691" alt="image" src="https://user-images.githubusercontent.com/44788583/202015222-209ab6ff-6d7b-43ba-82ac-861d90464517.png">
 
