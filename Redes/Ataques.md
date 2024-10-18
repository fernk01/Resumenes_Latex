# Tipos de ataques de red
Ataque de denegación de servicio (DoS, **Denial-of-Service**). La mayoría de los ataques DoS en Internet pueden clasificarse dentro de una de las tres categorías siguientes:

- **Ataque de vulnerabilidad**. Este ataque implica el envío de unos pocos mensajes especialmente diseñados a una aplicación o sistema operativo vulnerable que esté ejecutándose en un host objetivo. Si se envía la secuencia de paquetes correcta a una aplicación o un sistema operativo vulnerables, el servicio puede detenerse o, lo que es peor, el host puede sufrir un fallo catastrófico.
- **Inundación del ancho de banda**. El atacante envía una gran cantidad de paquetes al host objetivo, de modo que se inunda el enlace de acceso del objetivo, impidiendo que los paquetes legítimos puedan alcanzar al servidor.
- **Inundación de conexiones**. El atacante establece un gran número de conexiones TCP completamente abiertas o semi-abiertas (las conexiones TCP se estudian en el Capítulo 3) con el host objetivo. El host puede llegar a atascarse con estas conexiones fraudulentas, impidiéndose así que acepte las conexiones legítimas.


Suplantacion de IP (IP Spoofing). La suplantación de IP es una técnica en la que un atacante envía paquetes IP con una dirección IP falsificada. El atacante puede hacerse pasar por otra máquina o red, lo que le permite enviar paquetes maliciosos sin ser detectado. La suplantación de IP se utiliza a menudo en ataques de denegación de servicio (DoS) y en otros tipos de ataques maliciosos.