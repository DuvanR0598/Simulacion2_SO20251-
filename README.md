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

3. How would you run the scheduler to reproduce each of the examples in the chapter?
   
   <details>
   <summary>Answer</summary>
   Coloque aqui su respuerta
   </details>
   <br>

4. How would you configure the scheduler parameters to behave just like a round-robin scheduler?

   <details>
   <summary>Answer</summary>
   Coloque aqui su respuerta
   </details>
   <br>

5. Craft a workload with two jobs and scheduler parameters so that one job takes advantage of the older Rules 4a and 4b (turned on
with the -S flag) to game the scheduler and obtain 99% of the CPU over a particular time interval.

   <details>
   <summary>Answer</summary>
   Coloque aqui su respuerta
   </details>
   <br>

6. Given a system with a quantum length of 10 ms in its highest queue, how often would you have to boost jobs back to the highest priority level (with the `-B` flag) in order to guarantee that a single longrunning (and potentially-starving) job gets at least 5% of the CPU?

   <details>
   <summary>Answer</summary>
   Coloque aqui su respuerta
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
