# Unidad 5
## Bitácora de proceso de aprendizaje

### Actividad 1

🔹 Capa de comportamiento

¿Qué propiedades tiene cada partícula? Clasifícalas: ¿Cuáles definen su estado físico? ¿Cuáles su estado vital?
Cada partícula tiene propiedades que se pueden dividir en dos tipos. El estado físico incluye la posición, la velocidad y la aceleración, que son las que controlan cómo se mueve la partícula. Por otro lado, el estado vital está representado por el lifespan, que indica cuánto tiempo va a existir antes de desaparecer.

¿Qué condición determina que una partícula “muere”? ¿Es una muerte instantánea o gradual?
Una partícula muere cuando su lifespan es menor que cero. No es una muerte instantánea, sino gradual, porque el valor del lifespan va disminuyendo poco a poco en cada frame, haciendo que la partícula se desvanezca antes de desaparecer.

¿Cómo se actualiza la partícula en cada frame? Identifica el patrón motion 101 dentro de la partícula.
En cada frame, la partícula se actualiza siguiendo el patrón de Motion 101. Primero la aceleración se suma a la velocidad, luego la velocidad se suma a la posición y después la aceleración se reinicia. Además, el lifespan disminuye constantemente, lo que hace que la partícula envejezca mientras se mueve.

🔹 Capa de estructura

¿Quién crea las partículas? ¿En qué momento?
Las partículas son creadas desde el programa principal, generalmente dentro de la función draw(), por lo que se generan continuamente en cada frame.

¿Quién decide cuándo eliminar una partícula del array?
El sistema principal es el que decide cuándo eliminar una partícula del array. La partícula solo indica si está muerta, pero no se elimina por sí sola.

¿Por qué se recorre el array en orden inverso para eliminar? ¿Qué pasaría si no se hiciera así?
Se recorre en orden inverso porque al eliminar un elemento, los índices del array cambian. Si se recorriera hacia adelante, algunas partículas podrían saltarse y no ser evaluadas correctamente, lo que causaría errores en el sistema.

Si no eliminaras nunca las partículas, ¿Qué pasaría con la memoria y el rendimiento?
Si no se eliminaran las partículas, el array crecería sin límite. Esto haría que el programa consuma cada vez más memoria y que el rendimiento disminuya, provocando una caída en el frame rate y haciendo que el sistema se vuelva más lento.

🔹 Capa de visualización

¿Qué elementos visuales usa para representar una partícula?
Cada partícula se representa visualmente como un círculo con color y transparencia.

¿Cómo se conecta el “tiempo de vida” con la apariencia visual?
El tiempo de vida se usa para controlar la transparencia. A medida que el lifespan disminuye, la partícula se vuelve más transparente hasta desaparecer.

### aCTIVIDAD 2

## Bitácora de aplicación 


## Bitácora de reflexión
