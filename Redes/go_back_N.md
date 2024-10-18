# Retroceder N (GBN - Go Back N)

En un protocolo GBN (Go-Back-N, Retroceder N - protocolo de ventana deslizante), el emisor puede transmitir varios paquetes (si  están disponibles) sin tener que esperar a que sean reconocidos, pero está restringido a no tener más de un número máximo permitido, N, de paquetes no reconocidos en el canal.

- **base**: número de secuencia del paquete más antiguo no reconocido.
- **next_seq_num**: número de secuencia del siguiente paquete a enviar.

Se pueden identificar los cuatro intervalos en rango de los números de secuencia de los paquetes: 
- **Ya reconocidos**: paquetes que han sido reconocidos. 
    $$[0, base - 1]$$
- **Enviados pero no reconocidos**: paquetes que han sido enviados pero no reconocidos. 
    $$[base, next\_seq\_num - 1]$$
- **Por enviar**: paquetes que no han sido enviados. 
    $$[next\_seq\_num, base + N - 1]$$
- **No enviados**: paquetes que no han sido enviados. 
    $$[base + N, \infty]$$


**¿El receptor únicamente envía ACK del siguiente paquete?** <br>
En el protocolo Go-Back-N, el receptor envía ACKs acumulativos. Esto significa que el receptor envía una confirmación (ACK) para el último paquete recibido en orden. Por ejemplo, si el receptor ha recibido correctamente los paquetes 1, 2 y 3, pero el paquete 4 se pierde, el receptor seguirá enviando ACKs para el paquete 3 hasta que reciba el paquete 4. No envía ACKs para paquetes fuera de orden.

**confirmaciones acumulativas** significa que el receptor no tiene que enviar un ACK para cada paquete individualmente, sino que puede confirmar varios paquetes a la vez. Esto reduce la sobrecarga de la red y mejora la eficiencia del protocolo.

El recepto NO tiene la necesidad de almacenar los paquetes fuera de orden.

## Preguntas
Entiendo tu preocupación. En el protocolo Go-Back-N, el número de secuencia se utiliza para identificar los paquetes enviados. Si tienes un número de secuencia de 2 bytes, eso te da un rango de 0 a 65,535 (es decir, 65,536 números de secuencia en total).

Para un archivo de 1 GB (1,073,741,824 bytes), si cada paquete tiene un tamaño de, por ejemplo, 1,472 bytes (para evitar la fragmentación en una red Ethernet estándar), necesitarías aproximadamente:

$$ \frac{1,073,741,824 \text{ bytes}}{1,472 \text{ bytes/paquete}} \approx 729,125 \text{ paquetes} $$

Esto claramente excede el rango de 65,536 números de secuencia. Sin embargo, el protocolo Go-Back-N utiliza números de secuencia de forma cíclica (modular). Esto significa que después de alcanzar el número de secuencia máximo (65,535), el siguiente número de secuencia será 0 nuevamente¹².

El uso de números de secuencia cíclicos permite que el protocolo maneje grandes cantidades de datos, siempre y cuando el tamaño de la ventana deslizante sea menor que el rango de números de secuencia. En tu caso, mientras la ventana deslizante sea menor que 65,536, el protocolo debería funcionar correctamente.

1. 



2. ¿Cómo se calcula el tamaño de la ventana en Go-Back-N?

    El tamaño de la ventana en el protocolo Go-Back-N se determina en función de varios factores, incluyendo:

    - **Capacidad del receptor**: La cantidad de datos que el receptor puede manejar sin desbordarse.
    - **Condiciones de la red**: La latencia y la tasa de errores de la red pueden influir en el tamaño óptimo de la ventana.
    - **Número de secuencia**: El tamaño de la ventana debe ser menor que el rango del número de secuencia para evitar ambigüedades. Si el número de secuencia tiene un rango de 0 a \(2^k - 1\), el tamaño máximo de la ventana es \(2^k - 1\).

3. ¿Cómo funciona el control de flujo en Go-Back-N?

    El control de flujo en el protocolo Go-Back-N se maneja principalmente a través del tamaño de la ventana deslizante. Aquí hay un resumen de cómo funciona:

    - **Ventana deslizante**: El emisor puede enviar hasta \(N\) paquetes sin esperar una confirmación, donde \(N\) es el tamaño de la ventana.
    - **Confirmaciones acumulativas**: El receptor envía ACKs para el último paquete recibido en orden, lo que permite al emisor deslizar su ventana y enviar nuevos paquetes.
    - **Retransmisión**: Si el emisor no recibe una confirmación antes de que expire el temporizador, retransmite todos los paquetes desde el primer paquete no confirmado hasta el último paquete enviado.

    Este mecanismo asegura que el emisor no sobrecargue al receptor con demasiados paquetes a la vez y que los datos se reciban en el orden correcto.

    - ¿Por qué no permitir un número ilimitado de tales paquetes?
    
    El control de flujo es una de las razones para imponer un límite en el emisor.

## Code

```python
import threading
import time
import random

class GoBackNProtocol:
    def __init__(self, N, timeout):
        self.N = N  # Window size
        self.timeout = timeout  # Timeout duration
        self.base = 0  # Base of the window
        self.next_seq_num = 0  # Next sequence number to be sent
        self.lock = threading.Lock()
        self.timer = None
        self.packets = []  # List to store packets

    def send_packet(self, packet):
        with self.lock:
            if self.next_seq_num < self.base + self.N:
                self.packets.append(packet)
                print(f"Sending packet {self.next_seq_num}")
                self.next_seq_num += 1
                if self.base == self.next_seq_num - 1:
                    self.start_timer()
            else:
                print("Window is full, cannot send packet")

    def start_timer(self):
        if self.timer:
            self.timer.cancel()
        self.timer = threading.Timer(self.timeout, self.timeout_handler)
        self.timer.start()

    def timeout_handler(self):
        with self.lock:
            print("Timeout, resending packets")
            for i in range(self.base, self.next_seq_num):
                print(f"Resending packet {i}")
            self.start_timer()

    def receive_ack(self, ack_num):
        with self.lock:
            if ack_num >= self.base:
                self.base = ack_num + 1
                print(f"Received ACK for packet {ack_num}")
                if self.base == self.next_seq_num:
                    self.timer.cancel()
                else:
                    self.start_timer()

# Example usage
N = 4
timeout = 5
gbn = GoBackNProtocol(N, timeout)

# Simulate sending packets
for i in range(10):
    gbn.send_packet(f"Packet {i}")
    time.sleep(random.uniform(0.5, 1.5))

# Simulate receiving ACKs
for i in range(10):
    time.sleep(random.uniform(0.5, 2))
    gbn.receive_ack(i)
```

Hader:
En una implementación práctica, podrías incluir estos campos adicionales en el encabezado del paquete para mejorar la funcionalidad. Aquí tienes un resumen de cómo podrías estructurarlo:

- Número de Secuencia: Para mantener el orden de los paquetes.
- Datos: El contenido del paquete.
- EOF: Para indicar el final del archivo.
- FIN: Para indicar el cierre de la conexión.


## Referencias
Vamos a abordar tus preguntas una por una.

### Movimiento de la Ventana en Go-Back-N y Selective Repeat

#### Go-Back-N (GBN)

En Go-Back-N, la ventana deslizante se mueve solo cuando se recibe un ACK para el primer paquete no confirmado en la ventana. Aquí tienes un ejemplo:

1. **Envío de Paquetes**: El emisor envía los paquetes 0, 1, 2 y 3.
2. **Recepción de ACKs**:
   - Si se recibe ACK 0, la ventana se desliza y ahora incluye los paquetes 1, 2, 3 y 4.
   - Si el paquete 1 se pierde y no se recibe ACK 1, la ventana no se desliza hasta que se reciba el ACK 1.
3. **Retransmisión**: Si el temporizador expira para el paquete 1, el emisor retransmite los paquetes 1, 2 y 3.

En resumen, en GBN, la ventana se desliza solo cuando se recibe un ACK para el primer paquete no confirmado, y no se envían nuevos paquetes hasta que se deslice la ventana.

#### Selective Repeat (SR)

En Selective Repeat, la ventana deslizante se mueve de manera más flexible. Aquí tienes un ejemplo:

1. **Envío de Paquetes**: El emisor envía los paquetes 0, 1, 2 y 3.
2. **Recepción de ACKs**:
   - Si se recibe ACK 0, la ventana se desliza y ahora incluye los paquetes 1, 2, 3 y 4.
   - Si el paquete 1 se pierde pero se reciben los paquetes 2 y 3, el emisor puede enviar el paquete 4 y 5, ya que la ventana se desliza para cada ACK recibido.
3. **Retransmisión**: Si el temporizador expira para el paquete 1, el emisor retransmite solo el paquete 1.

En resumen, en SR, la ventana se desliza para cada ACK recibido, permitiendo enviar nuevos paquetes incluso si algunos paquetes anteriores no han sido confirmados.

### Diferencias entre Go-Back-N y Selective Repeat

1. **Manejo de Paquetes Fuera de Orden**:
   - **Go-Back-N**: El receptor descarta los paquetes fuera de orden y solo acepta paquetes en orden secuencial.
   - **Selective Repeat**: El receptor acepta y almacena paquetes fuera de orden, permitiendo una mayor eficiencia.

2. **Retransmisión**:
   - **Go-Back-N**: Si un paquete se pierde, el emisor retransmite ese paquete y todos los paquetes enviados después de él.
   - **Selective Repeat**: Si un paquete se pierde, el emisor retransmite solo ese paquete específico.

3. **Ventana Deslizante**:
   - **Go-Back-N**: La ventana se desliza solo cuando se recibe un ACK para el primer paquete no confirmado.
   - **Selective Repeat**: La ventana se desliza para cada ACK recibido, permitiendo enviar nuevos paquetes más rápidamente.

4. **Complejidad**:
   - **Go-Back-N**: Es más simple de implementar pero puede ser menos eficiente en redes con alta latencia o alta tasa de errores.
   - **Selective Repeat**: Es más complejo de implementar debido a la necesidad de manejar buffers y temporizadores individuales, pero es más eficiente en condiciones adversas.

### Resumen

- **Go-Back-N**: Más simple, retransmite múltiples paquetes en caso de pérdida, ventana se desliza solo con ACKs acumulativos.
- **Selective Repeat**: Más eficiente, retransmite solo paquetes específicos perdidos, ventana se desliza con cada ACK recibido.

¿Te gustaría profundizar en algún otro aspecto de estos protocolos o ver un ejemplo de código para Selective Repeat?

## Problemas con Números de Secuencia Bajos y Cíclicos en GBN y SR

Cuando se utilizan números de secuencia bajos y cíclicos en los protocolos Go-Back-N (GBN) y Selective Repeat (SR), pueden surgir problemas de sincronización entre las ventanas del emisor y del receptor. Vamos a desglosar estos problemas y cómo afectan a cada protocolo.

### Ejemplo y Problemas

Supongamos un rango de números de secuencia de 0 a 3 (4 números de secuencia) y un tamaño de ventana de 3. Aquí hay dos escenarios que ilustran los problemas:

1. **Escenario 1**: Los ACKs para los tres primeros paquetes se pierden y el emisor los retransmite.
2. **Escenario 2**: Los ACKs para los tres primeros paquetes se entregan correctamente, y el emisor envía los siguientes paquetes con números de secuencia 3, 0 y 1.

En ambos escenarios, el receptor no puede diferenciar entre una retransmisión del primer paquete y una transmisión inicial del quinto paquete debido a la reutilización de los números de secuencia.

### Problemas Específicos

1. **Confusión de Paquetes**: El receptor puede confundir una retransmisión de un paquete anterior con un nuevo paquete debido a la reutilización de los números de secuencia. Esto puede llevar a la entrega incorrecta de datos.

2. **Desincronización de Ventanas**: La falta de sincronización entre las ventanas del emisor y del receptor puede causar que el receptor acepte paquetes fuera de orden o descarte paquetes válidos.

### Tamaño de la Ventana

Para evitar estos problemas, el tamaño de la ventana debe ser menor o igual a la mitad del tamaño del espacio de números de secuencia. Esto asegura que los números de secuencia no se reutilicen dentro de la misma ventana de transmisión, evitando la confusión entre paquetes nuevos y retransmitidos.

### Justificación Matemática

Si $ k $) es el número de bits para el número de secuencia, el rango de números de secuencia es de $ 0 $ a $ 2^k - 1$. El tamaño de la ventana debe ser $ \leq \frac{2^k}{2} $ para evitar la confusión. Esto se debe a que, con un tamaño de ventana mayor, los números de secuencia se reutilizarían dentro de la misma ventana, causando los problemas mencionados.

### Ejemplo de Tamaño de Ventana

- Si \( k = 3 \) (3 bits para el número de secuencia), el rango de números de secuencia es de 0 a 7.
- El tamaño de la ventana debe ser $ \leq \frac{8}{2} = 4$.

### Resumen

- **Problemas con Números de Secuencia Bajos y Cíclicos**: Confusión de paquetes y desincronización de ventanas.
- **Solución**: El tamaño de la ventana debe ser menor o igual a la mitad del tamaño del espacio de números de secuencia.

¿Te gustaría profundizar en algún otro aspecto de estos protocolos o tienes alguna otra pregunta?