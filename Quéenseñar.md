# 🧩 Qué enseñar

---

## 🧱 1. Ejemplo de creación de un proceso

*(Muestra el contexto y el PCB/TCB)*

### 🔹 Explicación

Cuando un proceso se crea en un sistema operativo, el kernel:

1. Asigna una **estructura PCB (Process Control Block)**.
2. Copia el **contexto del padre** (registros, punteros, estado, memoria).
3. Cambia algunos valores (nuevo PID, estado = “Ready”).
4. Añade el proceso a la **cola de listos**.

### 🧩 Código de ejemplo (C)

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main() {
    pid_t pid = fork();  // crea un nuevo proceso

    if (pid == 0) {
        // Proceso hijo
        printf("Soy el hijo. PID = %d, PPID = %d\n", getpid(), getppid());
    } else {
        // Proceso padre
        printf("Soy el padre. PID = %d, hijo = %d\n", getpid(), pid);
    }
    return 0;
}
```

### 📊 Qué mostrar

* En la consola se imprimen dos líneas (una del padre y otra del hijo).
* Cada proceso tiene su **propio PID y contexto**.
* El PCB del hijo es una **copia** del PCB del padre, pero con valores únicos (PID, PC, SP).

🧠 Puedes acompañarlo con un **diagrama**:

```
Padre (PID 100)
 ├── PC: 0x400
 ├── SP: 0x7ff...
 └── Estado: Running
↓ fork()
Hijo (PID 101)
 ├── PC: 0x400
 ├── SP: 0x7ff...
 └── Estado: Ready
```

---

## 🔁 2. Ejemplo de cambio de contexto entre dos procesos

### 🔹 Explicación

El **cambio de contexto** ocurre cuando el CPU deja de ejecutar un proceso (A) y pasa a otro (B).
El kernel guarda los **registros, contador de programa y pila** del proceso A en su PCB, y luego **carga** los del proceso B.

### 🧩 Código de ejemplo (simulado en C)

```c
#include <stdio.h>

struct PCB {
    int pid;
    int pc;
    int sp;
    int registros[3];
};

void save_context(struct PCB *p, int pc, int sp, int r1, int r2, int r3) {
    p->pc = pc; p->sp = sp;
    p->registros[0]=r1; p->registros[1]=r2; p->registros[2]=r3;
}

void load_context(struct PCB *p) {
    printf("Cargando proceso %d (PC=%d, SP=%d)\n", p->pid, p->pc, p->sp);
}

int main() {
    struct PCB procesoA = {1, 100, 5000, {1,2,3}};
    struct PCB procesoB = {2, 200, 6000, {4,5,6}};

    printf("→ Cambio de contexto: A → B\n");
    save_context(&procesoA, 150, 5100, 7,8,9);
    load_context(&procesoB);
}
```

### 📊 Qué mostrar

* Primero se guarda el contexto del proceso A.
* Luego se carga el contexto del proceso B.
* Así el CPU “salta” de uno a otro.

💡 Visualmente puedes mostrar una **flecha**:

```
CPU ejecuta A → interrupción → guarda PCB_A → carga PCB_B → ejecuta B
```

---

## 📡 3. Ejemplo de comunicación entre procesos (IPC) usando señales

### 🔹 Explicación

Las **señales** son una forma de comunicación asíncrona entre procesos.
Un proceso puede “avisar” a otro que algo ocurrió, sin compartir memoria ni usar archivos.

### 🧩 Código de ejemplo (C)

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void handler(int sig) {
    printf("Proceso %d recibió señal %d\n", getpid(), sig);
}

int main() {
    signal(SIGUSR1, handler);  // registrar manejador

    pid_t pid = fork();
    if (pid == 0) {
        // Hijo: espera señal
        pause();
    } else {
        // Padre: envía señal al hijo
        sleep(1);
        kill(pid, SIGUSR1);
    }
    return 0;
}
```

### 📊 Qué mostrar

* El padre envía una señal `SIGUSR1` al hijo.
* El hijo **captura la señal** y ejecuta la función `handler()`.
* Es un ejemplo claro de **IPC asíncrono** (sin necesidad de compartir memoria).

🧠 Diagrama sugerido:

```
Padre (PID 100)
   |
   |-- kill(pid_hijo, SIGUSR1)
   ↓
Hijo (PID 101) → handler() → “Recibí señal 10”
```

---

## ⚠️ 4. Ejemplo de deadlock y cómo resolverlo

### 🔹 Explicación

Un **deadlock** ocurre cuando dos procesos se bloquean mutuamente esperando recursos.
Por ejemplo:

* Proceso A tiene el recurso 1 y necesita el recurso 2.
* Proceso B tiene el recurso 2 y necesita el recurso 1.

Ninguno puede avanzar → bloqueo total.

### 🧩 Código de ejemplo (C, usando hilos y mutex)

```c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>

pthread_mutex_t r1 = PTHREAD_MUTEX_INITIALIZER;
pthread_mutex_t r2 = PTHREAD_MUTEX_INITIALIZER;

void *procesoA(void *arg) {
    pthread_mutex_lock(&r1);
    sleep(1);
    pthread_mutex_lock(&r2);
    printf("Proceso A terminó\n");
    pthread_mutex_unlock(&r2);
    pthread_mutex_unlock(&r1);
}

void *procesoB(void *arg) {
    pthread_mutex_lock(&r2);
    sleep(1);
    pthread_mutex_lock(&r1);
    printf("Proceso B terminó\n");
    pthread_mutex_unlock(&r1);
    pthread_mutex_unlock(&r2);
}

int main() {
    pthread_t t1, t2;
    pthread_create(&t1, NULL, procesoA, NULL);
    pthread_create(&t2, NULL, procesoB, NULL);
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
}
```

### 📊 Qué mostrar

* Ambos hilos se quedan **bloqueados para siempre** (deadlock).
* Cada uno tiene un mutex y espera al otro.

🧠 **Solución posible:**
Establecer un **orden global** para adquirir los recursos:

```c
pthread_mutex_lock(&r1);
pthread_mutex_lock(&r2);
```

Ambos procesos deben pedir los recursos **en el mismo orden**, evitando el ciclo de espera circular.

📈 Diagrama:

```
Sin orden → A espera B y B espera A ❌
Con orden → Ambos piden r1 antes que r2 ✅
```

---

## 🧩 Conclusión de la sección

> Con estos ejemplos hemos visto cómo:
>
> * Se **crea un proceso** y se guarda su **contexto**.
> * El sistema realiza un **cambio de contexto** entre tareas.
> * Los procesos se **comunican mediante señales**.
> * Y cómo pueden **bloquearse mutuamente (deadlock)** si no se gestionan bien los recursos.
>
> Estos mecanismos son la base de la **multitarea y la sincronización** en cualquier sistema operativo moderno.
