# Trabajo_UD7
1. Realiza los siguientes apartados relativos a Copia de Seguridad en Windows,
documentando los apartados d y f:
<br><br>
 **a**. En un Windows 11 Pro instala el software “Paragon Backup & Recovery
Community Edition”.
![1](https://github.com/DaniM266/Trabajo_Github/assets/166503414/3aaee48a-6a52-48b7-9d24-194630a01e3c)

<br><br>
 **b**. Dedica unos minutos a familiarizarte con su interfaz e identifica cómo
puedes: crear diferentes tipos de copias de seguridad (completa,
incremental y diferencial), cómo programar la ejecución de las copias,
cómo establecer copia de seguridad para todo el sistema, cómo
establecer copia de seguridad para un conjunto de ficheros y directorios
concreto, cómo puedes restaurar los datos que contiene una copia de
seguridad, etc.
<br><br>
 **c**. Crea la siguiente estructura de ficheros y directorios en
C:\Users\tuUsuario\Documents\:
- [d] DatosBackup
- [d] Logs
- [f] ficheroLog1.txt
- [f] ficheroLog2.txt
- [f] fichero1.txt
- [f] fichero2.txt
- [f] fichero1.pdf
- [f] otroFichero1.jpeg
- [f] otroFichero2.jpeg

En las siguientes imagenes podemos ver el proceso de creacion de los directorios, así como una vista final con ellos ya creados:

 ![2](https://github.com/DaniM266/Trabajo_Github/assets/166503414/e90f276b-0c65-4716-966e-854e0b374c01)

![3](https://github.com/DaniM266/Trabajo_Github/assets/166503414/4d6f6076-b8bd-491e-a26c-ac5c383a276a)



- <br><br>
 **d**. Diseña y configura en “Paragon Backup & Recovery Community Edition” una
política de backup que te permita mantener copia de seguridad diaria
de todo el contenido del directorio “DatosBackup”, teniendo en cuenta
las recomendaciones estudiadas en clase (optimizar el espacio dedicado
a copias y reducir el tiempo entre copias). Habrá que programar la
ejecución de diferentes tipos de copia de seguridad. Indica de forma
esquemática o empleando una tabla la política de backup que has
diseñado. Justifica el por qué de cada copia y su frecuencia.

Para la política de backup hemos de tratar varios puntos:

Haremos la política de backup centrada en un usuario normal de un ordenador, que lo utiliza como método de trabajo habitual.

Realizaremos copias 3 dias a la semana  ya que es lo mas optimo para un usuario normal

| Frecuencia | Tipo de Backup | Justificación |
|---|---|---|
| Cada 2 días| Incremental | Realizamos una copia desde la última copia incremental. |
| Cada 4 días | Diferencial | Copia los cambios realizados desde la última copia completa, con esto buscamos que una vez a la semana se guarden los cambios sobre la copia completa minimizando los errores o pérdidas de los archivos originales |
| Cada 7 dias| Total | Hacemos una copia total sobreescribiendo todos los datos nuevos |

![4](https://github.com/DaniM266/Trabajo_Github/assets/166503414/220d40e3-1021-4de4-b53d-70fb7cb7af6d)

<br><br>
 **e**. Simula los siguientes escenarios:
i. El lunes se modificaron erróneamente fichero1.txt y fichero2.txt y
se crearon los ficheros fichero3.txt y fichero4.txt
ii. El martes se creó el fichero5.txt
iii. El miércoles se borro erróneamente fichero1.pdf y se crearon los
ficheros otroFichero3.jpeg y otroFichero4.jpeg
iv. El jueves se borró erróneamente otroFichero1.jpg y se crearon los
ficheros ficheroLog3.txt y ficheroLog4.txt (directorio Logs)
<br><br>
**f.** Realiza y documenta el proceso de restauración que llevarías a cabo el
viernes tras caer en la cuenta de los errores generados durante la
semana, simulados anteriormente. El proceso debe permitir recuperar
con éxito todos los ficheros modificados/borrados erróneamente.
(NOTA: En lugar de utilizar días de la semana reales, realiza las modificaciones
necesarias para cumplir cada escenario y lanza manualmente la copia de
seguridad que proceda en cada caso).

Teniendo en cuenta que creamos la politica de backup un lunes, el prodecimiento seria el siguiente:
-Al ser la copia diferencial cada 4 días, deberíamos hacer un backup de la copia total original para recuperar los archivos "originales" que hemos sobreescrito de forma errónea.
-Para el borrado erróneo de ficheros del miércoles, nos bastaría con hacer un backup de la copia incremental del día anterior, ya que la hacemos cada 2 días, lo que nos permitiría recuperar esos ficheros.
-Parra el borrado erróneo del jueves y creación de nuevos ficheros, deberíamos recurrir a la anterior copia incremental realizada el martes, por lo que recuperaríamos los archivos borrados, pero tendríamos que crear los nuevos.
<br><br>
**2**. Realiza los siguientes apartados relativos a Sistemas RAID en Windows,
documentando los apartados b, c, d, e y f:
<br><br>
**a**. En una máquina virtual que tenga instalado Windows 11 Education Pro,
crea dos nuevos discos virtuales de 10 GB cada uno. Una vez arrancado
Windows, utilizando la herramienta “Administración de discos”, inicializa
cada uno de los dos discos como “Disco Dinámico” y realiza lo siguiente:
<br><br>
**b**. Crea un volumen simple de 1 GB.
<br><br>
**c**. Crea un volumen distribuido en dos discos (1 GB y 2 GB,
respectivamente).
<br><br>
**d**. Crea un volumen seccionado formado por dos discos de 4 GB cada uno.
<br><br>
**e**. Crea un volumen reflejado de 4 GB.
<br><br>
**f**. Copia algunos ficheros dentro de cada uno de los diferentes volúmenes
creados, desconecta el segundo disco virtual e indica qué ha pasado con
esos datos (si están disponibles o no) en cada uno de los volúmenes. Crea
un esquema o tabla que describa el comportamiento que ha tenido cada
uno de los volúmenes tras la desconexión del segundo disco virtual.¿Cuál
se ha comportado como un RAID0 y cuál como un RAID1?. Justifica tus
respuestas.

<br><br>
**3.** Realiza los siguientes apartados relativos a Sistemas RAID en Ubuntu Linux,
documentando los apartados b, c, d y f:
<br><br>
**a.** En una máquina virtual que tenga instalado Ubuntu Linux, crea tres
nuevos discos virtuales de 10 GB cada uno. Investiga sobre el uso de la
herramienta MDADM y configura lo siguiente:
<br><br>
**b.** Un RAID 0 que utilice dos discos virtuales de 10 GB cada uno.
<br><br>
**c.** Un RAID 1 que utilice dos discos virtuales de 10 GB cada uno.
<br><br>
**d.** Un RAID 5 que utilice tres discos virtuales de 10 GB cada uno.
<br><br>
**e.** Una vez creada cada unidad RAID, copia algunos ficheros dentro de ella,
desconecta uno de los discos virtuales que la forman y comprueba qué ha
pasado con esos datos.
<br><br>
**f.** Crea un esquema o tabla que describa el comportamiento que ha tenido
cada una de las unidades RAID creadas: discos necesarios, espacio
disponible, tolerancia al fallo de un disco, etc.
<br><br>
