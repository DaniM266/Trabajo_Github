# UD7: Política de Backup y sistemas RAID
## 1. Copias de seguridad de windows
### Diseño de la política de backup
Vamos a utilizar la siguiente estructura de ficheros para simular una serie de fallos:
````
C:\Users\Roi\Documents\:
- [d] DatosBackup
- [d] Logs
- [f] ficheroLog1.txt
- [f] ficheroLog2.txt
- [f] fichero1.txt
- [f] fichero2.txt
- [f] fichero1.pdf
- [f] otroFichero1.jpeg
- [f] otroFichero2.jpeg

`````
Para la política de bakup, creemos que lo más correcto es establecer 5 copias incrementales durante la semana, cada día una, y el fin de semana una copia completa para de esta forma poder aprovechar las horas del fin de semana para realizar la copia más pesada que es la completa.
Además de esto, hemos programado la hora de las copias para las 4 de la mañana, hora en la que no habrá nadie usando el sistema por lo que no intervendrá en ni ningún proceso y se podrá efectuar de forma más eficiente.
  
En el programa de paragon quedaría de la siguiente manera:
![image](https://github.com/DaniM266/Trabajo_Github/assets/73694734/25a40d4c-d34b-401b-81e4-5f1c8dfa8df0)


### Simulación de fallos

Para poner a prueba nuestra política de copias de seguridad haremos lo siguiente:  
1. Creamos una copia de seguridad completa simulando que es Domingo
2. El lunes modificamos erroneamente fichero1.txt fichero2.txt y creamos fichero3.txt y fichero4.txt
3. Martes creamos fichero5.txt
4. Miercoles borramos erróneamente fichero1.pdf y creamos otroFichero3.jpg y otroFicheroLog4.jpg
5. Jueves borramos erróneamente otroFichero1.jpg y creamos ficheroLog3.txt y ficheroLog4.txt
  
Ahora vamos a simular las copias de seguridad realizadas durante la semana, además de la completa del doming, lo que quedaría de la siguiente forma:

A día de viernes, contamos con 2 archivos que contienen un error no deseado (fichero1.txt y fichero2.txt) y dos ficheros borrados de forma errónea.
Para recuperar los archivos que nos interesan haremos lo siguiente:  

Comenzaremos de atrás a adelante, es decir, comenzaremos desde el viernes.

1.El viernes no hemos modificado nada y el error está en el jueves, por lo que nos interesa recuperar la copia del miércoles (suponiendo que no sabemos a que hora se ha borrado el archivo por eso no cogemos la del mismo día). Este proceso nos permitirá recuperar "otroFichero1.jpg" que había sido borrado por accidente.
2. Para recuperar el "fichero1.pdf" borrado el miércoles, nos interesará hacer el backup del día martes.
3. Finalmente, para recuperar los errores de "fichero1.txt" y "fichero2.txt" modificados erroneamente el lunes, haremos un backup de la copia completa que se realizó del domingo.
    

## 2. RAID en windows

Añadimos 2 discos a nuestra máquina virtual:
![discoduro](https://github.com/DaniM266/Trabajo_Github/assets/73694734/1a65787e-0331-4c28-a259-27d9fb53cefc)

En nuestro caso usaremos el formato VHD, ya que nos permitirá simular un disco duro en la virtual vox.

En este paso, crearemos un disco de 10Gb ya que creemos que es un tamaño bastante adecuadno para lo que vamos a hacer, una vez creado hay que añadirlo a la máquina pulsando en la opción "añadir".

![image](https://github.com/DaniM266/Trabajo_Github/assets/73694734/eea0cf0d-cab7-40a6-ae12-c52248b132de)


Seguidamente, tenemos que abrir la maquina, entrar en el gestor de discos y poner nuestro disco en linea.

Una vez realizado el paso anterior, le queremos dar formato al disco, para ello, le daremos clic derecho al disco, seleccionamos el formato que deseemos, en nuestro caso ntfs y le damos un nombre al dsico.


### Creación de un volumen distribuido.

1. Primero creamos dos volumenes simples de 1GB en cada disco, como se indica en el ejercicio:
![volumen_simple](https://github.com/DaniM266/Trabajo_Github/assets/73694734/12d69afc-2696-4cd8-93da-7187cd0c8734)


Más tarde, dejamos 1 GB libre en el primer disco y 2 en el segundo, pra dar comienzo al siguient epaso que es crear un volumen distribuido:
![espacio_libre](https://github.com/DaniM266/Trabajo_Github/assets/73694734/9e2e366a-6631-44a7-bf4e-cc0d15a791ed)




3. Click derecho en el volumen restante del disco 1, selecciona,os volumen distribuido, y le metemos al disco 1 el espacio restante del disco 2:
![image](https://github.com/DaniM266/Trabajo_Github/assets/73694734/cbe06a08-dac4-4a12-9a77-8f42a1c0ed10)


4.Le daremos el mismo formato que a los volumenes anteriores (NTFS): 

![image](https://github.com/DaniM266/Trabajo_Github/assets/73694734/98238ea9-0579-428d-886f-24b167cdbf82)




4. Así quedaría una vez finalizado el proceso:

![image](https://github.com/DaniM266/Trabajo_Github/assets/73694734/25e89634-64e4-476f-a64d-376923bfb3ac)


Para poder llevar a cabo esto, el sistema nos pide que aceptemos el cambio de los discos a dinámico: 


![image](https://github.com/DaniM266/Trabajo_Github/assets/73694734/eb58c5d6-1255-4768-9f7f-30c0d222cf3e)




### Volumen seccionado

Para esto, repetiremos los mismos pasos que antes, pero teniendo 4GB de espacio disponible en cada disco..

1. Repetiremos los mismos pasos que antes pero seleccionando la opcion de volumen seleccionado:

![image](https://github.com/DaniM266/Trabajo_Github/assets/73694734/c071d2b7-3b52-49e6-a348-77a7a3b1f587)



2. El resultado seria el siguiente:

![image](https://github.com/DaniM266/Trabajo_Github/assets/73694734/b69096cd-783c-4c2a-b9b5-797067d9c914)


Como el el tamaño del disco 2 era mayor, se nos crea un espacio con la diferencia llamado "NO".




### Volumen reflejado

Para esto, necesitamos seguir los mismos pasos que para el paso anterior, disponer de 4GB libres en cada disco, darle click derecho a esa parte y darle a volumen reflejado.

Quedaría así:


![image](https://github.com/DaniM266/Trabajo_Github/assets/73694734/7ed6f583-6740-46c8-bc16-6494bfb42289)



Este sería el resultado:

![image](https://github.com/DaniM266/Trabajo_Github/assets/167864718/4f4c6001-a2d8-4e73-bc94-6633b5a35cdf)



**3.** Realiza los siguientes apartados relativos a Sistemas RAID en Ubuntu Linux,
documentando los apartados b, c, d y f:
<br><br>
**a.** En una máquina virtual que tenga instalado Ubuntu Linux, crea tres
nuevos discos virtuales de 10 GB cada uno. Investiga sobre el uso de la
herramienta MDADM y configura lo siguiente:
<br><br>
Creacion de los discos:
En la primera imagen podemos ver como añadimos los discos a la máquina virtual.


![image](https://github.com/DaniM266/Trabajo_Github/assets/167864718/696330e0-5825-4fa9-971d-554b7c0145aa)
Mientras que en esta segunda, podemos apreciar como se ven los discos dentro del sistema, después de crear las particiones para poder realizar los raids de forma más cómoda.


![linux1](https://github.com/DaniM266/Trabajo_Github/assets/73694734/94953e26-4dad-4e8e-b634-f675afde6164)


**b.** Un RAID 0 que utilice dos discos virtuales de 10 GB cada uno.
En esta primera imagen podemos ver como creamos el raid0, con sus respectivo comando:


![raid0](https://github.com/DaniM266/Trabajo_Github/assets/73694734/e5c08323-6006-402e-a8ab-0e44017b7cc3)
En esa segunda, vemos como quedaría la información del raid, tanto su versión como su tamaño, etc.


![raid0_justi](https://github.com/DaniM266/Trabajo_Github/assets/73694734/177026c6-5d6e-4464-9258-6367e8314700)
<br><br>

**c.** Un RAID 1 que utilice dos discos virtuales de 10 GB cada uno.
En esta primera imagen podemos ver como creamos el raid1, con sus respectivos comandos:

![raid1](https://github.com/DaniM266/Trabajo_Github/assets/73694734/e2d629cf-cc98-484e-8379-c235c38feeb2)

En esa segunda, vemos como quedaría la información del raid, tanto su versión como su tamaño, etc.

![raid1_justi](https://github.com/DaniM266/Trabajo_Github/assets/73694734/a95524a6-6564-425c-bc1d-359845a3d599)

<br><br>
**d.** Un RAID 5 que utilice tres discos virtuales de 10 GB cada uno.
Finalmente, en estas capturas podemos apreciar como se crea el raid y como se vería finalmente su información.


![raid5](https://github.com/DaniM266/Trabajo_Github/assets/73694734/effd9786-8cf2-4972-876c-b3c3a5a0509c)
![raid5_justi](https://github.com/DaniM266/Trabajo_Github/assets/73694734/2705b3d2-3270-4ec4-830c-9a25281982f5)

<br><br>

Crea un esquema o tabla que describa el comportamiento que ha tenido
cada una de las unidades RAID creadas: discos necesarios, espacio
disponible, tolerancia al fallo de un disco, etc.


| Raid | Discos | Espacio | Tolerancia a los fallos |
|---|---|---|---|
| Raid 0 | Empleamos 2 discos | Se usa el 100% del espacio disponible | No nos podemos permitir el fallo de ningún disco |
| Raid 1 | Empleamos 2 discos | Se usa el 50% del espacio disponible | Podemos suplir el fallo de 1 disco |
| Raid 5 | Utilizamos 3 discos |  Se usa el 66% del espacio disponible | Podemos tolerar el fallo de 1 disco |
<br><br>

