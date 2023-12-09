# File System
## 
El superbloque de un sistema de archivos indica que el (3)        inodo correspondiente al directorio raiz es el #43 . En la siguiente secuencia de comandos y siempre partiendo de ese directorio raiz, se pide indiccar  la cantidad de inodos y bloquies de datos a los que se precisa acceder  (leer) para resolver la ruta dada a cat(1) o stat(1)
```
# mkdir /dir  /dir/s /dir/s/w
# touch /dir/x /dir/s/y
# stat /dir/s/w/x
# stat /dir/s/y
```
indicar al cantidad Inodos y la cantidad de Bloques de datos para:
1. `# stat /dir/s/w/x`
2. `# stat /dir/s/y`

```
# ln /dir/s/x  /dir/h
# ln -s /dir/s/y /dir/y
# cat /dir/h
# cat /dir/y
```
indicar al cantidad Inodos y la cantidad de Bloques de datos para:
3. "# cat /dir/h`
4. `# cat /dir/y`

Ayuda: todos los directorios ocupan un bloque. La idea es que describan como stat llega a los archivos.

me explicas paso a paso?

Respuesta:
Claro, te explico paso a paso cómo se accede a los inodos y bloques de datos para cada comando:

1. `# stat /dir/s/w/x`

Se accede al inodo del directorio raíz (#43).
Se accede al inodo del directorio "dir" (1 inodo).
Se accede al inodo del directorio "s" (1 inodo).
Se accede al inodo del directorio "w" (1 inodo).
No se encuentra el archivo "x" en el directorio "w", por lo que se devuelve un error.
Total de inodos: 4 (directorios) + 0 (archivo) = 4 inodos.
Total de bloques de datos: 4 (directorios) + 0 (archivo) = 4 bloques.

2. `# stat /dir/s/y`

Se accede al inodo del directorio raíz (#43).
Se accede al inodo del directorio "dir" (1 inodo).
Se accede al inodo del directorio "s" (1 inodo).
Se accede al inodo del archivo "y" (1 inodo).
Total de inodos: 3 (directorios) + 1 (archivo) = 4 inodos.
Total de bloques de datos: 3 (directorios) + 1 (archivo) = 4 bloques.

3. `# cat /dir/h`

Se accede al inodo del directorio raíz (#43).
Se accede al inodo del directorio "dir" (1 inodo).
Se accede al inodo del archivo "h" (1 inodo), que es un hardlink al archivo "x" en el directorio "s".
Se accede al bloque de datos del archivo "x" (1 bloque), que es el mismo que el del archivo "h".
Se muestra el contenido del archivo "x".
Total de inodos: 2 (directorios) + 1 (archivo) = 3 inodos.
Total de bloques de datos: 2 (directorios) + 1 (archivo) = 3 bloques.

4. `# cat /dir/y`

Se accede al inodo del directorio raíz (#43).
Se accede al inodo del directorio "dir" (1 inodo).
Se accede al inodo del archivo "y" (1 inodo), que es un symlink al archivo "y" en el directorio "s".
Se accede al bloque de datos del archivo "y" (1 bloque), que contiene la ubicación del archivo original.
Se sigue la ubicación y se accede al inodo del directorio "s" (1 inodo).
Se accede al inodo del archivo "y" en el directorio "s" (1 inodo).
Se accede al bloque de datos del archivo "y" en el directorio "s" (1 bloque).
Se muestra el contenido del archivo "y".
Total de inodos: 3 (directorios) + 2 (archivos) = 5 inodos.
Total de bloques de datos: 3 (directorios) + 2 (archivos) = 5 bloques.