
### https://tryhackme.com/room/fileinc

## LFI #2

Se conoce que la función include es la que presenta vulnerabilidad, de hecho, si picamos el botón de include, vemos la forma que toma la URL y nos da una idea de cómo vulnerarla, como en el ejemplo anterior, que nos explicaban que una forma de leer archivos es: http://webapp.thm/index.php?lang=../../../../etc/passwd
(por ejemplo para leer etc/passwd)

pero, si tiene un filtro, entonces nos apoyaremos del null byte o del truco de /. (que significa no moverse de carpeta en linux)

<img width="640" alt="image" src="https://user-images.githubusercontent.com/44788583/158029807-7617f771-30a7-48ad-a716-9f6b12971d7b.png">
<img width="659" alt="image" src="https://user-images.githubusercontent.com/44788583/158029823-e059a415-3d14-4fae-8b56-750cadf665f9.png">

<img width="550" alt="image" src="https://user-images.githubusercontent.com/44788583/158029641-e7b8f82d-7ae3-45a8-96cc-8901cbd42f02.png">

Give Lab #3 a try to read /etc/passwd. What is the request look like?

10.10.201.20/lab3.php?file=../../../../etc/passwd%00
