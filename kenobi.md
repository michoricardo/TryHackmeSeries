https://tryhackme.com/room/kenobi

## Checar los puertos abiertos y checar los samba shares

- nmap 10.10.56.240 -vvv   //para ver los puertos abiertos del servidor
- //samba se usa para acceder a recursos, printers, etc en internet o intranet
- //se puede usar un script de nmap para enumerar los samba shares
- nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.56.240
- //se encuentran 3 ( IPC Service, hidden y printer) con acceso de read/write, read/write, none

----

## Entro a los shares 

- smbclient //10.10.221.228/anonymous
- smbget -R smb://10.10.221.228/anonymous para bajar el contenido directo
- cat log.txt para ver que trae el log file
![image](https://user-images.githubusercontent.com/44788583/148824837-9c9a2abc-9706-41e5-aa1f-d50df520dafb.png)
FTP en puerto 22

----

## Enumerar puerto 111 rpcbind

- nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.226.148

![image](https://user-images.githubusercontent.com/44788583/149047664-f113a09e-5c64-4aa7-9aa1-68ac71587920.png)

------

## Me conecto por netcat al puerto 21 que es el default de FTP , Pro FTPd es un servidor FTP open source 

![image](https://user-images.githubusercontent.com/44788583/149048118-22910ea9-87d1-49e9-8ac2-3d7f392c6bac.png)
 Ahí se puede ver que la versión es 1.3.5 
 
 - Ahora se buscan los exploits como si fuera xploit db con el siguiente comando: searchsploit proftpd 1.3.5

searchsploit proftpd 1.3.5

![image](https://user-images.githubusercontent.com/44788583/149048266-167fb787-38f1-4cf7-99c3-745367d40c55.png)

![image](https://user-images.githubusercontent.com/44788583/149048768-a7280eb2-111c-4eb8-80c7-91a96366ffc7.png)

Se encuentran 4 posibles xploits. 3 de 1.3.5 específico y 1 de 1.3.x
