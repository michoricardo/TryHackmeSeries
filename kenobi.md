https://tryhackme.com/room/kenobi

nmap 10.10.56.240 -vvv   //para ver los puertos abiertos del servidor
//samba se usa para acceder a recursos, printers, etc en internet o intranet

//se puede usar un script de nmap para enumerar los samba shares

nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.56.240


//se encuentran 3 ( IPC Service, hidden y printer) con acceso de read/write, read/write, none




