Task 7 day 2: 
Para buscar el archivo que se robaron de santa, filtramos un grep para el status 200 en el archivo de log

grep "200 " webserver.log

<img width="1162" alt="image" src="https://user-images.githubusercontent.com/44788583/206889577-0ad6196b-8da1-4cb1-a401-842d73a27ecd.png">
<img width="1183" alt="image" src="https://user-images.githubusercontent.com/44788583/206889629-4d483dca-b1d5-4886-910f-6f8bcce0a2f5.png">

para hacer una búsqueda recursiva buscando el "THM" , podemos hacer grep igual

grep "THM" -r 
<img width="1171" alt="image" src="https://user-images.githubusercontent.com/44788583/206889660-82d29bd4-99c7-4d5c-87f0-f87f95ffcb73.png">

-----

Task 8 day 3: 
<img width="858" alt="image" src="https://user-images.githubusercontent.com/44788583/206891284-7bfbdfa3-4e19-483b-bdd6-53cd88851134.png">
 
-----

Task 11 day 6: 


<img width="652" alt="image" src="https://user-images.githubusercontent.com/44788583/206926239-a865c995-0e23-46fe-8ddf-e4c1ce0681ff.png">

<img width="658" alt="image" src="https://user-images.githubusercontent.com/44788583/206926254-a877d86d-14c8-43d3-a00b-137b085f34d8.png">


<img width="1198" alt="image" src="https://user-images.githubusercontent.com/44788583/206953661-9cd2177c-b87b-4d02-b0d6-41553a8515be.png">

 
<img width="1252" alt="image" src="https://user-images.githubusercontent.com/44788583/206953821-fcbec3ac-6d11-4233-bc7b-bcf8abf56a15.png">

------

Task 15 day 10:
![image](https://user-images.githubusercontent.com/44788583/208166351-7d93e47a-9d26-4a58-9e69-df9856e58142.png)


----

Task 16 day 11: 
![image](https://user-images.githubusercontent.com/44788583/208493289-693b8552-690e-4d72-8832-963904dc9361.png)





------

Task 16 day 11: 

<img width="1251" alt="Captura de Pantalla 2022-12-18 a la(s) 17 38 43" src="https://user-images.githubusercontent.com/44788583/208325589-503d184c-c526-4fdf-9d8b-d729165ff2e5.png">
![image](https://user-images.githubusercontent.com/44788583/208496208-49ee9057-4ad6-449b-a635-4e23a19bf9fb.png)
![image](https://user-images.githubusercontent.com/44788583/208498687-2e682b5e-d54f-480b-87fb-aace8ea2bbc2.png)
![image](https://user-images.githubusercontent.com/44788583/208498790-38848726-c6c1-45ad-96b3-5e5813d9e504.png)
![image](https://user-images.githubusercontent.com/44788583/208499588-6e7b7402-8ffb-467d-8064-c7e67863a0e5.png)

Del procmon, eliminamos: 
RegOpenKey
RegQueryValue
RegQueryKey
RegCloseKey

Nos enfocaremos en RegCreateKey y RegSetValue

![image](https://user-images.githubusercontent.com/44788583/208501178-23dd5720-3b7a-4c3e-81ae-762d4459a39d.png)



