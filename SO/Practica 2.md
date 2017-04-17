#Practica 2

###Conceptos teóricos

__El propósito de esta primera sección de la práctica es introducir los conceptos preliminares que necesitará el alumno, para desarrollar la actividad práctica del apartado B de la presente guía de estudio.__

1. ¿Qué es el kernel de GNU/Linux? ¿Cuáles son sus funciones principales dentro del Sistema Operativo?
***
  Segun la catedra: Es un programa que ejecuta programas y gestiona dispositivos de hardware. Es el encargado de que el software y el harware puedan trabajar en conjunto.
  Las funciones principales son Administrar la __memoria principal__ y administrar el __uso del CPU__
  Es de codigo abierto a los Usuarios _(kernel/sched.c)_
  En un sentido estricto, ellos lo ven como el Sistema Operativo.

2. Indique una breve reseña histórica acerca de la evolución del kernel de GNU/Linux
***
  A menos que vuelva a esta practica a estudiar para el final y vea que toman esto, ni a palos voy a perder tiempo completando esto. De paso, como anegdota, el flaquito que explicaba la practica se salteó todas las diapositivas que eran de historia porque dijo que no le importaban. jaja

3. Explique brevemente la arquitectura del kernel de GNU/Linux teniendo en cuenta: tipo de
kernel, módulos, portabilidad, etc.
***
  Es un nucleo monolitico hibrido. 
  Los drivers y el codigo kernel se ejecutan en modo privilegiado. 
  Lo que lo hace hibrido es la posibilidad de cargar y descargar funcionalidades atravez de modulos.
  Hay mas detalle con referencia a esto, pero en este momento no es de suprema importancia y tengo que avanzar, asi que quizas agregue mas cosas acá, dependiendo de la necesidad.
  
4. ¿Cuáles son los cambios que se introdujeron en el kernel a partir de la versión 3.0? ¿ Cuál fue la razón por la cual se cambió de la versión 2 a la 3? ¿Y la razón para el cambio de la versión 3 a la 4?
***
  If you really whant to know... La posta, es que Linus Torbal es un Troll, y se le dio por cambiar las versiones porque lo sentia asi en el fondo de su corazon. Mas o menos, tengo fotos para probarlo. (?
  [Imgur](http://i.imgur.com/acFobDm.png)
  [Imgur](http://i.imgur.com/LSF4Ru4.png)
  Tambien estan las razones posta, pero bueno, esas no son tan importantes como las ganas de joder de Linus.

5. ¿Cómo se define el versionado de los kernels de GNU/Linux?
***
  A.B.C.[D]
  A -> Denota la version. Es el que menos cambia, desde 1994 cambio solo 4 veces.
  B -> Denota una revision mayor. Es algo importante. Pero no tan drastico como un cambio de version.
  C -> Denota revision menor. Cambia cuando hay nuevos drivers o caracteristicas.
  D -> Se utiliza cuando se corrigen bugs grabes sin agragar nuevas funcionalidades.

6. ¿Cuáles son las razones por las cuáles los usuarios de GNU/Linux recompilan sus kernels?
***
  Por varias razones. Entre ellas:
  * Para soportar nuevos dispositivos, como por ejemplo una placa de video.
  * Para agregar nueva funcionalidad, o soporte para algun hardware especifico.
  * Para optimizar el funcionamiento de acuerdo al sistema en el que corre.
  * Para adaptarlo al sistema en el que corre. (quitar hardware no utilizado).
  * Para corregir bugs y asegurar mayor seguridad.

7. ¿Cuáles son las distintas opciones para realizar la configuración de opciones de compilación de un kernel? Cite diferencias, necesidades (paquetes adicionales de software que se pueden requerir), pro y contras de cada una de ellas.

8. Nombre al menos 5 opciones de las más importantes que encontrará al momento de realizar la configuración de un kernel para su posterior compilación.

9. Indique que tarea realiza cada uno de los siguientes comandos durante la tarea de configu- ración/compilación del kernel:
  a. make menuconfig 
  b. make clean
  c. make (investigue la funcionalidad del parámetro -j)
  d. make modules (utilizado en antiguos kernels, actualmente no es necesario)
  e. make modules_install 
  f. make install

10. Una vez que el kernel fue compilado, ¿dónde queda ubicada su imagen? ¿dónde debería ser
reubicada? ¿Existe algún comando que realice esta copia en forma automática?

11. ¿A qué hace referencia el archivo initramfs? ¿Cuál es su funcionalidad? ¿Bajo qué condi- ciones puede no ser necesario?

12. ¿Cuál es la razón por la que una vez compilado el nuevo kernel, es necesario reconfigurar el gestor de arranque que tengamos instalado?

13. ¿Qué es un módulo del kernel? ¿Cuáles son los comandos principales para el manejo de módulos del kernel?

14. ¿Qué es un parche del kernel? ¿Cuáles son las razones principales por las cuáles se deberían aplicar parches en el kernel? ¿A través de qué comando se realiza la aplicación de parches en el kernel?