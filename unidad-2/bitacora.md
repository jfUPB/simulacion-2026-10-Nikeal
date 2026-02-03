# Unidad 2

## Bitácora de proceso de aprendizaje

### Actividad 2

1. ¿Cómo funciona la suma dos vectores en p5.js?

- La suma de vectores se realiza usando el método .add() del objeto p5.Vector.
- El resultado es que la posición se actualiza desplazándose en la dirección y magnitud indicadas por la velocidad. Conceptualmente, esto significa que la velocidad modifica la posición en cada frame, produciendo movimiento.
- La suma de vectores en p5.js no crea un nuevo vector por defecto, sino que modifica el vector original, en este caso position.

2. ¿Por qué esta línea position = position + velocity; no funciona?

- No funciona porque position y velocity no son números, sino objetos de tipo p5.Vector. En JavaScript, el operador + solo sabe sumar:

- números
- strings

Pero no sabe cómo sumar objetos complejos como vectores. Por eso, p5.js proporciona métodos específicos como:

- add()
- sub()
- mult()
- div()

Estos métodos saben cómo operar correctamente sobre las componentes internas (x e y) de un vector.

### Actividad 3

- ¿Qué tuviste que hacer para hacer la conversión propuesta?
  
- Escribe el código que utilizaste para resolver el ejercicio.

## Bitácora de aplicación 



## Bitácora de reflexión
