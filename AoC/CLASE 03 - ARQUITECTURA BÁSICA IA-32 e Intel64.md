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