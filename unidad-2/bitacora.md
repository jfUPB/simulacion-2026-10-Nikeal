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

### Actividad 7

1. ¿Cuál es el concepto del marco Motion 101 y cómo se interpreta geométricamente?

El marco **Motion 101** es una forma estructurada de describir el movimiento usando **vectores**, basada en tres componentes fundamentales:

* **Posición** → dónde está el objeto
* **Velocidad** → hacia dónde y qué tan rápido se mueve
* **Aceleración** → cómo cambia la velocidad

Conceptualmente, Motion 101 establece que el movimiento ocurre en capas:
la aceleración modifica la velocidad, y la velocidad modifica la posición.

Geométricamente:

* La **posición** es un punto en el espacio.
* La **velocidad** es un vector que indica la dirección y magnitud del desplazamiento desde ese punto.
* La **aceleración** es un vector que cambia la longitud y/o dirección del vector velocidad.

Esto permite entender el movimiento como una **suma progresiva de vectores**, donde cada frame representa una integración en el tiempo.

2. ¿Cómo se aplica Motion 101 en el ejemplo?

En el ejemplo propuesto, Motion 101 se aplica de la siguiente manera:

1. Se inicializan tres vectores:

   * `position` representa la ubicación del objeto.
   * `velocity` comienza en cero, indicando que el objeto parte en reposo.
   * `acceleration` tiene un valor constante, lo que provoca un cambio continuo en la velocidad.

2. En cada actualización (`update()`):

   * La aceleración se suma a la velocidad, haciendo que esta aumente progresivamente.
   * La velocidad se limita con `topSpeed` para evitar valores excesivos.
   * La posición se actualiza sumando la velocidad.

3. El resultado visual es un objeto que comienza moviéndose lentamente y acelera de forma gradual hasta alcanzar una velocidad máxima, describiendo una trayectoria influenciada por la dirección de la aceleración.

Este ejemplo demuestra cómo el uso de aceleración produce un movimiento más natural que un desplazamiento constante, y cómo Motion 101 permite simular comportamientos físicos básicos sin necesidad de un motor de físicas.

### Actividad 8

1. Aceleración constante

- Observaciones:

* El objeto **aumenta su velocidad de manera uniforme**.
* El movimiento comienza lento y se vuelve progresivamente más rápido.
* La trayectoria es **predecible y estable**.
* Si la aceleración apunta siempre en la misma dirección, el objeto puede salirse rápidamente de la pantalla.

- Conclusión:

Este tipo de aceleración produce un movimiento **controlado y realista**, similar a la gravedad o a un empuje continuo.
Es ideal para entender la relación básica:

> aceleración → velocidad → posición

Aquí el “trickle-down effect” se observa con claridad.

2. Aceleración aleatoria

- Observaciones:

* El movimiento es **errático e impredecible**.
* La velocidad cambia constantemente en magnitud y dirección.
* El objeto parece “temblar”, “vibrar” o moverse de forma caótica.
* No hay una trayectoria clara ni repetible.

- Conclusión:

Aunque la aceleración sea caótica, el sistema sigue funcionando correctamente.
Esto demuestra que **no importa si la aceleración es lógica o absurda**:
el modelo físico sigue respondiendo.

Aquí se rompe un poco la “regla” del libro, porque:

* El movimiento deja de ser realista
* Pero se vuelve **expresivo y útil para efectos visuales**

3. Aceleración hacia el mouse

- Observaciones:

* El objeto **persigue el mouse** suavemente.
* Cuanto más lejos está el mouse, **mayor es la aceleración**.
* El movimiento parece **inteligente u orgánico**.
* Si no se limita la velocidad, el objeto puede oscilar o “pasarse” del objetivo.

## Bitácora de aplicación 

1. Describe el concepto de tu obra generativa. Explica el concepto de tu obra generativa, qué regla aplicaste para la aceleración y por qué, si fue una decisión de diseño, o qué te evoca, si fue una exploración artística.

La obra es un campo sensible en movimiento, habitado por entidades que no siguen trayectorias predeterminadas, sino que responden a fuerzas que las atraviesan. El movimiento no nace de una instrucción directa, sino de un equilibrio inestable entre atracción, deriva y ruptura. La aceleración es la que decide el desplazamiento donde a partir de pequeñas fuerzas (algunas constantes y otras no) emergen trayectorias fluidas, a veces armónicas o a veces caóticas.

La interacción del usuario no impone un control, sino que actúa como una presencia que altera el campo. El mouse introduce una tensión local que atrae o repele a los agentes, como si el espacio reaccionara a un gesto externo. Sin embargo, el sistema conserva su autonomía, resistiéndose a ser completamente dominado.

Esta obra es una exploración artística del movimiento como fenómeno vivo. Evoca organismos que se adaptan, memorias de desplazamientos pasados y fuerzas que nunca se muestran, pero siempre se sienten. No hay una forma final: solo un flujo continuo donde reglas simples, aplicadas a la aceleración, generan complejidad, ritmo y transformación constante.

2.El código de la aplicación.

```
let agents = [];
let total = 70;

function setup() {
  createCanvas(640, 400);
  colorMode(HSB, 360, 100, 100, 100);

  for (let i = 0; i < total; i++) {
    agents.push(new Agent());
  }
}

function draw() {
  background(0, 0, 100, 18);

  for (let a of agents) {
    a.update();
    a.edges();
    a.show();
  }
}

class Agent {
  constructor() {
    this.position = createVector(random(width), random(height));
    this.prev = this.position.copy();

    this.velocity = p5.Vector.random2D();
    this.acceleration = createVector(0, 0);

    this.mass = random(0.6, 2);
    this.maxSpeed = random(2, 4);

    this.tx = random(1000);
    this.ty = random(1000);

    this.size = this.mass * 3;
    this.hue = random(360);
  }

  // Capítulo 2: fuerzas
  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.prev = this.position.copy();

    // Fuerza 1: atracción / repulsión al mouse
    let mouse = createVector(mouseX, mouseY);
    let dir = p5.Vector.sub(mouse, this.position);
    let d = dir.mag();

    if (d < 180) {
      dir.normalize();
      let strength = map(d, 0, 180, 0.6, 0);

      // Click invierte la fuerza (decisión interactiva)
      if (mouseIsPressed) strength *= -1;

      dir.mult(strength);
      this.applyForce(dir);
    }

    // Fuerza 2: campo de flujo (Perlin)
    let angle = noise(this.tx, this.ty) * TWO_PI * 2;
    let flow = p5.Vector.fromAngle(angle);
    flow.mult(0.15);
    this.applyForce(flow);

    // Fuerza 3: fricción (estabiliza)
    let friction = this.velocity.copy();
    friction.mult(-1);
    friction.normalize();
    friction.mult(0.03);
    this.applyForce(friction);

    // Evento raro (mutación)
    if (random(1) < 0.008) {
      let burst = p5.Vector.random2D();
      burst.mult(random(1, 3));
      this.applyForce(burst);

      // Mutación visual
      this.hue = random(360);
      this.mass = random(0.6, 2);
      this.size = this.mass * 3;
    }

    // Motion 101
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxSpeed);
    this.position.add(this.velocity);

    // Reset aceleración (Cap. 2)
    this.acceleration.mult(0);

    this.tx += 0.01;
    this.ty += 0.01;
  }

  edges() {
    if (this.position.x > width) this.position.x = 0;
    if (this.position.x < 0) this.position.x = width;
    if (this.position.y > height) this.position.y = 0;
    if (this.position.y < 0) this.position.y = height;

    this.prev = this.position.copy();
  }

  show() {
    // Trazo de dirección
    stroke(this.hue, 60, 80, 60);
    strokeWeight(1);
    line(this.prev.x, this.prev.y, this.position.x, this.position.y);

    // Núcleo
    stroke(this.hue, 80, 80, 90);
    strokeWeight(this.size);
    point(this.position.x, this.position.y);
  }
}
```

3. Un enlace al proyecto en el editor de p5.js.

https://editor.p5js.org/Nikeal/sketches/3M2w5NbBa

Selecciona capturas de pantalla representativas de tu pieza de arte generativa.



## Bitácora de reflexión







