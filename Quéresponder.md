# 🎓 Presentación: Procesos en un Sistema Operativo ASOC Semana2

**Grupo:** 5 integrantes (Raúl, Jacobo, Miguel, Javier y Daniel)
**Duración total:** 20 minutos
**Tema:** Procesos en un Sistema Operativo (SO)

---

## 👤 Persona 1 – Introducción general: Procesos e Hilos

### 📖 Lo que debe decir:

> Buenos días, nosotros vamos a hablar sobre los **procesos en un sistema operativo**, cómo se crean, se gestionan y se comunican entre sí.
>
> Yo comenzaré explicando **qué es un proceso** y la **diferencia entre proceso e hilo**.
>
> Un **proceso** es básicamente un **programa en ejecución**. Es una instancia activa de un programa que tiene su propio espacio de memoria, sus registros y sus recursos asignados.
>
> Por ejemplo, cuando abrimos dos veces el Bloc de notas, el sistema crea **dos procesos distintos**, aunque ejecuten el mismo programa.
>
> Un **hilo** o *thread*, en cambio, es una **unidad más pequeña dentro de un proceso**. Todos los hilos de un proceso comparten el mismo espacio de memoria y los mismos recursos, pero cada hilo tiene su propio contador de programa y su propia pila.
>
> La ventaja de los hilos es que permiten que un programa realice varias tareas a la vez. Por ejemplo, un navegador web puede tener un hilo para la interfaz, otro para las descargas y otro para reproducir videos.
>
> En resumen, un proceso es como una “caja” que contiene todo el entorno de ejecución, mientras que los hilos son los “trabajadores” dentro de esa caja.

---

## 👤 Persona 2 – PCB y Contexto del Proceso

### 📖 Lo que debe decir:

> Ahora que sabemos qué es un proceso, yo voy a explicar cómo el sistema operativo **guarda y controla la información de cada proceso**.
>
> Para esto se utiliza una estructura llamada **PCB**, o *Process Control Block*.
>
> El PCB contiene toda la información necesaria para que el sistema pueda **detener y reanudar** un proceso en cualquier momento.
>
> Entre los datos que guarda están:
>
> * El identificador del proceso (PID)
> * El estado (ejecutando, listo, bloqueado)
> * Los registros de la CPU
> * El contador de programa (la próxima instrucción a ejecutar)
> * Los punteros a la memoria del proceso
> * Los archivos abiertos
>
> Toda esta información forma lo que se llama el **contexto del proceso**, que es como una “fotografía” de su estado en un momento determinado.
>
> Cuando el sistema operativo cambia de un proceso a otro, guarda el contexto del proceso actual en su PCB, y luego carga el contexto del siguiente proceso.
>
> En pantalla se puede ver un ejemplo de cómo podría representarse un PCB en código C, con los campos principales.

---

## 👤 Persona 3 – Creación de procesos y cambio de contexto


### 📖 Lo que debe decir:

> Yo voy a explicar cómo se **crean los procesos** y cómo el sistema operativo realiza un **cambio de contexto** entre ellos.
>
> Cuando un sistema operativo se inicia, el kernel crea el **primer proceso**, que suele llamarse *init* (en Linux tiene el PID 1). Este proceso es el “padre” de todos los demás.
>
> A partir de ahí, los nuevos procesos se crean usando la llamada al sistema **`fork()`**.
>
> `fork()` crea una **copia exacta del proceso actual**, incluyendo su memoria, sus registros y su contexto. El proceso original es el **padre**, y la copia es el **hijo**.
>
> Después de crear el hijo, normalmente se usa la llamada **`exec()`** para reemplazar el contenido del proceso hijo con un nuevo programa.
>
> Por ejemplo, si un proceso quiere ejecutar el comando `ls`, primero hace `fork()` para crear un hijo, y luego ese hijo llama a `exec()` para cargar el binario de `ls` y ejecutarlo.
>
> En cuanto al **cambio de contexto**, esto ocurre cuando el procesador pasa de ejecutar un proceso a ejecutar otro.
>
> En un sistema con un solo núcleo (como un x86), el sistema operativo:
>
> 1. Guarda los registros y el contador de programa del proceso actual en su PCB.
> 2. Elige el siguiente proceso a ejecutar.
> 3. Carga los registros y el contador de programa del nuevo proceso desde su PCB.
> 4. Retoma la ejecución en ese punto.
>
> Este mecanismo permite que parezca que varios procesos se ejecutan al mismo tiempo, aunque en realidad el CPU va alternando entre ellos.

---

## 👤 Persona 4 – Planificación, Espera e IPC

### 📖 Lo que debe decir:

> Yo voy a hablar de **cómo el sistema operativo decide qué proceso ejecutar** y **cómo los procesos se comunican entre sí**.
>
> Uno de los algoritmos más conocidos es el **Round Robin**. En este método, cada proceso recibe un pequeño intervalo de tiempo de CPU llamado *quantum*.
>
> Cuando se le acaba el tiempo, el sistema guarda su contexto y pasa al siguiente proceso.
>
> Esto garantiza que todos tengan una oportunidad, pero también tiene problemas: si el *quantum* es muy corto, hay demasiados cambios de contexto; si es muy largo, algunos procesos pueden quedarse esperando demasiado.
>
> Cuando un proceso necesita un recurso que no está disponible, como un archivo o una entrada de usuario, pasa al estado **bloqueado** y espera hasta que el recurso esté libre.
>
> Para comunicarse entre sí, los procesos usan mecanismos de **IPC**, o *Inter Process Communication*.
>
> Los principales son:
>
> * *Pipes* o tuberías
> * *Message Queues* (colas de mensajes)
> * *Shared Memory* (memoria compartida)
> * *Signals* (señales)
>
> Por ejemplo, un proceso puede enviar una señal a otro usando la función `kill(pid, SIGUSR1)`, y el otro proceso puede capturarla con un *handler* definido con `signal(SIGUSR1, handler)`.

---

## 👤 Persona 5 – Señales, Deadlocks y Cierre

### 📖 Lo que debe decir:

> Yo voy a hablar del manejo de **señales**, de los **deadlocks** y a cerrar la exposición.
>
> Las **señales** son mensajes que el sistema operativo o un proceso pueden enviar a otro para notificarle un evento.
>
> Por ejemplo, `SIGINT` se envía cuando presionamos *Ctrl + C* para interrumpir un programa, o `SIGCHLD` cuando un proceso hijo termina.
>
> Un proceso puede definir una función para manejar una señal, llamada **manejador de señal** (*signal handler*).
>
> En el ejemplo que mostramos, si el proceso recibe `SIGINT`, ejecuta una función que muestra un mensaje en lugar de terminar.
>
> Por otro lado, un **deadlock** ocurre cuando dos o más procesos quedan bloqueados esperando recursos que tiene el otro.
>
> Por ejemplo:
>
> * El proceso 1 tiene el recurso A y necesita el B.
> * El proceso 2 tiene el recurso B y necesita el A.
>
> Ninguno puede continuar, y ambos quedan bloqueados.
>
> Para evitar esto, los sistemas operativos pueden:
>
> * Evitar el deadlock ordenando el acceso a los recursos.
> * Detectarlo y liberar procesos involucrados.
> * O prevenirlo usando mecanismos como *trylock* o límites de tiempo.
>
> En conclusión, todos estos conceptos —procesos, PCB, cambio de contexto, planificación, comunicación y manejo de recursos— son los que hacen posible que un sistema operativo ejecute muchas tareas al mismo tiempo de forma eficiente y segura.
