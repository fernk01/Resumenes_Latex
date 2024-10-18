# Latencia

## Tiempo de inserción
Tiempo que demora el páquete en ser insertado en el enlace.

- **L (largo del paquete)**: Representa la longitud del paquete de datos en bits. Cuanto más grande sea el paquete, más tiempo tomará insertarlo en el enlace.
- **R (velocidad de serialización)**: Indica la velocidad a la que los datos se transmiten en el enlace. Se mide en bits por segundo (bps). Si la velocidad de serialización es alta, los datos se insertarán más rápidamente.

$$ t_{ins} = \frac{L}{R} $$

Ejemplo: <br>
Supongamos que tenemos un paquete de datos de **12,000 bits** (equivalente a **1,500 bytes**) y una velocidad de serialización de **1 Gbps (1,000 Mbps)**:

$$
\text{Tiempo de inserción} = \frac{12,000 \text{ bits}}{1,000 \text{ Mbps}} = 0.000012 \text{ segundos}
$$

Por lo tanto, el tiempo de inserción para un paquete de 12,000 bits a una velocidad de 1 Gbps es aproximadamente **0.000012 segundos**.


## Tiempo de propagación:
Tiempo que demora el paquete en propagarse por el enlace de un router al próximo.

- Velocidad del medio.
    - Aire: Velocidad de la luz ($3 \times 10^8 m/s$).
    - Fibra óptica, Cobre, Coaxial: $2/3$ de la velocidad de la luz.
- Distancia entre los entremos del enlace.

$$ t_{prop} = \frac{d}{v} $$

## Diferencia entre tiempo de propagación y tiempo de inserción
- Inserción
    - Tiempo para insertar el paquete en el canal
    - Independiente de la distancia entre hosts
- Propagación
    - Tiempo para atravesar el canal
    - Independiente de la velocidad de serialización

Analogía: <br> 
Si pensamos en el recorrido de un automóvil desde un punto A a un punto B, el tiempo de inserción sería el tiempo que el automóvil tarda en entrar a la autopista, mientras que el tiempo de propagación sería el tiempo que el automóvil tarda en recorrer la distancia entre A y B a una velocidad constante.

## Tiempo de procesamiento
Es el tiempo que requiere el procesamiento del paquete en los routers.

- Causas
    - Leer el header
    - Tomar la decisión de por cual enlace se debe enviar
- Orden de magnitud = ns - μs

## Tiempo de encolado
Tiempo que espera paquete en el router desde que arriba hasta que es finalmente transmitido

- ¿De que depende?
    - Tasa de ocupación del router
    - Es decir del tamaño de la cola
    - A mayor tráfico, mayor tiempo de encolado

## Tiempo de encolado y pérdidas
- ¿El tiempo de encolado es constante? 
    - ➔ No, varía con el tráfico (aleatorio)
- Pensemos
    - ➔ $L$: Largo del paquete
    - ➔ $a$: tasa de arribo promedio de paquetes
    - ➔ $R$: velocidad de serialización
- Si $L \cdot a > R$
    - ➔ Están arribando más datos de los que el router puede enviar
    - ➔ Esto quiere decir que la cola crece (y crece)
    - ➔ Se llenan los buffers
    - ➔ Se descartan paquetes

- Deseo del dueño del SW (sofware)
    - Que esté Tx (transmisión) TODO el tiempo
- Problema
    - $L \cdot a = R$ —> la cola desborda
- Solución
    - $L \cdot a < R$ (subutilización)

## Round-Trip Time (RTT)
Tiempo que tarda un paquete de datos enviado desde un emisor en volver al mismo emisor habiendo pasado por el receptor de destino.

## Throughput
- Cantidad de datos que se pueden transmitir a través de una red en un período de tiempo determinado.
- Tasa a la que se transfieren bits entre trasmisor/receptor $[b/s]$

## ¿Qué diferencia hay entre el ancho de banda y el throughput?
El ancho de banda y el throughput son dos conceptos relacionados pero distintos en el contexto de redes de comunicación. Aquí te explico la diferencia:

### Ancho de Banda
- **Definición**: El ancho de banda es la capacidad máxima de transmisión de datos de una red o enlace en un período de tiempo determinado.
- **Medición**: Se mide en bits por segundo (bps), kilobits por segundo (kbps), megabits por segundo (Mbps), etc.
- **Analogía**: Piensa en el ancho de banda como el ancho de una autopista. Cuanto más ancha sea la autopista, más coches (datos) pueden circular al mismo tiempo.

### Throughput
- **Definición**: El throughput es la cantidad real de datos que se transmiten exitosamente a través de la red en un período de tiempo determinado.
- **Medición**: También se mide en bits por segundo (bps), kilobits por segundo (kbps), megabits por segundo (Mbps), etc.
- **Analogía**: Si el ancho de banda es el ancho de la autopista, el throughput es la cantidad de coches que realmente llegan a su destino en un tiempo determinado. Puede verse afectado por varios factores como la congestión de la red, errores de transmisión, etc.

### Diferencias Clave
- **Capacidad vs. Realidad**: El ancho de banda es la capacidad teórica máxima, mientras que el throughput es la cantidad real de datos transmitidos.
- **Factores de Influencia**: El throughput puede ser menor que el ancho de banda debido a factores como la latencia, la pérdida de paquetes, la congestión de la red, etc.

### Ejemplo
Si tienes una conexión a Internet con un ancho de banda de 100 Mbps, eso significa que, teóricamente, puedes transmitir hasta 100 megabits de datos por segundo. Sin embargo, debido a la congestión de la red, la calidad de la conexión, y otros factores, el throughput real podría ser solo 80 Mbps.

Espero que esto aclare la diferencia entre ancho de banda y throughput.

## Jitter
Variación en el tiempo que tarda un paquete de datos en viajar desde su origen hasta su destino. Mide la fluctuación del retardo.

## Bandwitdth
Bandwitdth es la cantidad de datos que se pueden transmitir en un período de tiempo determinado. Se mide en bits por segundo (bps).
### Throughput vs Bandwidth
- **Throughput**: Cantidad real de datos transmitidos con éxito en un período de tiempo determinado.
- **Bandwidth**: Capacidad máxima de transmisión de datos de una red o enlace en un período de tiempo determinado.

