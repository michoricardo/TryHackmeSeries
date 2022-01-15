https://tryhackme.com/room/kenobi

TODAS LAS IPS QUE SE VEN AQUÍ CAMBIAN...

## Checar los puertos abiertos y checar los samba shares

- nmap 10.10.56.240 -vvv   //para ver los puertos abiertos del servidor
- //samba se usa para acceder a recursos, printers, etc en internet o intranet
- //se puede usar un script de nmap para enumerar los samba shares
- nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.56.240
- //se encuentran 3 ( IPC Service, hidden y printer) con acceso de read/write, read/write, none

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
FTP en puerto 22

----


