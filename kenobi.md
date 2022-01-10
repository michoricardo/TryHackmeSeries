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



