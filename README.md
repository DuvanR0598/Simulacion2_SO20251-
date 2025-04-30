# Actividad de seguimiento - Simulación 2

|Integrante|correo|usuario github|
|---|---|---|
|Duvan Ferney Ruiz Ocampo|duvan.ruiz1@udea.edu.co|DuvanR0598|

## Instrucciones

Antes de empezar a realizar esta actividad haga un **fork** de este repositorio y sobre este trabaje en la solución de las preguntas planteadas en la actividad de simulación. Las respuestas deben ser respondidas en español o si lo prefiere en ingles en el lugar señalado para ello (La palabra **answer** muestra donde).


## Homework (Simulation)

This program, [mlfq.py](mlfq.py), allows you to see how the MLFQ scheduler presented in this chapter behaves. See the [README](https://github.com/remzi-arpacidusseau/ostep-homework/blob/master/cpu-sched-mlfq/README.md) for details.


### Questions

1. Run a few randomly-generated problems with just two jobs and two queues; compute the MLFQ execution trace for each. Make your life easier by limiting the length of each job and turning off I/Os.

   <details>
   <summary>Answer</summary>
      
   Para resolver el problema, ejecuté el programa mlfq.py con los siguientes parámetros: `python mlfq.py -n 2 -j 2 -m 10 -M 1000 -c`

   **¿Qué significa esto?**
   - `-n 2`: 2 colas.
   - `-j 2`: 2 trabajos.
   - `-m 10`: Cada trabajo tendrá como máximo 10 unidades de tiempo de ejecución (máximo 10 unidades de CPU).
   - `-M 1000`: Hace que el IO sea extremadamente raro (cada 1000 ticks)
   - `-c`: Que calcule y muestre el seguimiento de la ejecución.
  
   <br>
  
   **Seguimiento de Ejecución (Execution Trace):**
   - *Tiempo 0:* Job 0 comienza a ejecutarse inmediatamente.
   - *Tiempo 0 a 5:* Job 0 se ejecuta de forma continua.
   - *Tiempo 5:* Job 0 finaliza.
   - *Tiempo 5 a 12:* Job 1 comienza y se ejecuta de manera continua hasta finalizar.
  
   <br>

   **Analisis**
   - Ambos trabajos ingresaron al sistema al mismo tiempo.
   - Job 0 fue seleccionado primero, ya que los trabajos se ejecutan según prioridad y orden de llegada.
   - Como no hubo E/S, ni cambios de prioridad, los trabajos se ejecutaron de manera sencilla en el orden en que llegaron.
   - Job 1 tuvo que esperar a que Job 0 terminara para comenzar su ejecución (por eso su tiempo de respuesta es 5).
  
   <br>

   <div align="center">
      <img src="https://github.com/DuvanR0598/Simulacion2_SO20251-/blob/main/Imagenes/Pregunta%201.png?raw=true" alt="Pregunta 1" width="600"/>
   </div>

   </details>
   <br>

2. How would you run the scheduler to reproduce each of the examples in the chapter?
   
   <details>
   <summary>Answer</summary>   
      
   Para reproducir los ejemplos del capítulo usando el programador `mlfq.py`, se debe:

   <br>

   **1. Identificar las condiciones específicas de cada ejemplo:**
   - ¿Cuántos trabajos hay?
   - ¿Cuándo llega cada trabajo (startTime)?
   - ¿Cuánto CPU necesita cada trabajo (runTime)?
   - ¿Cada cuánto realiza operaciones de E/S?
   - ¿Cuántas colas de prioridad existen?
   - ¿Cuánto tiempo dura el quantum de cada cola?
   - ¿Hay boost de prioridades? (cada cuánto tiempo)
   - ¿Qué reglas de allotment (permanencia en nivel) se aplican?
  
   <br>

   **2. Traducir esas condiciones en opciones de línea de comandos para `mlfq.py`:**
   - Usando `-n`, `-q`, `-a`, `-Q`, `-A`, `-l`, `-j`, `-m`, `-M`, `-B`, etc.
  
   <br>

   **3. Ejecutar el programa usando esos parámetros.**
   - Si el ejemplo especifica trabajos exactos, se usa `-l` para definir los trabajos manualmente.
   - Si hay boost de prioridad, se usa `-B`.
   - Si hay diferentes tiempos de quantum o allotment por cola, usaré `-Q` y `-A`.
   
   </details>
   <br>

3. How would you configure the scheduler parameters to behave just like a round-robin scheduler?

   <details>
   <summary>Answer</summary>

   Para que el MLFQ (mlfq.py) se comporte como un planificador Round-Robin (RR) se debe configurar de manera que:

   **1. Haya solo una cola (un solo nivel de prioridad)**
   - Usar la opción: `-n 1`
  
   <br>

   **2. Definir el quantum (tiempo de CPU antes de cambiar de proceso) como lo que se quiere para el RR.**
   - Usar la opción: `-q <quantum>` (por ejemplo, `-q 4` para un RR clásico de 4 unidades de tiempo).
  
   <br>

   **3. No haya degradación de prioridad ni allotments especiales.**
   - No se necesita modificar allotments manualmente, ya que con una sola cola, los allotments no afectan.

   <br>

   **4. Sin boost, aunque con 1 sola cola ya no tiene sentido.**

   <br>

   **5. Sin operaciones de E/S, si se quiere un RR simple y limpio.**

   <br>

   **6. Comando de ejemplo para simular Round-Robin**
   - `python mlfq.py -n 1 -q 4 -l 0,10,0:2,8,0:4,6,0 -c`
  
   <br>

   
   *¿Qué hace esto?*
   - `-n 1`: Solo una cola → no hay prioridades.
   - `-q 4`: Cada proceso tiene 4 unidades de CPU antes de ser expulsado (time slice = 4).
   - `-l 0,10,0:2,8,0:4,6,0`: Defines 3 trabajos manualmente (tú puedes poner los trabajos que quieras).
   - `-c`: Calcula automáticamente el trace de ejecución.
  
   </details>
   <br>

4. Craft a workload with two jobs and scheduler parameters so that one job takes advantage of the older Rules 4a and 4b (turned on
with the -S flag) to game the scheduler and obtain 99% of the CPU over a particular time interval.

   <details>
   <summary>Answer</summary>
   
   **Comando Usado** → `python mlfq.py -n 2 -Q 5,10 -A 1,1 -S -l 0,50,4:0,50,0 -c`

   <br>

   - `-n 2`  2 niveles de cola (alta y baja prioridad)
   - `Q 5,10`  Quantum de 5 para la cola alta y 10 para la cola baja
   - `-A 1,1`  Solo 1 allotment por nivel (si se usa el quantum, se baja de nivel)
   - `-S`  **Clave:** si se hace E/S, se queda en el nivel actual y se reinician allotment y quantum
   - `-l 0,50,4:0,50,0`  Dos trabajos:
      - *Job 0:* empieza en `t=0`, requiere 50 unidades de CPU, hace E/S cada 4 unidades
      - *Job 1:* también en `t=0`, requiere 50 unidades, no hace E/S
   - `-c`  	Muestra el seguimiento de ejecución (Execution Trace) y estadísticas al final.
  
   <br>

   **¿Qué va a pasar?**
   - Job 0 hace E/S cada 4 unidades → nunca baja de nivel, se mantiene en la cola de mayor prioridad gracias a `-S`.
   - Job 1 no hace E/S → usa su quantum de 5, agota el allotment, baja al nivel 0, donde se ejecuta menos frecuentemente.
   - Durante los primeros ~50 ticks, Job 0 acapara casi toda la CPU.
  
   <br>

   **Resultado observado**
   - Job 0 ejecuta repetidamente en la cola de prioridad más alta (PRIORITY 1).
   - Realiza una operación de E/S cada 4 ticks, y gracias a la opción `-S`, se le reinicia el quantum y allotment sin degradarlo.
   - Mientras tanto, Job 1 nunca ejecuta durante ese intervalo, porque baja a la cola de menor prioridad y Job 0 ocupa constantemente la CPU.
  
   <br>

   <div align="center">
      <img src="https://github.com/DuvanR0598/Simulacion2_SO20251-/blob/main/Imagenes/Pregunta%204.png?raw=true" alt="Pregunta 1" width="600"/>
   </div>
   
   </details>
   <br>

5. Given a system with a quantum length of 10 ms in its highest queue, how often would you have to boost jobs back to the highest priority level (with the `-B` flag) in order to guarantee that a single longrunning (and potentially-starving) job gets at least 5% of the CPU?

   <details>
   <summary>Answer</summary>

   Durante el intervalo de duración `T`, el trabajo largo solo recibe CPU durante 10 ms (cuando es promovido). Queremos que ese trabajo obtenga al menos el 5 % del tiempo total del CPU.

   $\frac{10}{T} \ge 0.05\to T\le \frac{10}{0.05} = 200ms$

   <br>

   ✅ Para que un trabajo de larga duración reciba al menos el 5 % del CPU, se debe aplicar el (-B) cada 200 ms o menos. Dado lo anterior, se usaria el siguiente comando

   <br>

   `python mlfq.py -n 3 -Q 10,20,30 -A 1,1,1 -B 200 -l 0,100,0:0,100,0 -c`

   <br>

   Esta ejecución refleja exactamente lo que queremos: mostrar cómo el parámetro -B 200 (boost cada 200ms) garantiza que un trabajo de baja prioridad no se quede sin CPU, y pueda recibir al menos el 5 % del tiempo total.

   **En Conclusión:** Para garantizar que un trabajo de larga duración (que ha descendido a una cola de baja prioridad) obtenga al menos el 5 % del tiempo de CPU, debe recibir al menos 5 ms por cada 100 ms de CPU total.
   Dado que la cola más alta tiene un quantum de 10 ms, y los boosts con `-B`  promueven al trabajo hambriento a esa cola, se puede usar ese quantum para "inyectarle" tiempo de CPU.

   <br>

   El comando ejecutado reproduce el siguiente comportamiento:
   - 3 colas con quantum: 10 (alta), 20 (media), 30 (baja).
   - Allotment de 1 por nivel.
   - Boost cada 200 ms.
   - Dos trabajos de larga duración, ambos comienzan al mismo tiempo.
   - Sin E/S.
  
   <div align="center">
      <img src="https://github.com/DuvanR0598/Simulacion2_SO20251-/blob/main/Imagenes/Pregunta%205.png?raw=true" alt="Pregunta 1" width="600"/>
   </div>

   </details>
   <br>

7. One question that arises in scheduling is which end of a queue to add a job that just finished I/O; the -I flag changes this behavior
for this scheduling simulator. Play around with some workloads and see if you can see the effect of this flag.

   <details>
   <summary>Answer</summary>
   Coloque aqui su respuerta
   </details>
   <br>

## Conclusions

Coloque aqui las conclusiones...


### Criterios de evaluación
- [x] Despligue de los resultados y analisis claro de los resultados respecto a lo visto en la teoria.
- [x] Creatividad y orden.
- [x] Sección con las conclusiones de los experimentos realizados.
