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



## Bitácora de reflexión

