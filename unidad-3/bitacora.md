# Unidad 3

## Bitácora de proceso de aprendizaje

### Actividad 1

Después de ver la obra *Magnetosphere* y escuchar la charla de Robert Hodgin, me quedé con una sensación ambigua: por un lado, admiración profunda por la complejidad y belleza de los sistemas que construye; por otro, una inquietud sobre el momento histórico en el que estamos creando.

En Magnetosphere, lo que más me impacta no es solo el resultado visual, sino la evidencia de proceso. Se percibe que detrás hay exploración, iteración, errores, ajustes, obsesión. No es una imagen “generada” en el sentido superficial, sino el resultado de comprender fuerzas, sistemas dinámicos y comportamiento emergente. Hay física, pero también hay intención. Hay código, pero también hay sensibilidad.

En la charla, cuando Hodgin habla sobre hacer arte en la era de la inteligencia artificial, siento que toca algo incómodo: la facilidad con la que ahora podemos producir imágenes “impactantes” sin comprender realmente lo que está ocurriendo detrás. Me pregunto si estamos delegando no solo tareas, sino también procesos cognitivos. El asombro ya no surge del descubrimiento, sino del resultado inmediato. Consumimos sin fricción. Creamos sin atravesar la frustración. Es como si el proceso —que antes era el núcleo del aprendizaje— estuviera siendo comprimido.

Lo que más me resuena es la idea de que el valor no está únicamente en el producto final, sino en el recorrido. En escribir el código, equivocarse, ajustar fuerzas, ver cómo el sistema colapsa, entender por qué algo no funciona. Esa fricción es parte de la construcción de criterio y sensibilidad. Si eliminamos el esfuerzo, ¿qué queda del aprendizaje?

También pienso en el sistema económico que menciona implícitamente: producir más rápido, impactar más fuerte, destacar más. Pero ¿destacar para qué? ¿Ganar qué? En ese contexto, el arte generativo puede volverse una fábrica de efectos visuales vacíos. Sin embargo, cuando se entiende como investigación —como lo hace Hodgin— se convierte en una herramienta para explorar fenómenos complejos y preguntarse cosas más profundas.

Me inquieta la posibilidad de convertirnos en consumidores permanentes del algoritmo, esperando que nos entregue sorpresa sin haber construido las condiciones para sorprendernos. Si todo es inmediato, el asombro pierde densidad. Si todo es automático, el descubrimiento se vuelve superficial.

Tal vez la pregunta no es si vale la pena, sino cómo vale la pena. Quizá la respuesta esté en seguir programando fuerzas, sistemas y procesos desde la comprensión, no desde la inmediatez. En usar la tecnología como herramienta de exploración y no como sustituto del pensamiento.

Después de ver el trabajo de Hodgin, siento que hacer arte generativo no es producir imágenes, sino diseñar relaciones, tensiones y comportamientos. Y eso todavía exige atención, curiosidad y paciencia.

Tal vez ahí está la resistencia.

### Actividad 2

Al revisar nuevamente el marco Motion 101 y extenderlo para incluir fuerzas, entendí que estamos dando un paso conceptual muy importante. En la unidad anterior yo “inventaba” la aceleración directamente en cada frame. Ahora, en cambio, la aceleración ya no es una decisión arbitraria, sino el resultado de la suma de fuerzas.

Este cambio parece pequeño en el código, pero es enorme en términos de pensamiento.

Antes:

```
yo defino aceleración → cambia velocidad → cambia posición
```

Ahora:

```
fuerzas → generan aceleración → cambia velocidad → cambia posición
```

Es decir, ya no programo el movimiento directamente, sino que programo las causas del movimiento.

**Sobre la segunda ley de Newton**

La relación:

[
F = m \cdot a
]

me permite entender que la aceleración depende tanto de la fuerza aplicada como de la masa del objeto. En el mundo del arte generativo podríamos asumir masa = 1 para simplificar, pero cuando introducimos masa distinta en cada objeto, el comportamiento se vuelve más interesante. Objetos más pesados reaccionan menos; objetos más ligeros reaccionan más. Aparece diversidad dinámica.

Eso ya no es solo movimiento, es carácter.


**Sobre la suma de fuerzas**

Algo muy importante que comprendí es que la aceleración en cada frame es la sumatoria de todas las fuerzas:

```js
mover.applyForce(wind);
mover.applyForce(gravity);
```

Esto significa que las fuerzas no se reemplazan entre sí, se acumulan. El objeto no “elige” entre viento o gravedad; experimenta ambos simultáneamente. Esto permite sistemas mucho más ricos y complejos.

¿Por qué multiplicar la aceleración por cero?

```js
this.acceleration.mult(0);
```

Al principio parecía extraño, pero ahora entiendo que es fundamental.

La aceleración debe reiniciarse en cada frame porque:

* Las fuerzas solo actúan durante ese instante.
* En el siguiente frame se recalculan.
* Si no la reiniciamos, las fuerzas se acumularían infinitamente.

Es decir, la aceleración es un contenedor temporal de fuerzas. Se llena durante el frame, se aplica, y luego se vacía.

Si no la limpiamos, estaríamos simulando fuerzas permanentes sin control.

Sobre paso por referencia (algo muy importante)

Cuando vimos esto:

```js
force.div(10);
```

y entendimos que `force` es un objeto `p5.Vector`, comprendí que estamos trabajando con paso por referencia.

Eso significa que si modifico `force`, estoy modificando el objeto original que fue pasado como argumento. Esto puede causar errores inesperados.

Por eso la versión correcta es:

```js
let f = p5.Vector.div(force, 10);
this.acceleration.add(f);
```

Aquí se crea un nuevo vector, evitando alterar el original.

Este detalle técnico cambia completamente la estabilidad del sistema. Me hizo notar que no solo estamos trabajando con física conceptual, sino también con fundamentos profundos de programación.

Lo que más me impacta de esta unidad es que estamos pasando de dibujar movimiento a diseñar sistemas físicos simplificados. El movimiento ya no es decorativo, es consecuencia.

También me hace pensar que el arte generativo se vuelve más interesante cuando las reglas están bien fundamentadas. No es solo estética; es estructura, causa y efecto.

Ahora entiendo que trabajar con fuerzas es diseñar tensiones invisibles que luego se manifiestan visualmente. Y eso cambia la forma en que pienso el código: ya no como instrucciones lineales, sino como un ecosistema de influencias.

### Actividad 3

**Obra Generativa: Superficie Rugosa**

**Fuerza: Fricción**

Un conjunto de partículas se desliza por el espacio como si estuvieran sobre una superficie horizontal. Cuando entran en una zona específica (una “alfombra rugosa”), experimentan fricción más intensa y se desaceleran progresivamente.

La obra visualiza cómo la fricción no depende de la dirección del movimiento, sino que siempre actúa en sentido opuesto.

---

**Modelo físico**

Fricción básica:

[
F_f = - \mu \cdot \hat{v}
]

* Dirección opuesta a la velocidad.
* Magnitud constante (coeficiente μ).

**Fricción**

```js
let movers = [];

function setup() {
  createCanvas(700, 400);
  for (let i = 0; i < 20; i++) {
    movers.push(new Mover());
  }
}

function draw() {
  background(240);

  for (let m of movers) {
    let friction = m.velocity.copy();
    friction.mult(-1);
    friction.normalize();
    friction.mult(0.05);

    m.applyForce(friction);
    m.update();
    m.edges();
    m.show();
  }
}

class Mover {
  constructor() {
    this.position = createVector(random(width), random(height));
    this.velocity = p5.Vector.random2D().mult(random(2, 4));
    this.acceleration = createVector(0, 0);
    this.mass = 1;
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  edges() {
    if (this.position.x > width || this.position.x < 0)
      this.velocity.x *= -1;
    if (this.position.y > height || this.position.y < 0)
      this.velocity.y *= -1;
  }

  show() {
    fill(50);
    noStroke();
    circle(this.position.x, this.position.y, 10);
  }
}
```

---

**Obra Generativa: Sumergidos**

**Fuerza: Resistencia al aire / fluido (Drag)**

**Concepto**

Las partículas caen por gravedad. En la mitad inferior del lienzo hay “agua”. Cuando entran en esa zona, experimentan resistencia proporcional al cuadrado de su velocidad.

Visualmente:

* En el aire caen rápido.
* En el agua se ralentizan.

**Modelo físico**

[
F_d = -c \cdot v^2 \cdot \hat{v}
]

Depende de la magnitud de la velocidad al cuadrado.

**Resistencia**

```js
let movers = [];

function setup() {
  createCanvas(700, 400);
  for (let i = 0; i < 15; i++) {
    movers.push(new Mover());
  }
}

function draw() {
  background(220);

  // Dibujar agua
  fill(100, 150, 255, 150);
  noStroke();
  rect(0, height/2, width, height/2);

  for (let m of movers) {

    let gravity = createVector(0, 0.2 * m.mass);
    m.applyForce(gravity);

    if (m.position.y > height/2) {
      let speed = m.velocity.mag();
      let dragMagnitude = 0.02 * speed * speed;

      let drag = m.velocity.copy();
      drag.mult(-1);
      drag.normalize();
      drag.mult(dragMagnitude);

      m.applyForce(drag);
    }

    m.update();
    m.edges();
    m.show();
  }
}

class Mover {
  constructor() {
    this.mass = random(1, 3);
    this.position = createVector(random(width), random(50));
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  edges() {
    if (this.position.y > height) {
      this.position.y = 0;
      this.velocity.mult(0);
    }
  }

  show() {
    fill(0);
    circle(this.position.x, this.position.y, this.mass * 8);
  }
}
```

**Obra Generativa: Orbita Inestable**

Atracción gravitacional

**Concepto**

Un cuerpo central actúa como masa atractora.
Las partículas orbitan, pero debido a pequeñas variaciones iniciales, las órbitas nunca son perfectas.

Inspirado en gravedad universal.


**Modelo físico**

[
F = G \frac{m_1 m_2}{r^2}
]

Fuerza depende inversamente del cuadrado de la distancia.

*Atracción gravitacional*

```js
let movers = [];
let attractor;

function setup() {
  createCanvas(700, 450);
  attractor = new Attractor();

  for (let i = 0; i < 15; i++) {
    movers.push(new Mover());
  }
}

function draw() {
  background(10);

  attractor.show();

  for (let m of movers) {
    let force = attractor.attract(m);
    m.applyForce(force);

    m.update();
    m.show();
  }
}

class Attractor {
  constructor() {
    this.position = createVector(width/2, height/2);
    this.mass = 20;
    this.G = 1;
  }

  attract(m) {
    let force = p5.Vector.sub(this.position, m.position);
    let distance = constrain(force.mag(), 5, 25);

    force.normalize();
    let strength = (this.G * this.mass * m.mass) / (distance * distance);
    force.mult(strength);

    return force;
  }

  show() {
    fill(255, 150);
    circle(this.position.x, this.position.y, 30);
  }
}

class Mover {
  constructor() {
    this.position = createVector(random(width), random(height));
    this.velocity = p5.Vector.random2D();
    this.acceleration = createVector(0, 0);
    this.mass = random(1, 3);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    fill(200);
    circle(this.position.x, this.position.y, this.mass * 6);
  }
}
```

## Bitácora de aplicación 

1. Descripcion 
**Entropía de la Memoria**
- Es una obra generativa que simula un campo dinámico donde fragmentos de memoria flotan e interactúan constantemente. Cada partícula representa un recuerdo que no es estático, sino que está influenciado por fuerzas invisibles que determinan su movimiento. La historia que propongo en la obra es que la memoria nunca es completamente estable: tiende a agruparse, pero el tiempo(entropia) siempre introduce desorden. Existe una tensión constante entre atracción y repulsión, entre organización y caos.

- El sistema está construido bajo la segunda ley de Newton (F = m·a). La aceleración de cada partícula no se define directamente, sino que surge como resultado de la suma de fuerzas:

* Una fuerza de entropía (ruido) que introduce movimiento orgánico.
* Fuerzas colectivas de atracción y repulsión entre partículas.
* Resistencia del medio (drag), que amortigua el movimiento.
* Campos espaciales que modifican el comportamiento según la zona.
* Interacción del usuario, que actúa como una fuerza externa.

La forma global del sistema no está programada explícitamente; emerge de la interacción constante entre estas fuerzas.

Instrucciones:
- Click Izquierdo atrae
- Click Derecho repele momentaneamente
- F aumenta la gravedad

2.

```JS
// Arreglo que almacenará todas las partículas del sistema
let particulas = [];
// Número total de partículas que existirán en el entorno
let numParticulas = 400;
// Variable que activa la onda de choque (repulsión momentánea)
let shockwave = false;
// Vector que guarda el centro desde donde se expande la onda
let shockCenter;
// Número de frames que durará la onda expansiva
let shockFrames = 0;
// Multiplicador global que escala todas las fuerzas del sistema
let fuerzaGlobal = 1;
// Indica si el modo intenso (tecla F) está activo
let modoIntenso = false;

// SE EJECUTA UNA VEZ
function setup() {

  // Crea un canvas del tamaño completo de la ventana
  createCanvas(windowWidth, windowHeight);

  // Define el modo de color en HSB (tono, saturación, brillo, alfa)
  colorMode(HSB, 360, 100, 100, 100);

  // Crea todas las partículas iniciales
  for (let i = 0; i < numParticulas; i++) {
    particulas.push(new Particula());
  }

  // Desactiva el menú contextual del click derecho
  document.oncontextmenu = () => false;

  // Fondo inicial oscuro
  background(10);
}


// SE EJECUTA CADA FRAME
function draw() {

  // Fondo con transparencia para generar efecto de rastro
  background(10, 25);

  // Intensificación progresiva del entorno si F está presionada
  if (modoIntenso) {
    // Aumenta suavemente el multiplicador hacia 3.5
    fuerzaGlobal = lerp(fuerzaGlobal, 3.5, 0.08);
  } else {
    // Vuelve suavemente a la normalidad
    fuerzaGlobal = lerp(fuerzaGlobal, 1, 0.08);
  }

  // Recorremos cada partícula del sistema
  for (let p of particulas) {

    // FUERZA DE ENTROPÍA (Ruido Perlin)

    // Se genera un vector basado en ruido Perlin
    let fuerzaRuido = createVector(
      noise(p.pos.x * 0.005, p.pos.y * 0.005, frameCount * 0.01) - 0.5,
      noise(p.pos.y * 0.005, p.pos.x * 0.005, frameCount * 0.01) - 0.5
    );

    // Se escala por intensidad global
    fuerzaRuido.mult(0.25 * fuerzaGlobal);

    // Se aplica como fuerza real (afecta aceleración)
    p.aplicarFuerza(fuerzaRuido);


    // CAMPOS ESPACIALES

    // Zona izquierda → turbulencia aleatoria
    if (p.pos.x < width * 0.25) {
      let turbulencia = p5.Vector.random2D().mult(0.4 * fuerzaGlobal);
      p.aplicarFuerza(turbulencia);
    }

    // Zona derecha → mayor resistencia (drag)
    if (p.pos.x > width * 0.75) {

      let speed = p.vel.mag(); // Magnitud de la velocidad

      if (speed > 0) {

        // Fórmula clásica de drag proporcional a v²
        let dragMag = 0.04 * speed * speed * fuerzaGlobal;

        let drag = p.vel.copy();  // Copia para no modificar velocidad original
        drag.mult(-1);            // Dirección opuesta
        drag.normalize();         // Vector unitario
        drag.mult(dragMag);       // Magnitud final

        p.aplicarFuerza(drag);
      }
    }

    // Zona superior → repulsión radial
    if (p.pos.y < height * 0.25) {

      let centro = createVector(width / 2, height * 0.125);
      let repel = p5.Vector.sub(p.pos, centro);

      let d = repel.mag();

      if (d < 200) {
        repel.setMag(0.6 * fuerzaGlobal);
        p.aplicarFuerza(repel);
      }
    }

    // Zona inferior → fuerza calmante
    if (p.pos.y > height * 0.75) {
      let calma = fuerzaRuido.copy().mult(-0.6 * fuerzaGlobal);
      p.aplicarFuerza(calma);
    }

   // INTERACCIÓN COLECTIVA ENTRE PARTÍCULAS

    for (let otra of particulas) {

      if (otra !== p) {

        let dir = p5.Vector.sub(otra.pos, p.pos);
        let d = dir.mag();

        // Solo interactúan si están relativamente cerca
        if (d > 0 && d < 80) {

          dir.normalize();

          if (d < 20) {
            // Muy cerca → repulsión
            let repel = dir.copy().mult(-1).mult(0.6 * fuerzaGlobal);
            p.aplicarFuerza(repel);
          } else {
            // Distancia media → atracción suave
            let attract = dir.copy().mult(0.02 * fuerzaGlobal);
            p.aplicarFuerza(attract);
          }
        }
      }
    }


    // INTERACCIÓN CON CLICK IZQUIERDO


    if (mouseIsPressed && mouseButton === LEFT) {

      let mousePos = createVector(mouseX, mouseY);
      let atraccion = p5.Vector.sub(mousePos, p.pos);
      let distancia = atraccion.mag();

      if (distancia < 300) {
        atraccion.setMag(0.9 * fuerzaGlobal);
        p.aplicarFuerza(atraccion);
      }
    }


    // ONDA REPULSIVA (CLICK DERECHO)

    if (shockwave) {

      let repel = p5.Vector.sub(p.pos, shockCenter);
      let d = repel.mag();

      if (d < 400 && d > 0) {
        repel.setMag(1.5 * fuerzaGlobal);
        p.aplicarFuerza(repel);
      }
    }


    // DRAG GENERAL (Resistencia global)

    let speed = p.vel.mag();

    if (speed > 0) {

      let dragMag = 0.02 * speed * speed * fuerzaGlobal;

      let drag = p.vel.copy();
      drag.mult(-1);
      drag.normalize();
      drag.mult(dragMag);

      p.aplicarFuerza(drag);
    }

    // Actualiza física
    p.actualizar();

    // Dibuja partícula
    p.mostrar();

    // Revisa bordes
    p.bordes();
  }

  // Controla duración de la onda expansiva
  if (shockwave) {
    shockFrames--;
    if (shockFrames <= 0) {
      shockwave = false;
    }
  }
}


// EVENTOS DE INTERACCIÓN

// Click derecho → activa onda repulsiva
function mousePressed() {
  if (mouseButton === RIGHT) {
    shockwave = true;
    shockCenter = createVector(mouseX, mouseY);
    shockFrames = 15;
  }
}

// Tecla F → activa modo intenso
function keyPressed() {
  if (key === 'f' || key === 'F') {
    modoIntenso = true;
  }
}

// Al soltar F → vuelve a normalidad
function keyReleased() {
  if (key === 'f' || key === 'F') {
    modoIntenso = false;
  }
}


// CLASE PARTICULA

class Particula {

  constructor() {

    // Posición inicial aleatoria
    this.pos = createVector(random(width), random(height));

    // Velocidad inicial aleatoria
    this.vel = p5.Vector.random2D().mult(random(0.3, 1.5));

    // Aceleración inicia en cero
    this.acc = createVector(0, 0);

    // Masa física
    this.masa = random(0.8, 2.5);

    // Velocidad máxima (inversamente proporcional a masa)
    this.maxSpeed = 3 / this.masa;

    // Color inicial
    this.hue = random(180, 260);
  }

  // Aplica una fuerza considerando masa (F = m·a)
  aplicarFuerza(fuerza) {
    let f = p5.Vector.div(fuerza, this.masa);
    this.acc.add(f);
  }

  // Actualiza física (Motion 101 extendido)
  actualizar() {
    this.vel.add(this.acc);     // v = v + a
    this.vel.limit(this.maxSpeed); // Limita velocidad
    this.pos.add(this.vel);     // p = p + v
    this.acc.mult(0);           // Reinicia aceleración

    // Cambia ligeramente el tono
    this.hue += 0.02;
    if (this.hue > 360) this.hue = 0;
  }

  // Dibuja partícula
  mostrar() {
    let brillo = map(this.vel.mag(), 0, this.maxSpeed, 60, 100);
    stroke(this.hue, 60, brillo, 70);
    strokeWeight(2);
    point(this.pos.x, this.pos.y);
  }

  // Sistema toroidal (bordes conectados)
  bordes() {
    if (this.pos.x > width) this.pos.x = 0;
    if (this.pos.x < 0) this.pos.x = width;
    if (this.pos.y > height) this.pos.y = 0;
    if (this.pos.y < 0) this.pos.y = height;
  }
}
```
3. Un enlace al proyecto en el editor de p5.js.
https://editor.p5js.org/Nikeal/sketches/-9d3OD0Cw

4. Selecciona capturas de pantalla representativas de tu pieza de arte generativa.
<img width="940" height="772" alt="image" src="https://github.com/user-attachments/assets/9bc6d676-2458-41ac-b521-97cd559c5292" />
<img width="942" height="773" alt="image" src="https://github.com/user-attachments/assets/18851685-9e8b-4157-b7ee-e85d55c9e093" />

## Bitácora de reflexión

**1. Explica detalladamente en tu bitácora ¿Qué es el marco de movimiento motion 101 y cómo se relacionan: fuerza, aceleración, velocidad y posición?**

- El movimiento Motion 101 explica cómo se produce el movimiento en sistemas dinámicos a partir de una cadena de relaciones físicas, como una acumulacion de fuerzas en el tiempo, haciendo que el movimiento no se programe directamente y sea de manera natural como en el mundo real.
- Aqui el movimiento se construye de cuatro elementos que se relacionan entre sí de manera secuencial: Fuerza → Aceleración → Velocidad → Posición

1. La fuerza (vector) es el inicio de todo, aplicar la fuerza significa modificar la aceleración de un objeto. Esto se basa en la segunda ley de Newton (F = m·a), que indica que la aceleración depende de la fuerza aplicada y de la masa del objeto.

2. La aceleración representa el cambio en la velocidad y se calcula como la suma de todas las fuerzas que están actuando sobre el sistema.

3. La velocidad se actualiza sumando la aceleración e indica qué tan rápido y en qué dirección se está moviendo el objeto, es acumulativa haciendo que el movimiento se vuelve progresivamente más intenso.

4. La posición se actualiza sumando la velocidad y es el resultado final de todo lo anterior.

**2. Vas a analizar este video sobre el artista Alexander Calder. Selecciona una de sus obras y luego crea una obra generativa inspirada en la obra de Calder que seleccionaste y el marco de movimiento motion 101 con fuerzas que trabajamos en esta unidad.**

- Análisis de Alexander Calder, obra Lobster Trap and Fish Tail

Esta es uno de los primeros móviles suspendidos que realizó Calder y esta compuesta por muchas formas orgánicas distribuidas en diferentes niveles, conectadas por medio de varillas metálicas que funcionan como sistemas de equilibrio. Cada elemento tiene un peso específico y está ubicado estratégicamente para que el conjunto mantenga un balance dinámico.

Lo más interesante de esta pieza es que el movimiento no está controlado por ningún motor ni mecanismo artificial. El desplazamiento ocurre gracias a fuerzas naturales como:

* La gravedad
* La tensión en las varillas
* Las corrientes de aire
* El peso distribuido en cada pieza

Haciendo que el sistema nunca se mueva igual dos veces.

- Relación entre Calder y Motion 101

Al analizar la obra me di cuenta que Caldere utilizaba el mismo principio que en Motion 101:

Fuerza → Aceleración → Velocidad → Posición

En el caso de Calder:

* La fuerza es el viento o la gravedad.
* La aceleración ocurre cuando esas fuerzas cambian el estado del móvil.
* La velocidad surge como respuesta a esa alteración.
* La posición cambia constantemente en el espacio.

```js
let piezas = [];
let numPiezas = 8;

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100, 100);
  
  // Creamos piezas que cuelgan de diferentes puntos del "techo"
  for (let i = 0; i < numPiezas; i++) {
    piezas.push(new PiezaCalder(random(width), random(100, 200)));
  }
}

function draw() {
  background(20, 10, 95); // Fondo color papel viejo/museo

  // Dibujar línea de soporte superior
  stroke(0, 20);
  line(0, 50, width, 50);

  for (let p of piezas) {
    // 1. FUERZA: Brisa constante (Ruido de Perlin)
    let fuerzaViento = createVector(noise(frameCount * 0.01, p.anchor.x) - 0.5, 0);
    fuerzaViento.mult(0.2);
    p.aplicarFuerza(fuerzaViento);

    // 2. FUERZA: Gravedad y Tensión (Simulada como muelle)
    let gravedad = createVector(0, 0.1 * p.masa);
    p.aplicarFuerza(gravedad);

    // 3. INTERACTIVIDAD: Al mover el mouse, "empujas" las piezas
    if (dist(mouseX, mouseY, p.pos.x, p.pos.y) < 100) {
      let empuje = createVector(mouseX - pmouseX, mouseY - pmouseY);
      empuje.mult(0.5);
      p.aplicarFuerza(empuje);
    }

    p.actualizar();
    p.mostrar();
  }
}

class PiezaCalder {
  constructor(x, largoHilo) {
    this.anchor = createVector(x, 50); // Punto de donde cuelga
    this.pos = createVector(x, 50 + largoHilo);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    
    this.masa = random(2, 6);
    this.largoHilo = largoHilo;
    this.hue = random([0, 45, 200]); // Colores primarios clásicos de Calder (Rojo, Amarillo, Azul)
    this.tamano = this.masa * 15;
  }

  aplicarFuerza(f) {
    let fuerzaCopiada = f.copy();
    fuerzaCopiada.div(this.masa); // Newton: a = F/m
    this.acc.add(fuerzaCopiada);
  }

  actualizar() {
    // Motion 101
    this.vel.add(this.acc);
    this.vel.mult(0.98); // Amortiguación (resistencia del aire)
    this.pos.add(this.vel);
    this.acc.mult(0);

    // Restricción de Hilo (Mantenemos la pieza cerca de su anclaje)
    let vectorHilo = p5.Vector.sub(this.pos, this.anchor);
    if (vectorHilo.mag() > this.largoHilo) {
      vectorHilo.setMag(this.largoHilo);
      this.pos = p5.Vector.add(this.anchor, vectorHilo);
      this.vel.mult(0.5); // Pierde energía al tensarse el hilo
    }
  }

  mostrar() {
    // Dibujar el hilo
    stroke(0, 60);
    strokeWeight(1);
    line(this.anchor.x, this.anchor.y, this.pos.x, this.pos.y);

    // Dibujar la placa de metal (estética Calder)
    noStroke();
    fill(this.hue, 80, 80, 90);
    
    push();
    translate(this.pos.x, this.pos.y);
    // La pieza rota un poco según la velocidad
    rotate(this.vel.x * 0.2);
    // Formas orgánicas no perfectas
    if (this.hue == 0) ellipse(0, 0, this.tamano, this.tamano * 0.7);
    else if (this.hue == 45) triangle(-this.tamano/2, this.tamano/2, this.tamano/2, this.tamano/2, 0, -this.tamano/2);
    else rect(-this.tamano/2, -this.tamano/2, this.tamano, this.tamano, 5);
    pop();
  }
}
```
