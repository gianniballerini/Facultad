# Sistemas Operativos 

## Segundo Parcial Teórico Repaso
1. Los KLT, en un ambiente multiprocesador, pueden ejecutarse en distintos procesadores.

***
1. En multiprocesadores, en la organización maestro esclavo, una syscall puede ser atendida en cualquiera de los procesadores.  

		FALSO. -> Todas las syscall se redirigen a un mismo procesador (el maestro) y el resto de los procesadores (esclavos) corren procesos de usuario.
2. En multiprocesadores, si cada CPU tiene su SO es posible que una CPU este saturada y otras sin trabajo productivo.  

		VERDADERO -> al tener cada CPU su conjunto de procesos, se produce un desbalance de cargas de trabajo.
		3. En multiprocesadores, la técnica de SMP no requiere de exclusión mutua para el acceso a las estructuras del kernel.  

		FALSO -> SMP (Multiprocesadores simetricos) Hay una unica copia del SO en memoria, y cualquier CPU puede ejecutarlo. Por lo tanto, para que muchos CPU usen el mismo codigo se necesita exclusión mutua entre ellas para no generar conflictos.
		4. En multiprocesadores, el uso de cache de CPU para la operación TSL soluciona problema de contención del bus.  

		FALSO -> TSL (test set lock) es un sistema para garantizar exclusion mutua, prueban y establecen el bloqueo. El problema son los multiprocesadores que pueden llegar a obtener acceso al CPU al mismo tiempo.
		Para tratar de solucionar se bloquea el acceso al bus, lo cual necesita apoyo del hardware y a su vez desperdicia tiempo de CPU.
		Para solcionar esto se pueden utilizar caches para evitar el uso del BUS, pero esto genera mas problemas manteniendo la consistencia de los datos (lo cual requiere al fin y al cabo uso de BUS).
		5. No existen diferencias en la planificación de procesos entre SO monoprocesadores y multiprocesadores

		FALSO -> La planificacion tiene que ser adaptada para optimizar la utilizacion en multiples procesadores en vez de uno solo.
		6. En un sistema distribuido, todos los SO de las diferentes computadoras que participan deben ser iguales.

		FALSO -> Al ser distribuidos, no es necesario que los SO sean los mismos pero si hace falta que compartan algun protocolo de comunicacion, de manera que puedan entenderse entre ellas.
		7. En las multicomputadoras, cada CPU tiene su memoria.

		VERDADERO -> Una multicomputadora, son varios pares CPU-memoria interconectados pasandose mensajes.
		8. En multicomputadoras, la comunicación entre procesos se realiza por:
  
		a. Memoria Compartida X -> ! no tienen memoria compartida
 		b. Pasajes de Mensajes O -> Correcta.
 		c. RPC  X
 		d. Ninguna X
 		 		9. En multicomputadoras, cada nodo puede correr un SO diferente.

		VERDADERO. -> se maneja por mensajes, solo necesita que el protocolo se respete entre las diferentes computadoras.
		10. Las computadoras que forman una Grid deben ser todas iguales

		DUDOSO -> En el power no menciona Grids en ningun lado, y hay muchos tipos de computadoras conectadas en forma de grid. Deberia ser mas especifico para saber a que se refiere.
		
		Segun Wiki:
		Grid computing is the collection of computer resources from multiple locations to reach a common goal. The grid can be thought of as a distributed system with non-interactive workloads that involve a large number of files. Grid computing is distinguished from conventional high performance computing systems such as cluster computing in that grid computers have each node set to perform a different task/application. Grid computers also tend to be more heterogeneous and geographically dispersed (thus not physically coupled) than cluster computers.[1] Although a single grid can be dedicated to a particular application, commonly a grid is used for a variety of purposes. Grids are often constructed with general-purpose grid middleware software libraries. 
		11. El middleware es una capa de software entre el Hardware y el Sistema Operativo

		DUDOSO -> Yo diria que si, el nombre te dice que es una capa intermedia entre 2 cosas. Pero otra vez, en las diapositivas no se menciona ni una sola vez la palabra middleware.
		
		Segun Wiki:
		"The software layer that liesbetween the operating system and applications on each side of a distributed computing system in a network."
		12. El formato ELF permite trabajar con linking dinámico.
		
		VERDADERO -> ELF (Formato de Linkeo y ejecutable).
		Segun lo que dice el documento:
		ELF (Executable and Linking Format). There are three main types of object files.
					• A relocatable file holds code and data suitable for linking with other object files to create an executable or a shared object file.
						• An executable file holds a program suitable for execution.
						• A shared object file holds code and data suitable for linking in two contexts. First, the link editor may process it with other relocatable and shared object files to create another object file. Second, the dynamic linker combines it with an executable file and other shared objects to create a process image.
		13. En el formato ELF, los segmentos indican “como” ejecutar un programa.

		VERADERO
		14. En el formato ELF, un segmento puede contener varias secciones.

		VERDADERO -> An object file segment contains one or more sections.
		15. En el formato ELF, las secciones tienen un flag que indica si la misma es de lectura o escritura.

		DUDOSO -> Las secciones TIENEN un FLAG, y el mismo puede indicar varias cosas. Entre ellas hay una que se llama WRITE, que especifica:
		"SHF_WRITE The section contains data that should be writable during process execution."
		16. En el formato ELF, un símbolo solo representa el nombre de una variable.

		FALSO -> Nos permite relacionar la direccion de memoria con el valor del programa.
		17. En el formato PE, existe una sección que indica que símbolos de otros archivos PE son utilizados.

		VERDADERO -> La seccion de exports, contiene informacion de codigos o datos que pueden ser exportados desde el archivo. Se tratan de nombres de variables o funciones que pueden ser referenciados desde otros PE Los elementos que se exportan se denominan 'simbolos'.
		18. El formato PE permite incluir imágenes dentro del archivo.

		DUDOSO -> No encontre nada que diga que esto sea verdadero asi que lo tomo por falso.
		19. La cantidad de secciones del formato PE está limitada.

		VERDADERO -> El campo para indicar la cantidad de secciones de 2bytes y el maximo numero de secciones es de 96.
		20. Comúnmente la sección .text del formato PE almacena nombres de símbolos.

		FALSO -> segun wikipedia: "...the .text section (which holds program code)..."
		21. Un dominio define el conjunto de objetos a proteger.

		VERDADERO -> Un dominio es un conjunto de pares {Objeto, Derecho} cada par especifica un objeto y un subconjunto de operaciones que se pueden realizar con el.
		22. Dentro de un sistema existen diferentes dominios.

		VERDADERO -> hay muchos dominios dentro de los cuales se ejecutan los procesos.
		23. Un objeto puede estar en distintos dominios con derechos diferentes en cada uno. 

		VERDADERO
		
24. El derecho copy se asocia a un objeto, y permite que un proceso en ese dominio pueda copiar los derechos de ese objeto dentro de su columna.

		DUDOSO -> en principio la parte de que 'permite que un proceso en ese dominio pueda copiar los derechos de ese objeto dentro de su columna' es verdad. Pero en la diapo aparece como que el derecho copy se asocia a un elemento access (i,j) de la matriz, y no 'a un objeto'.
		25. Switch es una operación entre dominios.

		VERDADERO -> Para poder cambiar de un dominio a otro se debe habilitar la operación switch sobre un objeto.
		26. El derecho de copia se puede transferir.

		VERDADERO -> Transferencia: si un derecho se copia desde matriz(i,j) a matriz(k,j), el derecho desaparece para matriz(i,j), o sea, el derecho fue transferido.
		27. Con el derecho owner, un proceso en ese dominio puede agregar o borrar cualquier entrada dentro de su columna.

		CREO QUE ES FALSO -> Si matriz(i,j) incluye el derecho de owner entonces un proceso ejecutándose en el dominio Di puede agregar y borrar cualquier entrada en la columna j.
		28. Control es aplicable sólo a dominios.

		VERDADERO
		
29. Un proceso en el dominio i puede remover cualquier derecho del j, si tiene habilitada la operación control en su dominio.

		DUDOSO -> es VERDADERO si la matriz es (i,j). porque: "Si matriz (i,j) incluye el derecho de control, entonces un proceso ejecutándose en el dominio Di puede remover cualquier derecho de acceso dentro de la fila j." Si la matriz no es asi, entonces es FALSO.
		30. Cada objeto puede implementarse como una Lista de acceso.

		FALSO -> La matriz de acceso puede implementarse como una lista de acceso a objetos, para optimizarla.
		31. Primero se crean los mecanismos, luego se definen las políticas.

		FALSO -> Primero las politicas, luego los mecanismos.
		32. La privacidad es el derecho de cada individuo a proteger su información.

		VERDADERO (supongo, no lo encontre en las diapos)33. La privacidad y la confidencialidad son lo mismo.

		CREO QUE FALSO -> yo diría qeu la privacidad es lo anterior, y lo confidencial es informacion sensible, que por alguna razon no debe ser expuesta al publico general y debe ser protegida de alguna manera.
		34. La identificación y la autenticación son sinónimos.

		FALSO -> Identificarse hace referencia a decir quien somos, y autenticarse hace referencia a demostrar que somos quien decimos ser.
		35. La interrupción de un servicio es una amenaza a la disponibilidad.

		VERDADERO
		36. La modificación es una amenaza a la confidencialidad.

		FALSO -> es una amenaza a la Integridad de los datos.
		37. La invención es una amenaza a la integridad.

		FALSO -> igual creo que se equivocaron de palabra diciendo 'invención'
		38. El desbordamiento de buffer se puede prevenir declarando ese buffer como de sololectura.

		NI IDEA. Si declaras un buffer como solo lectura, como lo usas como buffer??
		39. ¿Cuál es el problema de permitir ejecución en una pila?

		Se puede sobreescribir la dirección de retorno de printf y saltar donde quiera. 
		Si el programa se ejecuta con SETUID root, se podría saltar a un Shell code que se ejecute con esos privilegios.