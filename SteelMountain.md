## Máquina de steel mountain: https://tryhackme.com/room/steelmountain 

### Empleado del mes

Este reto empieza preguntándonos quién es el empleado del mes y como hint nos dicen "reverse image search"

![image](https://user-images.githubusercontent.com/44788583/149637303-95fa8b47-d68b-4b10-9a33-d85a3caab33b.png)

No es necesario hacer reverse search a la imagen, si la descargas te sale el nombre del empleado ahí. Si eres fan de Mr. robot como yo, sabrás fácil quién es.
![image](https://user-images.githubusercontent.com/44788583/149637576-e5b0c3b1-a9f5-42ad-8024-f00d945aa170.png)

### Acceso inicial

Escaneo por nmap:
- nmap 10.10.5.28 -sV -p- -n -T5 (-sV para ver los servicios , -p- puertos , -n para inhabilitar la resolución DNS y -T5 para que vaya lo más rápido posible)

![image](https://user-images.githubusercontent.com/44788583/149639672-7a066343-5d2f-4b35-8836-8bb78a01b010.png)

![image](https://user-images.githubusercontent.com/44788583/149639906-a0d7e639-e4d4-43a8-a477-3183792e4e81.png)
Vemos que el  servidor del web server está en el puerto 8080 (el otro): 
![image](https://user-images.githubusercontent.com/44788583/149639735-d03eafe1-fa6a-438e-bc56-e1bb9ac0822c.png)
