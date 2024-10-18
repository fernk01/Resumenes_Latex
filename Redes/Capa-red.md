Funciones clave de la capa red:
1. **Reenvío (forwarding)**: Mover paquetes entre los puertos de entrada y salida del router.
2. **Enrutamiento (routing)**: Determinar la ruta de los pauqetes del origen al destino.
    - Algoritmos de enrutamiento: Determinan la mejor ruta para un paquete.

Analogía:
- Enrutamiento: proceso de planificar el viaje desde el origen al destino.
- Reenvío: proceso de pao por un cruce de carreteras.

#

## Redes de datagramas
- SIn establimiento de la llamada en la capa de red
- Routers: no mantinen estado de la conexión
    - Sin conexión entendida como conexíon de red.
- Los paquetes se reenvías utilizando la dirección del host de destino
    - los paquetes entre pares origen-destino pueden tomas diferentes rutas.

Tabla de reenvío: Es una tabla que contiene la dirección de destino y la interfaz de salida. 

En direcciones IPv4, la tabla de reenvío contiene la dirección de red y la máscara de subred. Tiene $32$ bits. $2^{32} = 4.3$ billones de entradas.

### Regla Coincidencia con el prefijo más largo
Es una regla que se utiliza para determinar la mejor ruta en una tabla de reenvío. Se selecciona la entrada con el prefijo más largo que coincida con la dirección de destino.

# Ruters
Un router tiene dos funciones clave:
- Ejecutar algoritmos/protocolos de enrutamiento (RIP, OSPF, BGP).
- Reenviar datagrmas desde enlaces (puerto) de entra a enlaces (puerto) de salida.

Entramado de conmutacíon: Es el proceso de mover paquetes de un puerto de entrada a un puerto de salida.
Procesador de enrutamiento: Es el componente que ejecuta los algoritmos de enrutamiento.