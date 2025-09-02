## iAPx86
La compatibilidad fue la clave al enorme éxito de esta arquitectura, pero también la condicionó en determinados períodos de su evolución. El compromiso de compatibilidad adoptado significa que las decisiones de fondo adoptadas inicialmente en el diseño de la arquitectura, inevitablemente condicionarán lo que puede o no hacerse en el futuro.

## **Arquitectura de 16 bits básica**
![[Pasted image 20250902003843.png]]
### Registros de propósito general
Son 4 de 16 bits (**AX**, **BX**, **CX**, **DX**), cada uno dividido en dos registros de 8 bits, que dividen al valor en su parte alta (**AH**, **BH**, **CH**, **DH**) y su parte baja (**AL**, **BL**, **CL**, **DL**). Un programa podía usar tanto los registros de 16 como los registros de 8 bits.
### Registros índice
Son dos, **SI** y **DI**, los cuales están disponibles para direccionamiento indexado y para operaciones de cadenas de caracteres.
### Registros punteros
También están los registros punteros, el **SP** es el puntero a la última posición ocupada en la pila, es decir, el **Stack Pointer** y **BP** el puntero a la base de la pila, es decir, el **Base Pointer**.

El **Instruction Pointer** o **IP** es un registro de 16 bits que contiene el desplazamiento de dirección de la siguiente instrucción que se ejecuta. 
### Registro Flags
Es un registro de 16 bits, de los cuales nueve sirven para indicar el estado actual de la máquina y el resultado del procesamiento. La tabla contiene 16 posiciones, de 0 a 15, que son los 16 bits del registro **flags**, numeradas **little endian**.
![[Pasted image 20250902005729.png]]
Las flags son las siguientes
* **OF** $\rightarrow$ Overflow. Indica si la operación causó overflow o no.
* **DF** $\rightarrow$ Direction. Controla la selección de incremento o decremento de los registros SI y DI en las operaciones con cadenas de caracteres
* **IF** $\rightarrow$ Interruption. Controla el disparo de las interrupciones.
* **TF** $\rightarrow$ Trap. Permite la operación del procesdor en modo depuración.
* **SF** $\rightarrow$ Sign. 0 = positivo, 1 = negativo.
* **ZF** $\rightarrow$ Zero. Indica si el resultado de la operación es 0 (ZF = 1) o no (ZF = 0).
* **AF** $\rightarrow$ Auxiliary Carry. Contiene el carry del bit 3.
* **PF** $\rightarrow$ Parity. Indica si el número es par (bit menos significativo 0) o impar (bit menos significativo 1). Si es par PF = 1, sino PF = 0.
* **CF** $\rightarrow$ Carry Flag. Contiene el carry del bit más significativo después de una operación aritmética. También almacena el último bit en una operación de desplazamiento o rotación.
### Registros de Segmento
Definen áreas de 64kb dentro del espacio de direcciones de 1Mb del procesador 8086. Pueden solaparse total o parcialmente. No es posible acceder a una posición de memoria no definida por algún segmento.
* **CS** $\rightarrow$ Code Segment. Almacena la dirección inicial del segmento de código de un programa.
* **DS** $\rightarrow$ Direction Segment. Almacena la dirección inicial de un segmento de datos de programa.
* **SS** $\rightarrow$ Stack Segment. Permite la colocación en memoria de una pila, para almacenar temporalmente direcciones y datos.
* **ES** $\rightarrow$ Extra Segment. Se utiliza en los casos donde se requiera usar un registro extra de segmento para manejar el uso de memoria.

## **IA-32**
![[Pasted image 20250902123207.png]]
Extiende los registros, manteniendo la compatibilidad con aquellos de 16 bits, a 32 bits. Como se puede observar, la parte baja de los registros son los de 16 bits. Se mantienen los registros de segmento con 16 bits también.

### Floating Point Unit (FPU)
![[Pasted image 20250902123415.png]]
Es un entorno de ejecución separado dentro de la arquitectura IA-32. Consiste de 8 registros de datos y tres registros de propósito dedicado.
Cada uno de los registros R0-R7 tiene 80 bits, dando así lugar a operaciones con numeros de doble precisión, o con 2 flotantes de precisión simple por registro.
El bit mas significativo es el signo, los 15 siguientes son el exponente y los últimos 64 la mantisa.
![[Pasted image 20250902124818.png]]
La FPU **funciona como pila**.
<img src=https://halcyoona.wordpress.com/wp-content/uploads/2018/10/floating.gif>
### Extensiones MMX
Son registos de 64 bits que sirven para tareas multimedia. Para implementarlos, se tomaron los 64 bits menos significativos de los registros R0-R7 de la FPU. Entonces no podían utilizarse ambas cosas a la vez.

## **Intel®64**
![[Pasted image 20250902125218.png]]
Los registros de propósito general se extienden a 64 bits y aparecen 8 registros de 64 bits adicionales.

# **MODOS DE DIRECCIONAMIENTO**
## ¿Cómo obtengo los operandos?
Las instrucciones de estos procesadores pueden obtener los operandos desde:
* La instrucción en si misma
* Un registro
* Una posición de memoria
* Un puerto de E/S
## ¿Cómo almaceno los resultados?
El resultado de una instrucción puede tener como destino para su almacenamiento:
* Un registro
* Una posición de memoria
* Un puerto de E/S

## 1. Modo Implícito
Instrucciones en las cuales el `opcode` es suficiente como para establecer que operación realizar, y cuál es el operando.
Las instrucciones que operan sobre los flags suelen ser ejemplos de este modo.

## 2. Modo Inmediato
Instrucciones donde el operando fuente viene dentro del código de la instrucción, con lo cual no es necesario ir a buscarlo a memoria luego de decodificada la instrucción. Por ejemplo `ADD al, '0'`
## 3. Modo Registro
Todos los operandos involucrados son registros del procesador. No importa si hay uno o más operandos, son todos registros. Por ejemplo, `MOV EAX, EBP`

## 4. Modos de Direccionamiento a Memoria
Los procesadores IA-32 organizan la memoria como una secuencia de byres, direccionables a través de su Bus de Address. La memoria conectada a este bus es la **memoria física**; y el espacio de direcciones que pueden volcarse sobre este bus se denomina **direcciones físicas**.
El espacio de direccionamiento se administra por **segmentación**.
![[Pasted image 20250902135050.png]]
Intel decidió, con la familia del iAPx86, organizar el espacio de direccionamiento en segmentos. Por el compromiso de la compatibilidad, Intel quedó atada a mantener el esquema.
### Condiciones iniciales de segmentación
* 4 registros de segmento para almacenar hasta 4 selectores de segmento.
* Registros de 16 bits, los segmentos tienen a lo sumo 64K de tamaño.
* Expresión de las direcciones en el modelo de programación mediante dos valores:
   * Identificador del segmento en el que se encuentra la variable  o la instrucción que se desea direccionar.
   * Desplazamiento, offset, o **dirección efectiva** a partir del inicio de ese segmento en donde se encuentra efectivamente.

Para identificar un operando de una instrucción en memoria, se utiliza lo que Intel denomina **dirección lógica**. Esta es una dirección abstracta expresada en términos de su arquitectura pero que necesita ser procesada para convertirse en la dirección fisica que saldrá hacia el bus del sistema.
![[Pasted image 20250902135952.png]]
### Desplazamiento para ubicar operandos en memoria
Un desplazamiento tiene al menos uno de los siguientes componentes, o cualquiera de las combinaciones posibles:
* **Desplazamiento Directo:** un valor de 8, 16 o 32 bits, incluido en la instrucción.
* **Base:**  Un valor contenido en un registro de propósito general, que indica una dirección a partir de la cual se calcula el desplazamiento. Un valor de 32 bits en IA-32, 64 bits en Intel 64.
* **Índice:** Valor contenido en un registro de propósito general, que representa la dirección a la cual nos queremos referir. Al incrementarse permite recorrer el buffer de memoria. Es de 32 bits para IA-32, 64 bits para Intel 64.
* **Escala:** Valor por el cual multiplicar el índice. Puede valer 2, 4 u 8.

![[Pasted image 20250902140750.png]]
En general, la expresión por la que se calcula el desplazamiento internamente por el procesador es:
$$Dirección \ Efectiva = Base + (Índice * Escala) + Desplazamiento$$
El registro ESP no puede usarse de índice, pues su uso es dedicado como puntero de pila. Cuando se usan como registros base a ESP y EBP, se usan asociados al segmento SS. Para el resto de los usos se asocian al DS. El factor de escala solo se puede usar cuando se utilizan registros índice.

# **Tipos de Datos**
![[Pasted image 20250902141258.png]]
## Almacenamiento en memoria
Esta arquitectura emplea el almacenamiento en memoria de las variables en formato Little Endian. Una vaariable de varios bytes de tamaño almacena su byte menos significativo en la dirección con que se referencia a la variable y, a partir de allí, coloca el resto de los bytes en orden de significancia, con el byte más significativo en la dirección de memoria más alta (es decir, termina con el menor).
![[Pasted image 20250902141734.png]]
## Alineación en memoria
Los procesadores Intel no ponen restricciones respecto de la alineación en memoria para las diferentes variables de los programas. Esto da gran flexibilidad a la hora de aprovechar al máximo la memoria. Pero, si una variable queda repartida en dos filas distintas, se requieren **dos ciclos de lectura** para acceder a ella. De esta manera perdemos performance.