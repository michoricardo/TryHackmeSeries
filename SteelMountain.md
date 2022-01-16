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

## Buscando un CVE para explotar vulnerabilidades de Rejetto HTTP File Server
vamos a la página de https://www.exploit-db.com/ y buscamos CVE´s para Rejetto File Server 2.3
![image](https://user-images.githubusercontent.com/44788583/149642434-0f0a8527-0a46-4b69-b15f-197f60269e1d.png)
Abrimos una de ellas para buscar el CVE
![image](https://user-images.githubusercontent.com/44788583/149642570-e3137c3f-fbe7-4c81-ba8c-abbd3483baf0.png)

