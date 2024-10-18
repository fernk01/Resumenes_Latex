# TCP (Transmission Control Protocol)
El Transmission Control Protocol (TCP). Se originó en la implementación inicial de la red en la que complementó al Protocolo de Internet (IP).

**Mecanismos de Control de Errores en TCP**

1. **Checksum**: Cada segmento TCP incluye un campo de checksum que se utiliza para verificar la integridad de los datos. Si el checksum calculado en el receptor no coincide con el checksum del segmento recibido, se considera que el segmento está corrupto y se descarta.

2. **Numeración de Secuencia y ACKs**: TCP utiliza números de secuencia para cada byte de datos transmitido. El receptor envía confirmaciones (ACKs) que indican el próximo número de secuencia esperado. Esto permite detectar y corregir la pérdida de segmentos.

3. **Retransmisión de Segmentos**: Si un segmento no es confirmado dentro de un tiempo determinado (temporizador de retransmisión), TCP retransmite el segmento. Esto asegura que los datos perdidos o corruptos sean reenviados.

4. **ACKs Duplicados y Retransmisión Rápida**: Si el emisor recibe múltiples ACKs duplicados para el mismo número de secuencia, esto indica que un segmento intermedio se ha perdido. TCP utiliza la retransmisión rápida para reenviar el segmento perdido sin esperar a que expire el temporizador.

**Mecanismos de Control de Flujo en TCP**

1. **Ventana Deslizante**: TCP utiliza una ventana deslizante para controlar la cantidad de datos que se pueden enviar antes de recibir una confirmación. El tamaño de la ventana se ajusta dinámicamente en función de la capacidad del receptor (control de flujo), es decir, la cantidad de espacio disponible en el buffer del receptor.

2. **Ventana de Recepción**: El receptor informa al emisor sobre el tamaño de la ventana de recepción, que es la cantidad de datos que puede manejar sin problemas (control de flujo). Esto evita que el emisor sature al receptor con demasiados datos.

3. **Control de Congestión**: TCP implementa varios algoritmos de control de congestión (como Slow Start, Congestion Avoidance, Fast Retransmit y Fast Recovery) para ajustar la tasa de transmisión en función de las condiciones de la red y evitar la congestión.
La ventana de congestión se ajusta dinámicamente en función de la congestión detectada en la red y se mide en términos de la cantidad de datos que se pueden enviar antes de recibir una confirmación.
   - **Slow Start**: Inicialmente, TCP aumenta la tasa de transmisión exponencialmente hasta que se alcanza un umbral (ssthresh). Luego, cambia a un crecimiento lineal. Cuando decimos que aumentamos la tasa de transmisión exponencialmente, significa que duplicamos la tasa de transmisión en cada ronda y con tasa de transmision nos referimos a la cantidad de paquetes que enviamos en cada ronda.
   - **Congestion Avoidance**: TCP aumenta la tasa de transmisión de forma lineal para evitar la congestión. Si se detecta congestión, se reduce la tasa de transmisión y se activa Slow Start.
   - **Fast Retransmit y Fast Recovery**: TCP retransmite rápidamente los segmentos perdidos y recupera la ventana de transmisión sin esperar a que expire el temporizador.


**Resumen <br>**
TCP combina estos mecanismos de control de errores y control de flujo para proporcionar una transmisión de datos fiable y eficiente. La combinación de checksum, numeración de secuencia, ACKs, retransmisión de segmentos y control de congestión hace que TCP sea robusto y adecuado para una amplia variedad de aplicaciones de red.


## Servicios proporcionados por TCP:
1. **Servicio orientado a la conexión**:  
   TCP establece una conexión entre el emisor y el receptor antes de transmitir datos. Esta conexión garantiza que los datos se entreguen de manera confiable y en orden, utilizando un proceso llamado **three-way handshake** (acuerdo en tres fases).
   
2. **Servicio de entrega confiable**:  
   TCP garantiza la entrega confiable y sin errores de los datos. Utiliza mecanismos como:
   - **Retransmisión de paquetes**: si se detecta pérdida de datos.
   - **Control de flujo**: para gestionar el ritmo de transmisión entre emisor y receptor.
   - **Corrección de errores**: asegurando la integridad de los datos transmitidos.

3. **Mecanismo de control de congestión**:  
   TCP ajusta dinámicamente su tasa de transmisión para evitar la congestión en la red y garantizar un rendimiento óptimo.

4. **Mecanismo de recuperación de errores**:  
   TCP puede detectar y recuperarse de errores en la transmisión de datos, garantizando una entrega confiable.
   - retransmisión de paquetes.

5. **control de flujo**:  
   TCP utiliza un mecanismo de control de flujo para evitar que el emisor sature al receptor con demasiados datos. El receptor puede informar al emisor sobre su capacidad de recepción actual, lo que permite una transmisión eficiente de datos.

6. **Control de Congestión**:  
   TCP utiliza un mecanismo de control de congestión para evitar la congestión en la red. Ajusta dinámicamente la tasa de transmisión en función de la congestión detectada en la red.

## Características de TCP:
- **Ejecución en sistemas terminales**:  
   El protocolo TCP se ejecuta solo en los sistemas terminales, no en los elementos intermedios de la red (como routers y switches de capa de enlace). Estos dispositivos no mantienen el estado de la conexión TCP, ya que únicamente manejan los datagramas, sin ser conscientes de las conexiones TCP.
   
- **Conexión punto a punto**:  
   Una conexión TCP generalmente es **punto a punto**, es decir, entre un único emisor y un único receptor.

- **Servicio full-duplex**:  
   TCP permite la transmisión simultánea de datos en ambas direcciones.

- **Three-way handshake (acuerdo en tres fases)**:  
   El cliente inicia el proceso enviando un segmento especial, el servidor responde con un segundo segmento, y finalmente el cliente envía un tercer segmento. Solo el tercer segmento puede llevar datos de la capa de aplicación, mientras que los dos primeros no contienen carga útil.

## Buffers de TCP:
- **Buffer de emisión**:  
   TCP toma fragmentos de datos desde el buffer de emisión y los pasa a la capa de red para su transmisión.
   
- **Buffer de recepción**:  
   TCP recibe los datos de la capa de red y los coloca en el buffer de recepción, para que la aplicación pueda procesarlos.

### MSS (Maximum Segment Size) vs. MTU (Maximum Transmission Unit):
El **tamaño máximo de segmento (MSS)** es el tamaño más grande de un segmento que TCP puede enviar. Es una métrica de la capa de transporte.

MTU es el tamaño máximo de un paquete que se puede transmitir en un enlace físico. Se aplica a la capa de red.


## Header de TCP:
### Número de secuencia y números de reconocimiento (acknowledgment):
- **Número de secuencia**:  
   Identifica el primer byte de datos en un segmento TCP. Se utiliza para reordenar los segmentos en el receptor. Esto se hace con el fin de minimizar la posibilidad de que un segmento que todavía está presente en la red a causa de una conexión anterior que ya ha terminado entre dos hosts pueda ser confundido con un segmento válido de una conexión posterior entre esos dos mismos hosts (que también estén usando los mismos números de puerto que la conexión antigua)
   - una conexión TCP eligen aleatoriamente un número de secuencia inicial.

- **Número de reconocimiento**:
    Indica el siguiente byte que el emisor espera recibir. Se utiliza para confirmar la recepción de datos.
    - TCP proporciona reconocimientos acumulativos.

# Principios del control de congestión en TCP

## Las causas y los costes de la congestión
### Escenario 1: dos emisores, un router con buffers de capacidad ilimitada
El escenario que describes se refiere a una situación en la que dos hosts, A y B, están enviando datos a través de un router con **buffers de capacidad ilimitada** hacia un **enlace** de salida compartido, que tiene una capacidad total de **R**. Los hosts A y B envían datos a una velocidad de **l** bytes/segundo, y comparten un enlace de salida con capacidad limitada. El objetivo es entender cómo se comporta el sistema cuando la tasa de transmisión de los hosts se aproxima o supera **R/2**, la mitad de la capacidad del enlace.
[You tube](https://www.youtube.com/watch?v=Y0amVWTl2a0&list=PL_rzFKbTx1Vno2jBUnBjjE3xKG_qySotn&index=12)

#### Desglose del escenario:

1. **Modelo del sistema**:
   - **Dos emisores** (host A y host B) están enviando datos a través de un **router** hacia un **enlace de salida compartido**.
   - Ambos emisores transmiten datos a una tasa de **l** bytes/segundo.
   - El enlace tiene una capacidad de transmisión de **R** bytes/segundo.
   - El router tiene un **buffer ilimitado**, por lo que puede almacenar una cantidad infinita de paquetes, evitando el descarte de paquetes, pero aumentando el **retardo**.

2. **Tasa de transferencia**:
   - Si la tasa de transmisión de un host está entre **0** y **R/2**, la tasa de transferencia en el receptor es igual a la tasa de transmisión en el emisor. Esto significa que el receptor recibe todos los datos que el emisor está enviando, y el retardo se mantiene finito.
   - Cuando la tasa de transmisión excede **R/2**, el enlace no puede manejar más de **R/2** para cada emisor. Por lo tanto, la tasa de transferencia efectiva de cada conexión no puede superar **R/2**, sin importar cuánto intenten aumentar la tasa los emisores A y B.

3. **Impacto de la congestión y el retardo**:
   - Mientras la tasa de transmisión de cada emisor es **menor o igual a R/2**, el sistema funciona correctamente, ya que los datos enviados por A y B son recibidos en su totalidad en el destino.
   - **Cuando la tasa de transmisión se aproxima a R/2**, el retardo promedio empieza a aumentar significativamente. Esto se debe a que, a medida que el router se congestiona, los paquetes se almacenan en el buffer del router, lo que introduce un mayor retardo para cada paquete que espera en la cola.
   - **Si la tasa de transmisión excede R/2**, el buffer del router comienza a llenarse indefinidamente, lo que hace que el retardo crezca hacia el infinito. Esto ocurre porque el router no puede procesar los paquetes más rápido de lo que llegan, lo que provoca un aumento continuo en la cola de paquetes.

4. **Buffer infinito** y el retardo creciente:
   - Aunque el router tiene un **buffer infinito** (lo que evita la pérdida de paquetes), el problema principal en este caso es el **retardo creciente**.
   - Cuando la tasa combinada de los dos emisores (A y B) supera la capacidad del enlace de salida (**R**), el buffer empieza a llenarse rápidamente. Como los paquetes se encolan para ser enviados, cada nuevo paquete tiene que esperar más tiempo antes de ser procesado y transmitido.
   - Esto crea un **retardo de cola**. A medida que aumenta el número de paquetes en la cola, el tiempo que cada paquete tiene que esperar antes de ser enviado también aumenta, lo que causa un aumento en el **retardo promedio**.

5. **Limitación del rendimiento**:
   - El rendimiento máximo por cada conexión se limita a **R/2**. Esto ocurre porque el enlace de salida tiene que dividir su capacidad de transmisión entre las dos conexiones de A y B.
   - Aunque los emisores intenten transmitir a una tasa superior a **R/2**, no podrán enviar más de **R/2** bytes/segundo al receptor, debido a la limitación de la capacidad del enlace compartido.

#### Problemas surgidos por operar cerca de la capacidad del enlace:

- **Congestión**: A medida que los emisores se acercan a **R/2**, la congestión en el router aumenta. El buffer comienza a llenarse, lo que genera mayores **retardos de cola**.
- **Retardo infinito**: Si los emisores intentan transmitir a una tasa superior a **R/2**, la congestión se vuelve insostenible, y el retardo puede crecer indefinidamente si los hosts continúan transmitiendo a esa tasa.
- **Capacidad compartida**: El enlace de salida debe dividir su capacidad entre los dos emisores, lo que significa que ninguno de ellos puede transmitir a una tasa superior a **R/2**, sin importar cuánto intenten aumentar su velocidad de transmisión.

#### Analogía para entender mejor:

Imagina que tienes dos coches (A y B) que se acercan a un peaje con solo un carril (el router con un enlace de salida). El peaje puede procesar coches a una velocidad constante de **R** coches por minuto. Si cada coche viaja a una velocidad constante (A y B enviando datos a velocidad **l**), el peaje puede manejar **R/2** coches por minuto de cada uno, sin problemas.

Sin embargo, si ambos coches aceleran e intentan enviar más de **R/2** coches por minuto al peaje, se formará una **cola** de coches esperando ser procesados. Esta cola solo crecerá si no se ralentizan, ya que el peaje no puede manejar más coches de los que puede procesar. Con el tiempo, la **cola se vuelve infinita** (retardo infinito), y el tiempo de espera para que los coches pasen por el peaje será muy largo.

#### Resumen:

- Cuando dos emisores comparten un enlace con capacidad limitada, el rendimiento de cada uno se limita a la mitad de la capacidad del enlace (**R/2**).
- Si intentan transmitir a una tasa superior a **R/2**, no solo su rendimiento no mejora, sino que los paquetes empiezan a experimentar **retardos infinitos** debido al almacenamiento en los buffers del router.
- Aunque el buffer sea ilimitado, la congestión hace que el retardo crezca de manera indefinida, lo que afecta negativamente el tiempo de entrega de los datos.

- "donde los números de secuencia de TCP cuentan los bytes del flujo de datos, en lugar de los paquetes". (pag 207)

#  Sack
¡Claro! La opción SACK (Selective Acknowledgment) en TCP se utiliza para mejorar la eficiencia de la transmisión de datos, especialmente en redes con alta latencia o pérdida de paquetes. Aquí tienes una descripción detallada del formato de la opción SACK:

### Formato de la Opción SACK

La opción SACK permite al receptor informar al emisor sobre los segmentos de datos que ha recibido correctamente, incluso si no son consecutivos. Esto ayuda al emisor a retransmitir solo los segmentos que realmente se han perdido, en lugar de retransmitir todos los segmentos posteriores a una pérdida.

#### Estructura de la Opción SACK

1. **Kind (1 byte)**: Este campo indica que se trata de una opción SACK. Su valor es 5.
2. **Length (1 byte)**: Este campo indica la longitud total de la opción SACK en bytes.
3. **Left Edge of Block 1 (4 bytes)**: Este campo indica el número de secuencia del primer byte del primer bloque de datos recibido correctamente.
4. **Right Edge of Block 1 (4 bytes)**: Este campo indica el número de secuencia del byte siguiente al último byte del primer bloque de datos recibido correctamente.
5. **Left Edge of Block 2 (4 bytes)**: Este campo indica el número de secuencia del primer byte del segundo bloque de datos recibido correctamente (si existe).
6. **Right Edge of Block 2 (4 bytes)**: Este campo indica el número de secuencia del byte siguiente al último byte del segundo bloque de datos recibido correctamente (si existe).

Un ejemplo de una opción SACK podría verse así:

```
+--------+--------+--------+--------+--------+--------+--------+--------+
| Kind=5 | Length | Left Edge of Block 1 | Right Edge of Block 1 |
+--------+--------+--------+--------+--------+--------+--------+--------+
| Left Edge of Block 2 | Right Edge of Block 2 |
+--------+--------+--------+--------+--------+--------+--------+--------+
```

### Ejemplo Descriptivo

Supongamos que un receptor ha recibido correctamente los segmentos con números de secuencia 1000-1999 y 3000-3999, pero ha perdido los segmentos entre 2000 y 2999. El receptor enviaría una opción SACK al emisor con la siguiente información:

- **Left Edge of Block 1**: 1000
- **Right Edge of Block 1**: 2000
- **Left Edge of Block 2**: 3000
- **Right Edge of Block 2**: 4000

Esto le indica al emisor que solo necesita retransmitir los segmentos entre 2000 y 2999.

Espero que esto te haya ayudado a entender mejor el formato de la opción SACK. Si tienes alguna otra pregunta o necesitas más detalles, ¡no dudes en preguntar!

Origen: Conversación con Copilot 29/9/2024
(1) RFC 2018: TCP Selective Acknowledgment Options - RFC Editor. https://www.rfc-editor.org/rfc/rfc2018.
(2) Selective Acknowledgments (SACK) in TCP - GeeksforGeeks. https://www.geeksforgeeks.org/selective-acknowledgments-sack-in-tcp/.
(3) packetlife.net. https://packetlife.net/blog/2010/jun/17/tcp-selective-acknowledgments-sack/.