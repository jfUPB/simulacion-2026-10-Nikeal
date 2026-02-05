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

1. ¿Qué tuviste que hacer para hacer la conversión propuesta?

- Para convertir el walker a vectores tuve que reemplazar las variables individuales de posición (x y y) por un solo vector de posición. En lugar de modificar directamente x o y, modifique un vector de desplazamiento que luego se suma a la posición usando operaciones vectoriales.
- Específicamente: Reemplacé this.x y this.y por this.position, que es un p5.Vector. En lugar de incrementar o decrementar valores manualmente, creé un vector step y utilicé add() para sumar el desplazamiento a la posición
- El comportamiento aleatorio del walker se mantuvo, pero expresado mediante vectores
- Esta conversión hace el código más claro y prepara el sistema para trabajar con velocidad, aceleración y fuerzas.
  
2. Escribe el código que utilizaste para resolver el ejercicio:

```
let walker;

function setup() {
  createCanvas(640, 240);
  // Creating the Walker object!
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.position = createVector(width / 2, height / 2);
  }

  step() {
    let r = random(1);
    let step = createVector(0, 0);

    // 40% de probabilidad de moverse a la derecha
    if (r < 0.4) {
      step.x = 1;
    } else if (r < 0.6) {
      step.x = -1;
    } else if (r < 0.8) {
      step.y = 1;
    } else {
      step.y = -1;
    }

    // Suma vectorial: posición = posición + desplazamiento
    this.position.add(step);
  }

  show() {
    stroke(0);
    point(this.position.x, this.position.y);
  }
}
```

### Actividad 4

1. ¿Qué resultado esperas obtener en el programa anterior?

- Espero que el vector position cambie sus valores después de llamar a la función playingVector(position). Aunque la función recibe el vector como parámetro, al modificar sus componentes (x e y), estos cambios deberían reflejarse fuera de la función.

2. ¿Qué resultado obtuviste? 
Antes de llamar a la función:

[6, 9]


Después de llamar a la función playingVector(position):

[20, 30]


3. Recuerda los conceptos de paso por valor y paso por referencia en programación.

Paso por valor:
Se pasa una copia del dato. Los cambios dentro de la función no afectan a la variable original.

Paso por referencia:
Se pasa una referencia al objeto original. Los cambios dentro de la función sí afectan al objeto fuera de la función.

4. ¿Qué tipo de paso se está realizando en el código?

En este código se está realizando un paso por referencia.

Esto ocurre porque los vectores (p5.Vector) son objetos, y cuando se pasan como parámetros a una función, no se copia el vector, sino que se pasa una referencia al mismo objeto en memoria.

Por eso, al modificar v.x y v.y dentro de la función, también se modifica position.

5. ¿Qué aprendiste?

Aprendí que los vectores en p5.js se comportan como objetos y se pasan por referencia. Esto significa que al enviarlos a una función, cualquier modificación afecta directamente al vector original. Este comportamiento es muy importante, ya que permite modificar posiciones, velocidades o fuerzas desde funciones externas, pero también requiere cuidado para evitar cambios no deseados en sistemas complejos.

### Actividad 5

1. ¿Para qué sirve el método mag()? Nota que hay otro método llamado magSq(). ¿Cuál es la diferencia entre ambos? ¿Cuál es más eficiente?

- El método mag() sirve para obtener la magnitud o longitud de un vector, es decir, qué tan largo es. Matemáticamente, corresponde a la distancia desde el origen hasta el punto definido por el vector.
- El método magSq() devuelve el cuadrado de la magnitud. La diferencia es que mag() calcula una raíz cuadrada, mientras que magSq() no.
- magSq() es más eficiente computacionalmente, por lo que se usa cuando solo se necesita comparar longitudes relativas y no el valor exacto de la magnitud.

2. ¿Para qué sirve el método normalize()?

- El método normalize() convierte un vector en un vector unitario, es decir, un vector con la misma dirección pero con magnitud igual a 1. Esto es útil cuando se quiere trabajar solo con la dirección del vector sin que su longitud influya, por ejemplo al definir fuerzas, direcciones de movimiento o vectores de orientación.

3. Te encuentras con un periodista en la calle y te pregunta ¿Para qué sirve el método dot()? ¿Qué le responderías en un frase?

- El método dot() sirve para medir qué tan alineados están dos vectores entre sí.

4. El método dot() tiene una versión estática y una de instancia. ¿Cuál es la diferencia entre ambas?

- Versión de instancia:
Se llama desde un vector existente.

v1.dot(v2);


Calcula el producto punto entre v1 y v2.

Versión estática:
Se llama desde la clase p5.Vector.

p5.Vector.dot(v1, v2);


Hace exactamente lo mismo, pero sin depender de un vector específico como contexto.

5. Ahora el mismo periodista curioso de antes te pregunta si le puedes dar una intuición geométrica acerca del producto cruz. Entonces te pregunta ¿Cuál es la interpretación geométrica del producto cruz de dos vectores? Tu respuesta debe incluir qué pasa con la orientación y la magnitud del vector resultante.

El producto cruz de dos vectores genera un nuevo vector perpendicular a los dos vectores originales.

Orientación:
La dirección del vector resultante sigue la regla de la mano derecha, lo que significa que su orientación depende del orden de los vectores.

Magnitud:
La magnitud del vector resultante es proporcional al área del paralelogramo formado por los dos vectores.
Si los vectores son paralelos, el producto cruz es cero.

Geométricamente, el producto cruz mide cuánto “se separan” dos vectores en el espacio.

6. ¿Para que te puede servir el método dist()?

El método dist() sirve para calcular la distancia entre dos puntos o vectores.

Es especialmente útil para:
- detectar cercanía entre objetos
- colisiones simples
- zonas de influencia
- interacción con el mouse

7. ¿Para qué sirven los métodos normalize() y limit()?

- normalize()
Se usa para mantener solo la dirección del vector, con magnitud fija en 1.

- limit()
Se usa para limitar la magnitud máxima de un vector sin cambiar su dirección.

Ambos métodos son fundamentales para:

- controlar velocidad
- evitar movimientos extremos
- mantener estabilidad en sistemas de movimiento

### Actividad 6

1. El código que genera el resultado que te pedí.

```
let t = 0;
let dir = 1;

function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(220);

  let origin = createVector(100, 300);

  let v1 = createVector(250, 0);    // Vector rojo (horizontal)
  let v2 = createVector(200, -250); // Vector verde (diagonal)

  // Interpolación entre v1 y v2
  let v3 = p5.Vector.lerp(v1, v2, t);

  drawArrow(origin, v1, color(255, 0, 0));
  drawArrow(origin, v2, color(0, 150, 0));
  drawArrow(origin, v3, color(150, 0, 150));

  // Animación del parámetro t
  t += 0.01 * dir;
  if (t > 1 || t < 0) {
    dir *= -1;
  }
}

function drawArrow(base, vec, myColor) {
  push();
  stroke(myColor);
  strokeWeight(3);
  fill(myColor);
  translate(base.x, base.y);
  line(0, 0, vec.x, vec.y);
  rotate(vec.heading());
  let arrowSize = 10;
  translate(vec.mag() - arrowSize, 0);
  triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
  pop();
}
```

2. ¿Cómo funciona lerp() y lerpColor().

- El método lerp() realiza una interpolación lineal entre dos valores o vectores.

En el caso de vectores:

p5.Vector.lerp(v1, v2, t)


v1 → vector inicial

v2 → vector final

t → valor entre 0 y 1

Interpretación:

t = 0 → resultado es v1

t = 1 → resultado es v2

t = 0.5 → punto exactamente a la mitad

Geométricamente, lerp() devuelve un vector que se mueve sobre la línea que conecta ambos vectores.

- lerpColor() funciona igual, pero con colores:

lerpColor(colorA, colorB, t)

Interpola gradualmente entre dos colores

3. ¿Cómo se dibuja una flecha usando drawArrow()?
El método drawArrow() dibuja una flecha combinando transformaciones geométricas:

1. translate(base.x, base.y)
Mueve el origen del sistema de coordenadas al punto de inicio del vector.

2. line(0, 0, vec.x, vec.y)
Dibuja el cuerpo de la flecha siguiendo el vector.

3. rotate(vec.heading())
Rota el sistema para que la punta apunte en la dirección del vector.

4. vec.mag()
Usa la magnitud del vector para colocar la punta al final.

5. triangle(...)
Dibuja la cabeza de la flecha.

6. push() / pop()
Aísla las transformaciones para no afectar otros dibujos.

## Bitácora de aplicación 



## Bitácora de reflexión




