Task 7 day 2: 
Para buscar el archivo que se robaron de santa, filtramos un grep para el status 200 en el archivo de log

grep "200 " webserver.log

<img width="1162" alt="image" src="https://user-images.githubusercontent.com/44788583/206889577-0ad6196b-8da1-4cb1-a401-842d73a27ecd.png">
<img width="1183" alt="image" src="https://user-images.githubusercontent.com/44788583/206889629-4d483dca-b1d5-4886-910f-6f8bcce0a2f5.png">

para hacer una búsqueda recursiva buscando el "THM" , podemos hacer grep igual

grep "THM" -r 
<img width="1171" alt="image" src="https://user-images.githubusercontent.com/44788583/206889660-82d29bd4-99c7-4d5c-87f0-f87f95ffcb73.png">

