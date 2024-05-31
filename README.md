# UD7: Política de Backup y sistemas RAID
## 1. Estructura de ficheros y política de **Backup** 
### Diseño política
Preparamos la siguiente estructura de ficheros que usaremos para hacer pruebas con el programa **Paragon Backup Community Edition**  
````
C:\Users\grupo4\Documents:
- [d] DatosBackup
    - [f] ficheroLog1.txt   
    - [f] ficheroLog2.txt
- [f] fichero1.pdf
- [f] fichero1.txt
- [f] fichero2.txt
- [f] otroFichero1.jpeg
- [f] otroFichero2.jpeg
`````
Una vez establecido el sistema de ficheros que usaremos pasamos a diseñar la política de copias de seguridad **diarias**. Hemos decido usar un ciclo de backups de **1 full + 6 increments**. Se harán todos los días a las 5:30 
hora española empezando el día 26/05/2024. Hemos decidido usar este sistema de copias de seguridad ya que así gestionamos el espacio de la mejor manera, todos los domingos se hará una copia de seguridad completa mientras que el resto de días de la semana
crearán una copia de seguridad incremental, lo que quiere decir que solo harán copia de lo que haya sido modificado desde la última copia de seguridad incremental o, en su defecto, completa.
  
Foto de como queda en el programa:  
![backup](https://github.com/DaniM266/Trabajo_Github/assets/166503414/1ddd83a9-f376-4e67-8c1e-ad011301557a)

### Pruebas

Para poner a prueba nuestra política de copias de seguridad haremos lo siguiente:  
1. Creamos una copia de seguridad completa simulando que es Domingo
2. El lunes modificamos erroneamente fichero1.txt fichero2.txt y creamos fichero3.txt y fichero4.txt
3. Martes creamos fichero5.txt
4. Miercoles borramos erróneamente fichero1.pdf y creamos otroFichero3.jpg y otroFicheroLog4.jpg
5. Jueves borramos erróneamente otroFichero1.jpg y creamos ficheroLog3.txt y ficheroLog4.txt
  
Cuando llegamos el viernes para solucionar los problemas nos encontramos con las siguientes copias de seguridad hechas:
![8](https://github.com/DaniM266/Trabajo_Github/assets/166503414/99134f8d-448b-45dd-9eaf-4059b2cc9444)

En este punto contamos con la siguiente estructura de ficheros:
````
C:\Users\grupo4\Documents:
- [d] DatosBackup
    - [f] ficheroLog1.txt   
    - [f] ficheroLog2.txt
    - [f] ficheroLog3.txt   
    - [f] ficheroLog4.txt
- [f] fichero1.txt [ERROR]
- [f] fichero2.txt [ERROR]
- [f] fichero3.pdf
- [f] fichero4.txt
- [f] fichero5.txt
- [f] otroFichero2.jpeg
- [f] otroFichero3.jpeg
- [f] otroFichero4.jpeg
````

Para restaurar esto haremos lo siguiente:  

1. Al no haber hecho cambios aún el viernes no tendremos que restaurar la copia de seguridad de jueves ya que es la misma que la que tenemos. Restauramos la copia de seguridad del miercoles. En este proceso
recuperamos el fichero otroFichero1.jpg que eliminamos por error el jueves.
````
C:\Users\grupo4\Documents:
- [d] DatosBackup
    - [f] ficheroLog1.txt
    - [f] ficheroLog2.txt    
    - [f] ficheroLog3.txt   
    - [f] ficheroLog4.txt
- [f] fichero1.txt [ERROR]
- [f] fichero2.txt [ERROR]
- [f] fichero3.pdf
- [f] fichero4.txt
- [f] fichero5.txt
- [f] otroFichero1.jpeg
- [f] otroFichero2.jpeg
- [f] otroFichero3.jpeg
- [f] otroFichero4.jpeg
````
2. La restauración del martes es redundante ya que, el fichero5.txt lo tenemos ya y el fichero1.pdf lo podemos recuperar con la copia de seguridad del lunes. Restauramos la copia del lunes y recuperaremos el fichero1.pdf
````
C:\Users\grupo4\Documents:
- [d] DatosBackup
    - [f] ficheroLog1.txt
    - [f] ficheroLog2.txt    
    - [f] ficheroLog3.txt   
    - [f] ficheroLog4.txt
- [f] fichero1.pdf
- [f] fichero1.txt [ERROR]
- [f] fichero2.txt [ERROR]
- [f] fichero3.pdf
- [f] fichero4.txt
- [f] fichero5.txt
- [f] otroFichero1.jpeg
- [f] otroFichero2.jpeg
- [f] otroFichero3.jpeg
- [f] otroFichero4.jpeg
````
3. Por último restauramos la copia de seguridad completa para volver a tener los archivos fichero1.txt y fichero2.txt sin errores.
````
C:\Users\grupo4\Documents:
- [d] DatosBackup
    - [f] ficheroLog1.txt
    - [f] ficheroLog2.txt  
    - [f] ficheroLog3.txt   
    - [f] ficheroLog4.txt
- [f] fichero1.pdf
- [f] fichero1.txt
- [f] fichero2.txt
- [f] fichero3.pdf
- [f] fichero4.txt
- [f] fichero5.txt
- [f] otroFichero1.jpeg
- [f] otroFichero2.jpeg
- [f] otroFichero3.jpeg
- [f] otroFichero4.jpeg
````   

## 2. Sistema RAID Windows

1. El primer paso sería añadirle un nuevo disco virtual a nuestra máquina con Windows 11
![discoduro](https://github.com/DaniM266/Trabajo_Github/assets/73694734/1a65787e-0331-4c28-a259-27d9fb53cefc)

En nuestro caso usaremos el formato VHD, ya que nos permitirá simular un disco duro en la virtual vox.

2.En eeste paso, crearemos un disco de 10Gb ya que creemos que es un tamaño bastante adecuadno para lo que vamos a hacer, una vez creado hay que añadirlo a la máquina pulsando en la opción "añadir".

![image](https://github.com/DaniM266/Trabajo_Github/assets/73694734/eea0cf0d-cab7-40a6-ae12-c52248b132de)


3. Seguidamente, tenemos que abrir la maquina, entrar en el gestor de discos y poner nuestro disco en linea.

4. Una vez realizado el paso anterior, le queremos dar formato al disco, para ello, le daremos clic derecho al disco, seleccionamos el formato que deseemos, en nuestro caso ntfs y le damos un nombre al dsico.


### Creación de un volumen distribuido.

1. Para la creación de este volumen, reduciremos el tamaño de nuestro disco principal en 1024 MB y de nuestro disco virtual 2048 MB (ambos sin formato).

![image](https://github.com/DaniM266/Trabajo_Github/assets/167864718/c7a66e6c-50cd-4146-b51a-8abef2c22c28)


2. Al volumen al que le hayamos dado clic para iniciar el proceso de creación del volumen distribuido, le añadiremos el volumen restante, en nuestro caso, al de 1 GB le añadiremos el de 2 GB.


3. Al igual que hicimos con el volumen simple, le daremos un formato (en este caso y en los siguientes NTFS) y un nombre para el volumen.

![image](https://github.com/DaniM266/Trabajo_Github/assets/167864718/d95f14a7-3ca8-4fc6-a819-f581c8a370a9)


4. Así es como quedaría una vez finalizado, nuestro particionado con volúmenes distribuidos.

![image](https://github.com/DaniM266/Trabajo_Github/assets/167864718/a9843d21-2cc6-45c4-8c91-39739adb2474)



### Creación de un volumen seccionado

Para la creación de este volumen sería exactamente lo mismo que en el anterior caso, simplemente los cambios son, que los discos tienen ambos 4 GB.

1. Clic derecho en el disco que le queramos crear el tipo de volumen seccionado.

![image](https://github.com/DaniM266/Trabajo_Github/assets/167864718/a3b8f7e2-470c-4b93-917f-f042d3ba106a)


2. Le damos formato y nombre al volumen y finalmente nuestro volumen seccionado aparecería de la siguiente manera.

![image](https://github.com/DaniM266/Trabajo_Github/assets/167864718/a5bf218c-032c-4e38-b291-162dd714144b)



### Creación de un volumen reflejado

Para este volumen necesitaremos otras dos secciones de disco (una por cada uno) de 4 GB de almacenamiento, sin formato.

1. El primer paso sería darle clic derecho sobre uno de los discos y seleccionar la opción de "Nuevo volumen refeljado".

2. El siguiente paso sería darle formato y nombre al volumen, como hicimos en los anteriores dos apartados.

La información del volumen sería la siguiente.

![image](https://github.com/DaniM266/Trabajo_Github/assets/167864718/fdf12c9c-fbc0-4642-9c42-56508173550b)


3. Y finalmente así es como quedaría el gestor de discos con estes volúmenes reflejados.

![image](https://github.com/DaniM266/Trabajo_Github/assets/167864718/4f4c6001-a2d8-4e73-bc94-6633b5a35cdf)


````

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
**e.** Una vez creada cada unidad RAID, copia algunos ficheros dentro de ella,
desconecta uno de los discos virtuales que la forman y comprueba qué ha
pasado con esos datos.
<br><br>
**f.** Crea un esquema o tabla que describa el comportamiento que ha tenido
cada una de las unidades RAID creadas: discos necesarios, espacio
disponible, tolerancia al fallo de un disco, etc.
<br><br>

