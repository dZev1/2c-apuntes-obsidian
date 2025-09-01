## **Arquitectura**
Conjunto de recusos accesibles para el programador, que por lo general se mantienen a lo largo de los diferentes modelos de procesadores de esa arquitectura. Incluye:
- Registros
- Set de instrucciones
- Estructuras de memoria relacionadas

## **Micro Arquitectura**
Implementación de la arquitectura. El diseño lógico detrás del set de registros y del modelo de programación. Puede ser simple o robusta y poderosa. Cambia de un modelo a otro dentro de una misma familia.

Se trata de definir los aspectos más relevantes en la arquitectura de un computador procurando maximizar el rendimiento del procesador, pero sin dejar de satisfacer otras limitaciones impuestas por los usuarios.

Requiere diseñar es **set de instrucciones**, el **modo en que se manejará la memoria** y sus modos de direccionamiento, los restantes bloques funcionales que componen el CPU, como el **file register**, el **datapath** y el diseño de la **lógica de control**

## **ISA (Instruction Set Architecture)**
Set de instrucciones visibles por el programador. Es el límite entre el software y el hardware.
### **Clases de ISA**
* Registros de propósito general - Registros dedicados
* Registro-memora - Load Store
* Alineación obligatoria de datos - administración de a bytes
* CISC (Complex Instruction Set Computing) - RISC (Reduced Instruction Set Computing)
* Instrucciones de tamaño fijo - Instrucciones de Tamaño variable

## **Organización**
Detalles de implementación de la ISA. Comprende:
* Organización e interconexión de la memoria
* Diseño de los diferentes bloques de la CPU y para implementar el Instruction Set
* Implementación de paralelismo a nivel de instrucciones y/o datos.

*Hay muchos procesadores con la misma ISA pero con distintas organizaciones.*

## **Hardware**
Detalles de diseño lógico y tecnología de fabricación. *Hay procesadores con la misma ISA y la misma organización , pero distintos a nivel de hardware y diseño lógico detallado.*
# Procesador
Sea cual sea el set de instrucciones que se vaya a implementar, *hay dos actividades que inicialmente cualquier instrucción debe realizar*:
1. Apuntar el PC a la posición de memoria que contiene la siguiente instrucción.
2. Leer uno o dos registros contenidos en la palabra de instrucción.

Las acciones posteriores del procesador dependen de cada instrucción específica.

Cuanto más simple el set de instrucciones, el paso siguiente será el mismo para un porcentaje mayor de instrucciones.

Luego de los dos pasos anteriores un porcentaje de las instrucciones usa la ALU.

Las instrucciones de **referencia a memoria** (load, store, branch incondicional), usan la ALU para *calcular la dirección de memoria a acceder*. Las instrucciones **aritméticas y lógicas** usan la ALU para *ejecutar la instrucción solicitada*. Los **saltos condicionales** para *evaluar la condición de salto*.
## **Datapath**
Camino que recorren las intrucciones y los datos por los buses internos y los bloques circuitales necesarios para su procesamiento.

Inicia en la memoria cuando se busca una instrucción o cuando se busca u dato. Afecta al **PC**, que se debe ajustar a la siguiente instrucción.

### **REGISTER FILE**
Un register file de tamaño N es un arreglo de N registros, numerados de 0 a N-1, que se pueden leer y escribir proporcionando tan solo el número de registro al que se accede.

**Se puede implementar con un decodificador para cada puerto de lectura o escritura y un arreglo de registros construida a partir de D Flip-Flops**

Como la lectura de un registro no cambia ningún estado, y la salida Q de un flip flop está disponible, solo necesitamos proporcionar un número de registro como entrada y la única salida serán los datos contenidos en el registro i-ésimo.

Para escribir un registro necesitamos tres entradas: el número de registro dentro del arreglo, los datos a escribir y un clock que controla la escritura en el registro.

Usamos uno con dos puertos de lectura y uno de escritura.

![[Pasted image 20250831210724.png]]

# Pipeline
Un procesador ejecuta una máquina de estados compuesta por diferentes etapas.
En los primeros microprocesadores cada etapa se ejecutaba durante uno o más ciclos de clock, durante los cuales la CPU estaba dedicada a ella.
Una vez finalizada una etapa, en el siguiente ciclo de clock se pasaba a ejecutar la siguiente.
Ejecutar una instrucción insumía varios ciclos de clock.

## **Qué hace el pipeline?**
Permite crear el efecto de superponer en el tiempo la ejecución de varias instrucciones a la vez.

Formaliza el concepto de ***Instruction Level Paralelism***

Requiere muy poco o ningún hardware adicional.
Solo necesita que los bloques del procesador que resuelven la máquina de estados de ejecución de una instrucción operen de manera simultánea.

Se logra si todos los bloques funcionales trabajan en paralelo, pero cada uno en una instrucción diferente. Es como una *línea de montaje*

## **Obstáculos de un pipeline**
Un pipeline no funciona idealmente en la vida real. Hay obstáculos que conspiran contra la eficiencia de un pipeline.
Al efecto ocasionado por un obstáculo se lo denomina *pipeline stall*. Su efecto degrada la performance del procesdor.
Hay tres clases de obstáculos.

### **1.  Obstáculos estructurales**
Se pueden dar por varias razones.
* Una etapa no está suficientemente atomizada y, entonces, para completarse requiere más de un ciclo de clock.
* Si dos instrucciones que usan esta etapa están más próximas del tiempo que necesita esta etapa para procesar su parte, caerán en conflicto de recursos para su ejecución.
Este grupo de instrucciones entonces no puede pasar por esa etapa del pipeline en un ciclo de clock. En estas circunstancias una de las instrucciones debe detenerse. Como consecuencia, el CPI aumenta en 1 o más respecto del valor ideal.

Para solucionarlo tenemos que si o si agregar hardware.
En el caso de accesos a memoria, se puede resolver mediante las siguientes opciones, por separado o combinadas:
1. Desdoblar el cache L1 en Cache de datos y Cache de instrucciones.
2. Emplear buffers de instrucciones implementados como pequeñas colas FIFO.
3. Ensanchar los buses más allá de los anchos de palabra del procesador.

Aumentar la profundidad del pipeline produce etapas mucho más simples y capaces de resolver en un clock su tarea.

### **2.  Obstáculos  de Datos**
Se producen cuando por efecto del pipeline, una instrucción requiere de un dato antes de que este esté disponible por efecto de la secuencia lógica prevista en el programa.

Para solucionar esto, se puede usar la estrategia de Forwarding:
* Ni bien el resultado está disponible, se lo realimenta a la entrada de la unidad de ejecución.
* La etapa que lo requiere lo dispone en el mismo ciclo de clock en que se escribirá en el operando destino.
* Permite ahorrar tiempo de escritura en el operando destino.
* Se aplica solamente a las etapas posteriores que quedarían en estado stall.
* Aquellas etapas que a pesar de la dependencia, no quedarían en stall, reciben el resultado cuando este es aplicado en el operando destino.
![[Pasted image 20250831221825.png]]

### **3.  Obstáculos de Control**
El pipeline busca instrucciones de forma secuencial.

Un branch es una instrucción de control de flujo que provoca una discontinuidad en el flujo de ejecución.

Es la peor situación en cuando a pérdida de performance; todo lo pre procesado debe descartarse, ya que son las instrucciones sucesoras secuenciales al branch en el programa. Como se rompe la secuencia de ejecución, ya no son necesarias y deben ser reemplazadas por las instrucciones del target del branch.

Se requieren N-1 ciclos de clock para obtener el próximo resultado, pues hay que vaciar el pipeline. con N la cantidad de etapas del pipeline. Esto es conocido como **branch penalty**.

Para solucionarlos, se pueden usar unidades de predicción de saltos.