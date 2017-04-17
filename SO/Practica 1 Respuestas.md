# Practica 1
### Preguntas de Repaso

1. ¿Que es la shell? ¿Para que sirve?
***
  La shell es la interfaz del usuario para acceder a los servicios del sistema operativo. En general, las shells usan interfaces de lineas de comando (CLI) o interfaces de usuario graficas (GUI), dependiendo del rol y las operaciones particulares. 
  Se la denomina shell (cascaron) porque es una capa que rodea el Kernel del Sistema Operativo.

2. ¿En que espacio (de usuario o de kernel) se ejecuta?
***
  La shell se ejecuta en el espacio del usuario.

3. Si pensamos en el funcionamiento de una shell basica podriamos detallarlo secuencialmente de la siguiente manera:
  * Esperar a que el usuario __ingrese__ un comando
  * __Procesar la entrada__ del usuario y obtener el __comando__ con sus parametros
  * Crear un __nuevo proceso__ para ejecutar el comando, iniciarlo y esperar que retorne
  * Presentar la __salida__ (de haberla) al usuario
  * Volver a empezar.
  Este tipo de comportamiento, tipico de las shell interactivas, se conoce como __RELP__ (Read-Eval-Print-Loop).
  Analice como implementaria este ciclo basico de interpretacion de scripts.
  ***
  No entiendo la pregunta. La implementacion, es tan simple como lo que esta nombrando ahi... que se yo que quiere que diga.

4. Investigue la system call fork:
  a. ¿Que es lo que realiza?
  b. ¿Que retorna?
  c. ¿Para que podrian servir los valores que retorna?
  d. ¿Por que invocaria a la misma al implementar una shell?
***
  a. Crea un nuevo proceso duplicando el proceso existente desde donde se llamo la funcion. El proceso existente de donde la funcion fue llamada se convierte en el proceso padre, y el proceso creado se convierte en el proceso hijo.

  b. Tiene un comportamiento interesante el retornar. Si el fork() fue exitoso, retorna 2 veces. Una vez retorna en el proceso hijo con el valor '0' y despues retorna en el proceso padre con el PID del proceso hijo como valor de retorno. Esto se da porque al crear el proceso hijo, se copian los segmentos de texto del padre y se continua la ejecucion desde la siguiente instruccion, que resulta ser esperar la respuesta en ambos.
  Si hay error, se retorna el valor de error.
  
  c. Para saber como hubicar al proceso hijo o comportarse frente al error ocurrido.
  
  d. Cuando se ejecuta un proceso desde una shell, la shell hace un fork() antes de ejecutar el proceso. Esto se produce asi porque cuando alguien llama a una instruccion de la familia de `exec` esto no crea un nuevo proceso, sino que reemplaza la memoria e instrucciones del proceso actual con las del proceso que se quiere ejecutar. Asi que cuando bash quiere ejecutar algo primero tiene que hacer un fork() y luego ejecutar. Si no lo hiciera asi, se ejecutaria el proceso pero no podriamos acceder mas a la tarminal bash.

5. Investigue la system call exec:
  a. ¿Para que sirve?
  b. ¿Como se comporta?
  c. ¿Cuales son sus diferentes declaraciones POSIX?
***
  a. Sirve para correr un archivo ejecutable en el contexto de un proceso ya existente, reemplazando al ejecutable previo.

  b. El comportamiento basico es conocido como __overlay__. No crea un nuevo proceso, por lo tanto el PID no cambia, pero el codigo, datos, heap, y el Stack del proceso son reemplazados por los del nuevo programa ejecutado.
  
  c. 
  ```sh
  int execl(char const *path, char const *arg0, ...);
  ```
  ```sh
  int execle(char const *path, char const *arg0, ..., char const *envp[]);
  ```
  ```sh
  int execlp(char const *file, char const *arg0, ...);
  ```
  ```sh
  int execv(char const *path, char const *argv[]);
  ```
  ```sh
  int execve(char const *path, char const *argv[], char const *envp[]);
  ```
  ```sh
  int execvp(char const *file, char const *argv[]);
  ```

6. Investigue la system call wait:
  a. ¿Para que sirve?
  b. Sin ella, ¿Que sucederia? -> pensando en la implementacion de shell.
***
  a. Sirve para bloquear el proceso llamante hasta que uno de sus procesos hijos termine o se reciba un señal. Hay dos posibilidades cuando se llama a wait():
  * Si hay procesos hijos corriendo al momento de hacer un wait(), el llamador sera bloqueado hasta que alguno de sus hijos termine. En ese momento el llamador resume su ejecucion.
  * Si no hay procesos hijos corriendo al momento de hacer un wait(), este mismo no tiene efecto. Es como si nunca se hubiese realizado la llamada al wait().

  b. Sin el wait no se podria eseprar a que una linea de comando termine de ejecutarse para poder recivir la siguiente. El orden que se detalla en el punto 3 en el que se espera a que un usuario ingrese algo no podria realizarse.

### Scripts

1. Realice un script que guarde en el archivo /tmp/usuarios los nombres de los usuarios del sistema cuyo UID sea mayour a 1000.
***
  ```sh
   while IFS=: read f1 f2 f3 f4 f5 f6 f7
   do
    if [$f3 -gt 1000]
    then
     $f1 >> /tmp/usuarios
    fi
   done < /etc/passwd
  ```
  ```
    IFS -> indica un delimitador, y guarda los resultados en las variables indicadas despues (f1,f2...etc)
  ```
2. Implemente un script que reciba como parametro el nombre de un proceso e informe cada 15 segundos cuantas instancias de ese proceso estan en ejecucion.
***
 ```sh
  while :
   pgrep $1 | wc -l
   sleep 900 
  done
 ```
 ```
   Asumo que el parametro que recibe mi script queda cargado en $1 (porque sino no se como se hace)
   pgrep -> lista el pid de los procesos que cumplan con la condicion.
   wc -> cuenta palabras. (lineas,palabras,caracteres)
 ```
3. Desarrolle un script que guarde en un arreglo todos los arcchivos del directorio actual (incluyendo sus subdirectorios) para los cuales el usuario que ejecuta el script tiene los permisos de __ejecucion__. Luego, implemente las siguientes funciones:
  a. cantidad: Imprime la cantidad de archivos que se encontraron
  b. archivos: Imprime los nombres de los archivos encontrados en orden alfabetico.
***
  ```sh
  	array=(`find . -type f -perm -111`)
  ```
  a. cantidad
  ```sh
    echo ${#array[@]}
  ```
  b. archivos
  ```sh
    IFS=$'\n' sorted=($(sort <<<"${array[*]}"))
    echo ${sorted[@]}
  ```
4. Se le ha encomendado organizar las fotos (en formato jpg) de todos los eventos de los que su empresa ha participado en el ultimo anio, los cuales se encuentran organizados en directorios con el nombre del evento. Para facilitar su busqueda posterior, los archivos deben tener nombres que sigan el siguiente patron: __EVENTO-N.jpg__, donde:
    * __EVENTO__ es el nombre del evento (el del directorio que se esta procesando)
    * __N__ es un indice de foto, comenzando en 1
    
    Realice un script que renombre los archivos de cada subdirectorio del directorio actual siguiendo lo especificado en el parrafo anterior.
__Ejemplo:__ dada la siguiente estructura de archivos y directorios:
```
 bashconf15/
  DSC1050.jpg
  DSC1051.jpg
  DSC1052.jpg
  DSC1053.jpg
 jsconf-14/
  DSC01230.jpg
  DSC01231.jpg
  DSC01232.jpg
  DSC01235.jpg
```
Se desea terminar con la siguiente estructura luego de ejecutar su script:
```
 bashconf15/
  bashconf15-1.jpg
  bashconf15-2.jpg
  bashconf15-3.jpg
  bashconf15-4.jpg
 jsconf-14/
  jsconf-14-1.jpg
  jsconf-14-2.jpg
  jsconf-14-3.jpg
  jsconf-14-4.jpg
```
***
5. Escriba un script que liste en orden alfabetico inverso el contenido del directorio actual. Es decir, si el contenido son los archivos:
```
 archivo_1.txt articulo.doc directorio directorio_2 script.sh
```
Se espera que el script los liste de la siguiente manera:
```
 script.sh directorio_2 directorio articulo.doc archivo.txt
```
***
6. Realice un script que copie todos los archivos del directorio home del usuario que lo ejecuta, a un subdirectorio del mismo llamado backup cambiandoles el nombre para que este en __MAYUSCULAS__. __No se deben procesar los subdirectorios del home del usuario__, unicamente los archivos ubicados directamente en este. Si el directorio backup existe al iniciar el script, el contenido del mismo debe borrarse antes de copiar los archivos.
__Ejemplo:__ si el home del usuario contiene:
```
 home/
  mi_usuario/
   so/
    practica1.pdf
    ejercicios/
     ejercicio-1.sh
     ejercicio-2.sh
   archivo1.txt
   mi-script.sh
   otro_archivo.txt
```
se espera tener lo siguiente luego de la ejecucion del script:
```
 /
  home/
   mi_usuario/
    backup/
     ARCHIVO1.TXT
     MI-SCRIPT.SH
     OTRO_ARCHIVO.TXT
    so/
     practica1.pdf
     ejercicios/
      ejercicio-1.sh
      ejercicio-2.sh
    archivo1.txt
    mi-script.sh
    otro_archivo.txt
```
***
7. Un escritor tiene organizados los capitulos de su proximo libre en distintos archivos de texto plano en un mismo directorio, y le ha solicitado ayuda para concatenar el contenido de cada uno de ellos en un unico archivo final llamado libro.txt, de modo tal que este ultimo contenga el texto de todos los otros archivos, uno luego del otro. Puede asumir que los archivos de los capitulos tienen nombres alfabeticamente ordenados: capitulo-01.txt, capitulo-02.txt,....., capitulo-48.txt, por ejemplo.
__tip:__ 
    ```sh
     man cat
    ```
***

### Redes y Sistemas Operativos

_En esta seccion se aprendera como integrar conceptos del sistema operativo, como redirecciones y pipes, con una red TCP/IP. Para ello se utilizara principialmente la herramienta netcat, tambien conocida como nc. Investigue su funcionamiento y resuelva._

1. La herramienta netcat provee una forma sencilla de establecer una conexion TCP/IP. En una terminal levante una sesion de netcar en modo servidor, que escuche en la IP 127.0.0.1 (localhost) en un puerto a eleccion. En otra terminal conectese, tambien via netcar al servidor recien levantado. Interactue y experimente con ambas terminales.
2. netcat tambien es vueno al momento de transmitir archivos sobre una red TCP/IP. Utilizando dos terminales como se hizo en el ejercicio anterior, transmita el archivo /etc/passwd desde una sesion de netcat hacia la otra. __Tip:__ recordar pipes y redirecciones.
3. Desarrolle un script que reciba en su entrada estandar una lista de hosts e imprima en su salida estandar unicamente aquellos que tienen el puerto 













