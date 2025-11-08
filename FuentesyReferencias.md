# 📚 Fuentes y Referencias

### 🧠 Libros y material académico clásico

1. **Silberschatz, Galvin, Gagne. *Operating System Concepts (10ª edición).***

   * Capítulos 3–5: Procesos, Hilos, Planificación y Sincronización.
   * Principal referencia académica sobre PCB, context switching, planificación y deadlocks.
   * ISBN: 978-1-119-32891-4
   * [https://www.os-book.com/](https://www.os-book.com/)

2. **Andrew S. Tanenbaum, Herbert Bos. *Modern Operating Systems (4ª edición).***

   * Capítulos 2 y 3: Procesos, comunicación entre procesos, señales y deadlocks.
   * Explica con detalle cómo funcionan `fork()` y `exec()`, así como los modelos de multitarea.
   * ISBN: 978-0133591620

3. **William Stallings. *Operating Systems: Internals and Design Principles (9ª edición).***

   * Capítulo 4: Process Description and Control.
   * Capítulo 6: Deadlocks.
   * Base para los ejemplos del PCB y cambio de contexto.

---

### 💻 Documentación técnica y man pages (Linux / POSIX)

4. **Manual de Linux (`man fork`, `man exec`, `man signal`)**

   * [https://man7.org/linux/man-pages/man2/fork.2.html](https://man7.org/linux/man-pages/man2/fork.2.html)
   * [https://man7.org/linux/man-pages/man2/exec.2.html](https://man7.org/linux/man-pages/man2/exec.2.html)
   * [https://man7.org/linux/man-pages/man7/signal.7.html](https://man7.org/linux/man-pages/man7/signal.7.html)
   * Fuente oficial para la sintaxis y comportamiento de las llamadas al sistema `fork()`, `exec()`, `kill()`, y `signal()`.

5. **POSIX Standard – IEEE Std 1003.1**

   * Sección 3: Process Management y Signal Handling.
   * Base normativa para las funciones `fork()`, `exec()`, `wait()`, `kill()`, etc.
   * [https://pubs.opengroup.org/onlinepubs/9699919799/](https://pubs.opengroup.org/onlinepubs/9699919799/)

---

### ⚙️ Repositorios y documentación de desarrollo de sistemas operativos

6. **OSDev Wiki – *Brendan’s Multitasking Tutorial***

   * Explica cómo implementar multitarea y cambio de contexto en arquitecturas x86.
   * Base para el ejemplo del cambio de contexto.
   * [https://wiki.osdev.org/Brendan%27s_Multi-tasking_Tutorial](https://wiki.osdev.org/Brendan%27s_Multi-tasking_Tutorial)

7. **GitHub: TretornESP/bec – Ejemplo de cambio de contexto**

   * Código de ejemplo con dos procesos (`p1` y `p2`) y simulación de PCB.
   * [https://github.com/TretornESP/bec/blob/main/s2/a.c](https://github.com/TretornESP/bec/blob/main/s2/a.c)

8. **GitHub: omen-osdev/omen**

   * Ejemplo real de inicialización de procesos dentro de un kernel minimalista.
   * [https://github.com/omen-osdev/omen](https://github.com/omen-osdev/omen)

---

### 🔬 Recursos educativos complementarios

9. **GeeksforGeeks – Operating System Tutorials**

   * Secciones sobre “Process Control Block”, “Process States”, “Context Switching”, “Inter Process Communication”, y “Deadlocks”.
   * [https://www.geeksforgeeks.org/operating-system/](https://www.geeksforgeeks.org/operating-system/)

10. **TutorialsPoint – Operating System Concepts**

* Guía simplificada de procesos, hilos y planificación.
* [https://www.tutorialspoint.com/operating_system/os_processes.htm](https://www.tutorialspoint.com/operating_system/os_processes.htm)

11. **The Linux Documentation Project (TLDP)**

* Guía práctica del funcionamiento de señales, procesos y planificación en Linux.
* [https://tldp.org/LDP/tlk/kernel/processes.html](https://tldp.org/LDP/tlk/kernel/processes.html)

---

### ⚠️ Ejemplos y demostraciones (Código)

* Los fragmentos de código que incluí están **basados en ejemplos tradicionales de C** del manual de Linux, libros de Tanenbaum y Silberschatz, y recursos de OSDev.
* El código de **IPC con señales** y **deadlocks con mutex** está inspirado en ejemplos de:

  * [Beej’s Guide to Unix IPC](https://beej.us/guide/bgipc/)
  * [POSIX Threads Programming](https://computing.llnl.gov/tutorials/pthreads/)

---

### 📘 En resumen

Las fuentes principales usadas son una combinación de:

* **Libros de referencia (Silberschatz, Tanenbaum, Stallings)** para teoría.
* **Documentación POSIX y man pages** para exactitud técnica.
* **OSDev Wiki y repositorios GitHub** para ejemplos de implementación.
* **Guías educativas (GeeksforGeeks, TLDP)** para ejemplos y visualizaciones.

---
