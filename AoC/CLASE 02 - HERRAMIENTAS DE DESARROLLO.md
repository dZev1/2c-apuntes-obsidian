## **LENGUAJES DE PROGRAMACIÓN**
### LENGUAJE PROCESADOR
Las CPUs definidas por los modelos originales fueron pensadas para tratar con valores que pueden tomar dos estados:
* 1 - verdadero - tensión V
* 0 - falso - tensión 0
Por este motivo cualquier microprocesador "habla" en binario.
Pero a los seres humanos no nos es natural hablar en binario. Pensemos que ya escribir un 1 en vez de un 0 en cualquier parte del programa y ya nos será imposible encontrarlo más adelante.
![[Pasted image 20250901111847.png]]
Esta imagen tiene un error, y como se ve es muy dificil de encontrar. El error es este
![[Pasted image 20250901111916.png]]

### LENGUAJE ENSAMBLADOR
En el lenguaje ensamblador, o "lenguaje de máquina", cada instrucción tiene un nombre alusivo a la operación que realiza y se lo representa por su abreviatura.
`MOV`, por MOVE, `ADD` por ADDITION, etc.
Cada sentencia en el programa corresponde a **una y solo una instrucción de la CPU** en el caso general. Con ayuda de un programa llamado **Ensamblador**, el texto se convierte a números binarios, el único lenguaje que entiende el microprocesador. A este texto original escrito en lenguaje "humano" se lo llama **código fuente**.
![[Pasted image 20250901112322.png]]

### Lenguaje de alto nivel
A diferencia del Assembler, en estos lenguajes **cada sentencia se compone de varias instrucciones del procesador**. Esto nos permite escribir aplicaciones de mayor complejidad con menos texto, no necesariamente con menos instrucciones finales.
El programa se escribe en un archivo de texto plano, como en Assembler y, con ayuda de un programa llamado **compilador** se convierte ese texto a números binarios, explotando cada sentencia en una o más instrucciones al microprocesador.

## **HERRAMIENTAS DE DESARROLLO**
![[Pasted image 20250901113221.png]]
### ¿Qué hace cada herramienta?
#### *Compilador*
Analiza sintácticamente un archivo de texto que contiene el programa fuente. Si está escrito de manera correcta, respetando la semántica del lenguaje para el cual compila, entonces genera un código binario adecuado para que el microprocesador lo ejecute.
Además **reemplaza los nombres lógicos** que adoptemos en nuestro programa para variables o funciones **por las direcciones de memoria** donde se ubican las mismas. No puede resolver referencias a funciones exteriores al archivo fuente que analiza. Esto lo resuelve la siguiente etapa.
![[Pasted image 20250901113732.png]]
Antes de hacer su trabajo, invoca a un programa denominado **preprocesador**, el cual elimina los comentarios, incluye otros archivos (por ejemplo, la línea `#include <stdio.h>` se reemplaza por el contenido del archivo stdio.h) y reemplaza todas las macros (aquellas definidas con `#define`).
Si el compilador genera errores, entonces **está mal escrito y hay que revisarlo**.
Si no da errores,  **solo significa que está bien escrito, no que funcione correctamente**.
Una vez que compiló, se genera un **programa objeto**, el cual es un binario pero que aún no está listo para poder ejecutarse.

#### *Linker*
Es un programa que **toma el archivo objeto**, lo **enlaza con otros programas objeto y bibliotecas de código** para así generar un **programa ejecutable** por el sistema operativo en el cual estemos trabajando.
**Enlazar** significa poner todos los bloques de código **juntos** y **ordenar código y datos en secciones comunes** para luego guardar ese conjunto en un **único archivo ejecutable**. Una vez ordenado, **resolver cada referencia** a una variable o función que en la fase de compilación eran externas. Por último marcar **el punto de entrada del programa**.