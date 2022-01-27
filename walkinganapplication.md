https://tryhackme.com/room/walkinganapplication

# Este walkthrough se hará solamennte para los pasos prácticos de este room

### Viewing the page source

Debes conectarte a la ip y abrir un navegador para ver qué tiene hosteado.

![image](https://user-images.githubusercontent.com/44788583/151408848-5e3cfc70-848e-4acf-9cc4-afeb0a38bc45.png)


-----

### Flag en el comentario de HTML

Lo primero que debemos hacer es hacer click derecho en cualquier parte de la página y hacer click en "view page source"

El hint nos dice que veamos en el comentario, los comentarios no deberían tener información sobre el servidor o sus rutas.

![image](https://user-images.githubusercontent.com/44788583/151409131-3670fbb5-8bd4-4d74-bc5a-4f17b708b94c.png)

Entonces nos vamos a la ruta de new-home-beta

![image](https://user-images.githubusercontent.com/44788583/151409550-07e2caf5-da33-412c-8bbd-ea82c25b8337.png)

----

### Flag en el link secreto

Seguimos viendo comentarios o información relevannte de rutas:

![image](https://user-images.githubusercontent.com/44788583/151410502-c74e65cc-b18d-4c4a-a5d4-0b287cf3aedc.png)

![image](https://user-images.githubusercontent.com/44788583/151410604-30c14327-2db0-40a1-b4ac-0c4ee03c1bba.png)

----

### Directory listing flag

Esta flag requiere un poco más de olfato, seguimos explorando las rutas en el código fuente, esta vez exploramos los assets.

![image](https://user-images.githubusercontent.com/44788583/151411082-c2e924bd-2a32-4f67-a6b3-fbf162f5dac5.png)
Dentro de la ruta /assets:

Abrimos el archivo que se llama flag.txt y vemos el contenido
![image](https://user-images.githubusercontent.com/44788583/151411385-65b4b931-c584-4574-a352-0b544208b548.png)

----

### Framework flag

Como hint, vemos que nos dicen que chequemos el archivo zip https://10-10-230-226.p.thmlabs.com/<file.zip> - Find the file on the framework changelog page.

En lo personal, no sabía de qué se trataba el changelog page, entonces busqué algo que dijera framework en el source page:

![image](https://user-images.githubusercontent.com/44788583/151411710-eff29e27-9a93-4ae5-8d83-d688b83e5beb.png)

Entramos a la página del framework de THM: https://static-labs.tryhackme.cloud/sites/thm-web-framework/

Y nos encontramos con el mentado changelog page: 

![image](https://user-images.githubusercontent.com/44788583/151412091-be223256-617e-4a16-8002-fa9d1fb38ee8.png)

Aquí podemos ver que se deja un mensaje de que puede haber un archivo tmp.zip que podría ser leído por intrusos.


![image](https://user-images.githubusercontent.com/44788583/151412482-9821955b-e419-4c40-8840-0b54a19609fd.png)

Vamos a la ruta sugerida por el hint, pero con el archivo tmp: https://10-10-230-226.p.thmlabs.com/tmp.zip
![image](https://user-images.githubusercontent.com/44788583/151413845-d5d235fd-6dab-4089-bc38-7cd80f527664.png)
En ese archivo viene la flag
![image](https://user-images.githubusercontent.com/44788583/151413963-d060cd69-f1e6-4bc8-8fc4-b2fa99f577d5.png)

----

### Developer tools inspector

Entramos a la página de los artículos y vemos que uno está bloqueado por una clase que se llama: premium-customer-blocker
Yo estoy usando firefox, entonces si la busco, puedo cambiar sus clases facilmente, solamente inhabilitando un checkbox o su valor.

![image](https://user-images.githubusercontent.com/44788583/151417085-22a63e17-cfdd-45c8-ba4b-d786aea426a7.png)

![image](https://user-images.githubusercontent.com/44788583/151416460-2c425c6b-144b-48fd-8abd-4d54ac9cbfa8.png)

![image](https://user-images.githubusercontent.com/44788583/151417208-c2a5a2b1-7ce9-4976-81b2-77e0562fe969.png)


