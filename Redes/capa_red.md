La función principal de la capa de red es engañosamente simple: transporta paquetes desde un host emisor a un host receptor.
- Reenvío (forwarding): Mover paquetes de la entrada a la salida. prohibirse la reescritura de la dirección de destino o bien el paquete puede duplicarse y enviarse a través de múltiples enlaces de salida.
- Enrutamiento (routing): Determinar la ruta que los paquetes deben seguir desde el emisor hasta el receptor.


El reenvío hace referencia a la acción local que realiza un router al transferir un paquete desde una interfaz de un enlace de entrada a una interfaz del enlace de salida apropiado. Se implementa normalmente en hardware.

El enrutamiento hace referencia al proceso que realiza la red en conjunto para determinar las rutas extremo a extremo que los paquetes siguen desde el origen al destino. El enrutamiento tiene lugar con escalas de tiempo mucho más largas (normalmente de segundos) y, como veremos, suele implementarse en software.