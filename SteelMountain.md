## Máquina de steel mountain: https://tryhackme.com/room/steelmountain 

### Empleado del mes

Este reto empieza preguntándonos quién es el empleado del mes y como hint nos dicen "reverse image search"

![image](https://user-images.githubusercontent.com/44788583/149637303-95fa8b47-d68b-4b10-9a33-d85a3caab33b.png)

No es necesario hacer reverse search a la imagen, si la descargas te sale el nombre del empleado ahí. Si eres fan de Mr. robot como yo, sabrás fácil quién es.
![image](https://user-images.githubusercontent.com/44788583/149637576-e5b0c3b1-a9f5-42ad-8024-f00d945aa170.png)

-----

### Acceso inicial

Escaneo por nmap:
- nmap 10.10.5.28 -sV -p- -n -T5 (-sV para ver los servicios , -p- puertos , -n para inhabilitar la resolución DNS y -T5 para que vaya lo más rápido posible)

![image](https://user-images.githubusercontent.com/44788583/149639672-7a066343-5d2f-4b35-8836-8bb78a01b010.png)

![image](https://user-images.githubusercontent.com/44788583/149639906-a0d7e639-e4d4-43a8-a477-3183792e4e81.png)
Vemos que el  servidor del web server está en el puerto 8080 (el otro): 
![image](https://user-images.githubusercontent.com/44788583/149639735-d03eafe1-fa6a-438e-bc56-e1bb9ac0822c.png)

El reto nos pide checar el otro servidor web y ver que file server está corriendo.
![image](https://user-images.githubusercontent.com/44788583/149642323-5a510e71-8160-4b3b-9755-cbc7e63568bc.png)
Si entramos, podemos ver que existe en la parte inferior, un cuadro que dice server information
![image](https://user-images.githubusercontent.com/44788583/149642334-a5aa2f7e-01dd-4118-aa3a-0dc22b5b90f2.png)
Por el número de letras de la respuesta, podemos ver que se trata de Rejetto HTTP File Server

----

### Buscando un CVE para explotar vulnerabilidades de Rejetto HTTP File Server
vamos a la página de https://www.exploit-db.com/ y buscamos CVE´s para Rejetto File Server 2.3
![image](https://user-images.githubusercontent.com/44788583/149642434-0f0a8527-0a46-4b69-b15f-197f60269e1d.png)
Abrimos una de ellas para buscar el CVE
![image](https://user-images.githubusercontent.com/44788583/149642570-e3137c3f-fbe7-4c81-ba8c-abbd3483baf0.png)

------

### Ganando acceso inicial con metasploit

- utilizamos: msfconsole -q para lanzar una consola con metasploit
- buscamos el exploit con el comando search hfs

![image](https://user-images.githubusercontent.com/44788583/149647575-a38e5561-022a-4e55-ae90-5b55e97db98c.png)

Usé 1 para elegir la opción 1 que es la que tiene el exploit para Rejetto HFS
- show options para ver qué parámetros se ponen en metasploit

![image](https://user-images.githubusercontent.com/44788583/149689958-8bbac4bb-61fe-401f-8956-5712bc6c96cf.png)
![image](https://user-images.githubusercontent.com/44788583/149690590-eec86396-02c6-4439-ad46-30775459dddd.png)
Se ponen los parámetros para 
- set RHOSTS 10.10.166.61
- set RPORT 8080
- run
Ahora ya tenemos una consola de metasploit corriendo el exploit y nos vamos a la carpeta C:/Users/bill/Desktop
![image](https://user-images.githubusercontent.com/44788583/149698796-86dc3fb2-429f-46fc-bcc2-10b6c47fc817.png)
Vemos que la flag está presente
- cat user.txt
![image](https://user-images.githubusercontent.com/44788583/149699085-08ee3dac-49ea-4bec-8676-84ee16f721b5.png)

### Escalando privilegios

Se usa el powershell del recurso: https://github.com/PowerShellMafia/PowerSploit en específico el archivo powershell de PowerUp.ps1
Yo lo descargué entrando a mi github 
Luego en la sesión de meterpreter se utiliza el siguiente comando: 
- upload /root/PowerSploit-master/Privesc/PowerUp.ps1. (lo guardé en esa ruta de mi máquina)
![image](https://user-images.githubusercontent.com/44788583/149704673-356cd1b8-cbd3-4d20-b195-ed7dae45edba.png)
![image](https://user-images.githubusercontent.com/44788583/149704652-4d69f6d0-d24e-40e2-a174-34b9fd154e92.png)
y luego se ejecuta: 
- load powershell
- powershell_shell
- ../PowerUp.ps1

![image](https://user-images.githubusercontent.com/44788583/149705290-5e102ecf-090c-4d3e-922d-f7a95d4318c6.png)

![image](https://user-images.githubusercontent.com/44788583/149704834-8cebd344-9b36-4ade-b2f8-2eede39c0708.png)
