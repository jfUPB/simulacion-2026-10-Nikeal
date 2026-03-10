# Unidad 4

## Bitácora de proceso de aprendizaje

### Actividad 1

Lo que más me llamó la atención es cómo fuern utilizados los principios del movimiento armónico para crear una animación que parece orgánica y muy fluida. La forma en que los elementos se mueven de manera repetitiva y sincronizada genera una sensación de equilibrio y ritmo visual. También me pareció interesante cómo un concepto matemático y físico, como la oscilación, puede transformarse en una experiencia estética y visual dentro del arte generativo. Esta obra me hizo pensar en cómo las leyes del movimiento pueden utilizarse para crear composiciones dinámicas e interactivas en tiempo real.

### Actividad 2

Al analizar la primera simulación, observé que los elementos gráficos están rotando alrededor del centro de la pantalla. La interacción consiste en modificar el ángulo de rotación para generar este movimiento. En cada frame el origen del sistema de coordenadas se traslada al centro de la pantalla para que la rotación ocurra alrededor de ese punto y no desde la esquina del canvas, lo que hace que el movimiento sea más natural y simétrico. La función `rotate()` aplica una rotación al sistema de coordenadas, por lo que todos los elementos que se dibujen después se verán afectados por esta transformación.

En el fragmento de código donde se dibujan líneas y círculos alrededor de la posición (0,0), los elementos parecen estar centrados en ese punto porque el sistema de coordenadas ya fue trasladado previamente. Aunque en cada frame se dibuja exactamente lo mismo, los elementos rotan porque el sistema de coordenadas está siendo rotado continuamente antes de dibujar las figuras.

En la segunda simulación se puede identificar el marco Motion 101 en la actualización de la posición usando los vectores de posición, velocidad y aceleración. Este marco se utiliza para simular movimiento físico básico en cada frame.

- ¿Qué hace la función heading()?
La función `heading()` devuelve el ángulo del vector de velocidad, es decir, la dirección en la que el objeto se está moviendo. Este ángulo se usa en `rotate()` para que el objeto gráfico apunte hacia la dirección de su movimiento.

- ¿Qué hace la función push() y pop()? Realiza algunos experimentos para entender su funcionamiento.
Las funciones `push()` y `pop()` guardan y restauran el estado del sistema de coordenadas. Esto permite aplicar transformaciones como `translate()` y `rotate()` a un objeto específico sin afectar a los demás elementos que se dibujen después.

- ¿Qué hace rectMode(CENTER)? Realiza algunos experimentos para entender su funcionamiento.
La función `rectMode(CENTER)` hace que el rectángulo se dibuje tomando su centro como referencia en lugar de la esquina superior izquierda. Esto facilita la rotación del objeto, ya que gira alrededor de su centro.

- ¿Cuál es la relación entre el ángulo de rotación y el vector de velocidad? Trata de dibujar en un papel el vector de velocidad y cómo se relaciona con el ángulo de rotación y la operación de traslación y rotación.
La relación entre el ángulo de rotación y el vector de velocidad es directa: el ángulo que se obtiene del vector indica hacia dónde se está moviendo el objeto, y ese mismo ángulo se utiliza para rotar la figura gráfica para que visualmente apunte en esa dirección.

### Actividad 3

Desarrollé una simulación de un vehículo que puede moverse por la pantalla utilizando las teclas de flecha del teclado. El objetivo era aplicar los conceptos de movimiento y rotación aprendidos anteriormente.

1. Creé una clase llamada Vehiculo que contiene los vectores de posición, velocidad y aceleración. Esto permite implementar el marco Motion 101, donde en cada frame la aceleración se suma a la velocidad y la velocidad se suma a la posición para generar el movimiento del objeto.

2. Agregué la interacción con el teclado utilizando las teclas de flecha. Cuando se presiona la flecha izquierda se aplica una fuerza negativa en el eje x, y cuando se presiona la flecha derecha se aplica una fuerza positiva. Estas fuerzas modifican la aceleración del vehículo.

3. Para que el vehículo apunte en la dirección en la que se mueve utilicé la función heading() del vector de velocidad. Esta función devuelve el ángulo del movimiento y se usa en la función rotate() para orientar el triángulo.

4. También utilicé las funciones translate(), push() y pop() para mover el sistema de coordenadas a la posición del vehículo y aplicar la rotación sin afectar otros elementos del programa.

5. El resultado es un vehículo triangular que se mueve por la pantalla y rota automáticamente según la dirección de su velocidad, lo que permite visualizar de forma clara la relación entre movimiento y rotación.


```js
let vehiculo;

function setup() {
  createCanvas(windowWidth, windowHeight);
  vehiculo = new Vehiculo(width/2, height/2);
}

function draw() {
  background(240);

  vehiculo.update();
  vehiculo.edges();
  vehiculo.display();
}

class Vehiculo {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {

    // CONTROLES
    if (keyIsDown(LEFT_ARROW)) {
      let fuerza = createVector(-0.1, 0);
      this.applyForce(fuerza);
    }

    if (keyIsDown(RIGHT_ARROW)) {
      let fuerza = createVector(0.1, 0);
      this.applyForce(fuerza);
    }

    // Motion 101
    this.velocity.add(this.acceleration);
    this.velocity.limit(5);
    this.position.add(this.velocity);

    this.acceleration.mult(0);
  }

  edges() {
    if (this.position.x > width) this.position.x = 0;
    if (this.position.x < 0) this.position.x = width;
    if (this.position.y > height) this.position.y = 0;
    if (this.position.y < 0) this.position.y = height;
  }

  display() {

    let angle = this.velocity.heading();

    push();
    translate(this.position.x, this.position.y);
    rotate(angle);

    fill(100);
    stroke(0);

    triangle(-15, -10, -15, 10, 15, 0);

    pop();
  }
}
```
### Actividad 4

En la simulación se puede identificar claramente el marco Motion 101. Este marco describe cómo se calcula el movimiento de un objeto utilizando vectores de posición, velocidad y aceleración. En cada frame del programa se sigue el siguiente proceso: primero se suman las fuerzas a la aceleración, luego la aceleración se suma a la velocidad y finalmente la velocidad se suma a la posición del objeto.

Cuando se agregan fuerzas acumulativas es necesario modificar el marco Motion 101 introduciendo un vector de aceleración que acumule todas las fuerzas aplicadas durante cada frame. Cada fuerza se aplica mediante una función como applyForce(), donde la fuerza se divide por la masa del objeto según la segunda ley de Newton (F = m * a). Después de actualizar la velocidad y la posición, la aceleración debe reiniciarse a cero. Esto es importante porque si no se reinicia, las fuerzas seguirían acumulándose indefinidamente y el movimiento se volvería incorrecto o inestable.

En la simulación también aparece un objeto llamado Attractor. Este objeto funciona como una fuerza de atracción que influye sobre las partículas u objetos del sistema. Para cambiar su color se puede modificar la función display() dentro de la clase Attractor y cambiar el valor de fill() por otro color, por ejemplo fill(255, 0, 0) para que se vea rojo.

El Attractor también tiene los atributos this.dragging y this.rollover. Estos permiten implementar interacción con el mouse. dragging indicaría que el objeto está siendo arrastrado y rollover indicaría si el cursor está sobre el objeto. Para que esto funcione se pueden usar funciones de interacción de p5.js como mousePressed(), mouseReleased() y mouseDragged().

Por ejemplo, en mousePressed() se puede verificar si el mouse está sobre el attractor usando una distancia entre el mouse y su posición. Si la distancia es pequeña se activa this.dragging = true. En mouseDragged() se puede actualizar la posición del attractor con mouseX y mouseY para que siga el cursor. Finalmente en mouseReleased() se cambia this.dragging = false para soltar el objeto. Para el rollover se puede verificar en cada frame si el mouse está cerca del attractor y cambiar su color cuando esto ocurra.

De esta forma el atractor se puede mover con el mouse y reaccionar visualmente cuando el cursor pasa sobre él, haciendo la simulación más interactiva.

### Actividad 5

En esta actividad exploré el sistema de coordenadas polares y cómo se relaciona con las coordenadas cartesianas que normalmente usamos en p5.js.

En el código original se observa la conversión de coordenadas polares a cartesianas mediante las siguientes ecuaciones:

x = r * cos(theta)
y = r * sin(theta)

En este caso r representa la distancia desde el origen (el radio) y theta representa el ángulo de rotación. Estas dos variables determinan la posición de un punto en el plano. En cada frame se incrementa theta, lo que provoca que el punto se mueva formando un movimiento circular alrededor del origen. Además, el sistema de coordenadas se traslada al centro de la pantalla usando `translate(width/2, height/2)` para que la rotación ocurra alrededor del centro del canvas.

En la primera modificación del código se usa la función `p5.Vector.fromAngle(theta)`. Esta función crea un vector unitario a partir de un ángulo, es decir, un vector con magnitud igual a 1. Debido a esto, el punto dibujado con `circle(v.x, v.y, 48)` aparece muy cerca del centro de la pantalla y no se observa un círculo grande como en la simulación original. Esto ocurre porque el vector no está siendo multiplicado por el radio r, por lo que su longitud es muy pequeña.

En la segunda modificación se utiliza `p5.Vector.fromAngle(theta, r)`. En este caso la función genera un vector con el mismo ángulo theta pero con magnitud r. Esto significa que el vector ahora tiene la misma longitud que el radio utilizado en el sistema de coordenadas polares. Como resultado, el punto vuelve a moverse en un círculo alrededor del centro de la pantalla, igual que en la primera simulación. La diferencia es que ahora la conversión de coordenadas polares a cartesianas se hace utilizando directamente un vector de p5.js en lugar de calcular manualmente las ecuaciones con seno y coseno.

Esta actividad me permitió entender mejor cómo se pueden usar vectores para representar coordenadas polares, y cómo las funciones trigonométricas o las funciones de p5.js permiten convertir fácilmente entre coordenadas polares y cartesianas para generar movimientos circulares en simulaciones y obras generativas.

### Actividad 6

En esta actividad exploré el comportamiento de la **función sinusoide** y cómo se relaciona con diferentes parámetros que controlan su movimiento: **amplitud, periodo, frecuencia, fase y velocidad angular**.

La función sinusoide es muy importante en simulaciones porque permite modelar **movimientos oscilatorios o repetitivos**, como el movimiento de un péndulo, las olas del mar o las vibraciones. En el código de la simulación se utiliza la función `sin()` para calcular la posición de un punto que se mueve horizontalmente.

La **amplitud** determina qué tan lejos se mueve el punto desde el centro. En el código, la variable `amplitude` controla la distancia máxima que alcanza el movimiento. Si la amplitud aumenta, el movimiento se vuelve más amplio.

El **periodo** define cuánto tiempo tarda la onda en completar un ciclo completo. En la simulación se usan dos periodos diferentes (`period1` y `period2`), lo que hace que cada punto se mueva con una velocidad distinta. Cuando el periodo es menor, la onda oscila más rápido; cuando es mayor, el movimiento es más lento.

La **frecuencia** está relacionada con el periodo, ya que indica cuántos ciclos ocurren en un intervalo de tiempo. En términos simples, a menor periodo mayor frecuencia.

La **fase** permite desplazar la onda en el tiempo o en el espacio. En la simulación se puede modificar presionando una tecla, lo que genera un desfase entre las dos ondas. Esto hace que aunque tengan movimientos similares, sus posiciones no coincidan exactamente en el mismo momento.

Por último, la **velocidad angular** está relacionada con qué tan rápido cambia el ángulo dentro de la función seno. En el código esto ocurre porque el valor de `frameCount` aumenta constantemente y se multiplica por `TWO_PI`, generando un movimiento periódico continuo.

Esta actividad me permitió entender que las funciones sinusoidales son una herramienta muy útil para crear **movimientos naturales y cíclicos en animaciones y obras generativas**, ya que permiten controlar fácilmente el ritmo, la intensidad y el desfase del movimiento.

### Actividad 7

Para esta actividad partí de la simulación base del **Oscillator**, que utiliza funciones sinusoidales para generar movimiento oscilatorio en los ejes x y y. El movimiento ocurre porque el ángulo cambia constantemente a través de `angleVelocity`, y luego las funciones `sin()` convierten esos ángulos en posiciones dentro de la pantalla.

Para modificar la simulación integré conceptos de **dos unidades anteriores**.

Primero, incorporé un concepto de la Unidad 1: aleatoriedad utilizando ruido de Perlin (noise) en lugar de random. La diferencia es que random genera valores completamente independientes, mientras que noise produce variaciones suaves y continuas. Esto permitió que el movimiento del oscilador cambiara gradualmente en el tiempo, generando un comportamiento más orgánico y menos caótico.

Luego integré un concepto de la Unidad 3: fuerzas. En lugar de modificar directamente la velocidad angular, agregué un sistema de aceleración angular que recibe fuerzas externas. Estas fuerzas modifican la velocidad del ángulo de forma acumulativa, siguiendo el marco de movimiento Motion 101: primero se aplica la fuerza a la aceleración, luego la aceleración modifica la velocidad, y finalmente la velocidad modifica la posición (en este caso el ángulo).

Gracias a estas modificaciones el sistema dejó de ser un simple oscilador predecible y pasó a comportarse como un sistema dinámico influenciado por fuerzas externas, donde el movimiento cambia constantemente debido al entorno. Esto genera un resultado más cercano a una obra generativa, ya que el comportamiento no es completamente repetitivo y presenta variaciones orgánicas a lo largo del tiempo.

```js
let osc;

function setup() {
  createCanvas(800, 500);
  osc = new Oscillator();
}

function draw() {
  background(255);

  osc.aplicarFuerza(); // aplicamos fuerzas al sistema
  osc.update();        // actualizamos motion 101
  osc.show();          // dibujamos el oscilador
}

class Oscillator {
  constructor() {

    // Ángulo de oscilación en x y y
    this.angle = createVector();

    // Velocidad angular
    this.angleVelocity = createVector(0.02, 0.03);

    // Aceleración angular (aquí se acumulan las fuerzas)
    this.angleAcceleration = createVector(0, 0);

    // Amplitud del movimiento
    this.amplitude = createVector(
      random(100, width / 2),
      random(100, height / 2)
    );

    // variable para el ruido
    this.noiseOffset = random(1000);
  }

  aplicarFuerza() {

    // Generamos una fuerza usando ruido de Perlin
    let n = noise(this.noiseOffset);

    // Convertimos el ruido en una pequeña fuerza
    let fuerza = map(n, 0, 1, -0.001, 0.001);

    // Aplicamos la fuerza como aceleración angular
    this.angleAcceleration.x += fuerza;
    this.angleAcceleration.y += fuerza;

    // Avanzamos el ruido para el siguiente frame
    this.noiseOffset += 0.01;
  }

  update() {

    // Motion 101 aplicado al ángulo
    // aceleración modifica velocidad
    this.angleVelocity.add(this.angleAcceleration);

    // velocidad modifica el ángulo
    this.angle.add(this.angleVelocity);

    // reiniciamos la aceleración
    this.angleAcceleration.mult(0);
  }

  show() {

    // convertimos el ángulo en posición usando seno
    let x = sin(this.angle.x) * this.amplitude.x;
    let y = sin(this.angle.y) * this.amplitude.y;

    push();

    // movemos el origen al centro
    translate(width / 2, height / 2);

    stroke(0);
    strokeWeight(2);

    // línea que conecta el centro con el oscilador
    line(0, 0, x, y);

    fill(127);

    // círculo que representa la masa oscilante
    circle(x, y, 32);

    pop();
  }
}
```

### Actividad 8

Para modificar la simulación y hacer que la onda se comportara como una ola en movimiento, cambié la estructura del programa para que el cálculo de la onda ocurriera dentro de la función draw(). Esto permite que la escena se actualice constantemente en cada frame.

Agregué una variable llamada waveOffset, que funciona como un desplazamiento de fase de la función seno. En cada frame esta variable aumenta ligeramente, lo que provoca que el valor inicial del ángulo cambie con el tiempo.

Durante el for, cada posición en x calcula su posición y usando la función sin(angle), lo que genera la forma de la onda. Como el ángulo inicial cambia continuamente, la onda completa parece desplazarse horizontalmente, simulando el comportamiento de una ola propagándose.

Este ejercicio me permitió entender mejor cómo la velocidad angular y la fase afectan el comportamiento de las funciones sinusoidales y cómo pueden utilizarse para crear animaciones de ondas en tiempo real.

```js
let angle = 0;           // fase inicial de la onda
let angleVelocity = 0.2; // separación entre puntos de la onda
let amplitude = 100;     // altura de la onda
let waveOffset = 0;      // desplazamiento de la onda en el tiempo

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(255);

  stroke(0);
  strokeWeight(2);
  fill(127, 127);

  let angle = waveOffset; // comenzamos el ángulo desplazado

  for (let x = 0; x <= width; x += 24) {

    // Calculamos la altura usando la función seno
    let y = amplitude * sin(angle);

    // Dibujamos los círculos formando la onda
    circle(x, y + height / 2, 48);

    // incrementamos el ángulo para el siguiente punto
    angle += angleVelocity;
  }

  // esto hace que la onda avance con el tiempo
  waveOffset += 0.05;
}
```

### Actividad 9

Para esta actividad modifiqué la simulación original, que tenía un solo resorte conectado a una masa, para crear un sistema de dos resortes conectados en serie.

Primero añadí un segundo objeto Bob, que representa una segunda masa. Luego creé un segundo objeto Spring. El primer resorte se conecta al punto de anclaje en la parte superior de la pantalla y tira del primer bob. El segundo resorte se conecta al primer bob, por lo que su punto de anclaje cambia dinámicamente en cada frame.

Esto significa que el movimiento del primer objeto afecta directamente al segundo, generando un sistema de oscilación más complejo. Cuando el primer objeto se mueve, también modifica la fuerza que actúa sobre el segundo resorte, produciendo un comportamiento más realista de un sistema de resortes.

Durante la simulación también se aplica una fuerza de gravedad, lo que provoca que las masas oscilen verticalmente. Además, se mantiene la interacción con el mouse, permitiendo arrastrar las masas y observar cómo el sistema responde dinámicamente.

Este ejercicio me permitió entender mejor cómo funcionan los sistemas acoplados de oscilación, donde múltiples objetos interactúan a través de fuerzas.


```js
let bob1;
let bob2;

let spring1;
let spring2;

function setup() {
  createCanvas(640, 240);

  // Primer resorte conectado al techo
  spring1 = new Spring(width / 2, 10, 100);

  // Segundo resorte se conectará al primer bob
  spring2 = new Spring(width / 2, 110, 100);

  // Dos masas
  bob1 = new Bob(width / 2, 100);
  bob2 = new Bob(width / 2, 200);
}

function draw() {
  background(255);

  // Gravedad aplicada a ambos objetos
  let gravity = createVector(0, 0.5);
  bob1.applyForce(gravity);
  bob2.applyForce(gravity);

  // Conexión del primer resorte al primer bob
  spring1.connect(bob1);
  spring1.constrainLength(bob1, 30, 200);

  // El segundo resorte usa la posición del bob1 como ancla
  spring2.anchor = bob1.position.copy();
  spring2.connect(bob2);
  spring2.constrainLength(bob2, 30, 200);

  // Actualización de las masas
  bob1.update();
  bob2.update();

  bob1.handleDrag(mouseX, mouseY);
  bob2.handleDrag(mouseX, mouseY);

  // Dibujar resortes
  spring1.showLine(bob1);
  spring2.showLine(bob2);

  // Dibujar elementos
  spring1.show();
  bob1.show();
  bob2.show();
}

function mousePressed() {
  bob1.handleClick(mouseX, mouseY);
  bob2.handleClick(mouseX, mouseY);
}

function mouseReleased() {
  bob1.stopDragging();
  bob2.stopDragging();
}

// SPRING CLASS
class Spring {
  constructor(x, y, length) {
    this.anchor = createVector(x, y);
    this.restLength = length;
    this.k = 0.2;
  }

  connect(bob) {
    let force = p5.Vector.sub(bob.position, this.anchor);
    let currentLength = force.mag();

    let stretch = currentLength - this.restLength;

    force.setMag(-1 * this.k * stretch);

    bob.applyForce(force);
  }

  constrainLength(bob, minlen, maxlen) {

    let direction = p5.Vector.sub(bob.position, this.anchor);
    let length = direction.mag();

    if (length < minlen) {
      direction.setMag(minlen);
      bob.position = p5.Vector.add(this.anchor, direction);
      bob.velocity.mult(0);

    } else if (length > maxlen) {
      direction.setMag(maxlen);
      bob.position = p5.Vector.add(this.anchor, direction);
      bob.velocity.mult(0);
    }
  }

  show() {
    fill(127);
    circle(this.anchor.x, this.anchor.y, 10);
  }

  showLine(bob) {
    stroke(0);
    line(bob.position.x, bob.position.y, this.anchor.x, this.anchor.y);
  }
}

// BOB CLASS
class Bob {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector();
    this.acceleration = createVector();
    this.mass = 24;

    this.dragging = false;
    this.dragOffset = createVector();
  }

  applyForce(force) {
    let f = force.copy();
    f.div(this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.mult(0.98);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(175);
    circle(this.position.x, this.position.y, this.mass * 2);
  }

  handleClick(mx, my) {
    let d = dist(mx, my, this.position.x, this.position.y);
    if (d < this.mass) {
      this.dragging = true;
      this.dragOffset.x = this.position.x - mx;
      this.dragOffset.y = this.position.y - my;
    }
  }

  stopDragging() {
    this.dragging = false;
  }

  handleDrag(mx, my) {
    if (this.dragging) {
      this.position.x = mx + this.dragOffset.x;
      this.position.y = my + this.dragOffset.y;
    }
  }
}
```

### Actividad 10

Para esta actividad modifiqué la simulación original de un péndulo simple para crear un sistema de dos péndulos conectados en serie, también conocido como un péndulo doble.

Primero creé dos objetos de la clase Pendulum. El primer péndulo mantiene su punto de pivote fijo en la parte superior de la pantalla. Luego modifiqué el segundo péndulo para que su punto de pivote no fuera fijo, sino que se actualizara constantemente con la posición del bob del primer péndulo.

Esto se logra asignando en cada frame:

p2.pivot = p1.bob.copy();

De esta manera, el segundo péndulo queda conectado al final del primero, formando una cadena de movimiento. El comportamiento resultante es más complejo, ya que el movimiento del primer péndulo afecta directamente al segundo.

Durante la simulación ambos péndulos siguen utilizando las ecuaciones del movimiento angular, donde la aceleración angular depende de la gravedad, la longitud del brazo y el seno del ángulo. También se mantiene el sistema de amortiguación, que reduce progresivamente la energía del movimiento.

Además, la interacción con el mouse permite arrastrar cualquiera de los péndulos para modificar su ángulo inicial y observar cómo el sistema responde dinámicamente.

Este ejercicio me permitió comprender mejor cómo funcionan los sistemas de oscilación conectados, donde el movimiento de un elemento influye directamente en el comportamiento de otro.

```js
let p1;
let p2;

function setup() {
  createCanvas(640, 360);

  // Primer péndulo (desde el techo)
  p1 = new Pendulum(width / 2, 60, 120);

  // Segundo péndulo (se conectará al primero)
  p2 = new Pendulum(width / 2, 180, 120);
}

function draw() {
  background(255);

  // Actualizar primer péndulo
  p1.update();
  p1.drag();
  p1.show();

  // El pivote del segundo péndulo es el bob del primero
  p2.pivot = p1.bob.copy();

  // Actualizar segundo péndulo
  p2.update();
  p2.drag();
  p2.show();
}

function mousePressed() {
  p1.clicked(mouseX, mouseY);
  p2.clicked(mouseX, mouseY);
}

function mouseReleased() {
  p1.stopDragging();
  p2.stopDragging();
}

// CLASE PENDULUM
class Pendulum {
  constructor(x, y, r) {

    this.pivot = createVector(x, y);
    this.bob = createVector();

    this.r = r;

    this.angle = PI / 4;

    this.angleVelocity = 0;
    this.angleAcceleration = 0;

    this.damping = 0.995;

    this.ballr = 24;

    this.dragging = false;
  }

  update() {

    if (!this.dragging) {

      let gravity = 0.4;

      this.angleAcceleration = ((-1 * gravity) / this.r) * sin(this.angle);

      this.angleVelocity += this.angleAcceleration;

      this.angle += this.angleVelocity;

      this.angleVelocity *= this.damping;
    }
  }

  show() {

    this.bob.set(
      this.r * sin(this.angle),
      this.r * cos(this.angle)
    );

    this.bob.add(this.pivot);

    stroke(0);
    strokeWeight(2);

    line(this.pivot.x, this.pivot.y, this.bob.x, this.bob.y);

    fill(127);
    circle(this.bob.x, this.bob.y, this.ballr * 2);
  }

  clicked(mx, my) {

    let d = dist(mx, my, this.bob.x, this.bob.y);

    if (d < this.ballr) {
      this.dragging = true;
    }
  }

  stopDragging() {

    this.angleVelocity = 0;

    this.dragging = false;
  }

  drag() {

    if (this.dragging) {

      let diff = p5.Vector.sub(
        this.pivot,
        createVector(mouseX, mouseY)
      );

      this.angle = atan2(-1 * diff.y, diff.x) - radians(90);
    }
  }
}
```

## Bitácora de aplicación 

### 1. Concepto de tu obra generativa

### 2. El código de la aplicación.
```js
// arreglo que almacenará la red de nodos que forman la membrana
let redNodos = []; 
// arreglo que almacenará los agentes que se moverán sobre la red
let simbiontes = [];
// número de anillos concéntricos que tendrá la membrana
let anillos = 12;
// número de divisiones radiales en cada anillo
let radios = 30;
// variable que almacenará el objeto del micrófono
let mic;
// variable que indica si el audio ya fue activado
let audioActivo = false;

// variable que guarda el volumen del micrófono suavizado
let volSuavizado = 0; 

// color base que se usará para la paleta de la visualización
let colorBase = 200;

// función setup() se ejecuta una sola vez al iniciar el programa
function setup() {

  // crea un canvas del tamaño de la ventana del navegador
  createCanvas(windowWidth, windowHeight);

  // cambia el modo de color a HSB (tono, saturación, brillo, alpha)
  colorMode(HSB, 360, 100, 100, 100);
  
  // crea el objeto del micrófono para capturar audio
  mic = new p5.AudioIn();

  // calcula el radio máximo que tendrá la membrana
  let radioMaximo = min(width, height) * 0.45;

  // calcula la posición X del centro del canvas
  let cx = width / 2;

  // calcula la posición Y del centro del canvas
  let cy = height / 2;

// 1. Construir la membrana

  // recorre cada anillo de la estructura
  for (let i = 0; i <= anillos; i++) {

    // crea un arreglo para almacenar los nodos de ese anillo
    let fila = [];

    // calcula la distancia radial del anillo actual
    let distanciaR = map(i, 0, anillos, 0, radioMaximo);
    
    // recorre cada división circular del anillo
    for (let j = 0; j < radios; j++) {

      // calcula el ángulo correspondiente a esta división
      let angulo = map(j, 0, radios, 0, TWO_PI);

      // convierte coordenadas polares a coordenadas cartesianas para X
      let x = cx + cos(angulo) * distanciaR;

      // convierte coordenadas polares a coordenadas cartesianas para Y
      let y = cy + sin(angulo) * distanciaR;

      // crea un nuevo nodo y lo agrega a la fila
      fila.push(new NodoVidrio(x, y, i, j));
    }

    // agrega la fila completa de nodos a la red
    redNodos.push(fila);
  }

  // 2. Inyectar los Simbiontes en la red

  // crea 45 agentes simbiontes
  for (let i = 0; i < 45; i++) {

    // agrega un nuevo simbionte al arreglo
    simbiontes.push(new Simbionte());
  }
}

// función draw() se ejecuta continuamente (cada frame)
function draw() {

  // pinta el fondo oscuro
  background(10, 15, 15);

  // variable para almacenar el volumen del micrófono
  let volumen = 0;

  // si el audio ya fue activado
  if (audioActivo) {

    // obtiene el nivel del micrófono y lo amplifica
    volumen = mic.getLevel() * 3; 

    // suaviza el cambio de volumen para evitar saltos bruscos
    volSuavizado = lerp(volSuavizado, volumen, 0.15); 
  }

 // --- FÍSICA Y REGLAS DE LA MEMBRANA ---

  // recorre todos los anillos
  for (let i = 0; i <= anillos; i++) {

    // recorre todos los radios de cada anillo
    for (let j = 0; j < radios; j++) {

      // obtiene el nodo actual de la red
      let nodo = redNodos[i][j];

// EVENTO MOUSE

      // si el mouse está presionado
      if (mouseIsPressed) {

        // crea un vector con la posición del mouse
        let mouseVec = createVector(mouseX, mouseY);

        // calcula la distancia entre el nodo y el mouse
        let distMouse = p5.Vector.dist(nodo.pos, mouseVec);
        
        // si el nodo está dentro del área de influencia
        if (distMouse < 350) {

          // calcula la dirección de la fuerza desde el nodo al mouse
          let fuerza = p5.Vector.sub(mouseVec, nodo.pos);

          // normaliza el vector (solo dirección)
          fuerza.normalize();

          // calcula la magnitud de la fuerza según la distancia
          let magnitud = map(distMouse, 0, 350, 1.5, 0);
          
          // si se presiona la barra espaciadora se invierte la fuerza
          if (keyIsDown(32)) fuerza.mult(-2.5); // Repulsión

          // si no, se aplica como atracción
          else fuerza.mult(magnitud); // Atracción
          
          // aplica la fuerza al nodo
          nodo.aplicarFuerza(fuerza);
        }
      }
// EVENTO AUDIO

      // si el volumen supera un umbral
      if (volSuavizado > 0.005) {

        // crea un vector en el centro del canvas
        let centro = createVector(width/2, height/2);

        // calcula un vector desde el centro hacia el nodo
        let expansion = p5.Vector.sub(nodo.pos, centro);

        // normaliza la dirección
        expansion.normalize();

        // multiplica la fuerza según el volumen
        expansion.mult(volSuavizado * 80); 

        // aplica la fuerza de expansión al nodo
        nodo.aplicarFuerza(expansion);
      }

      // actualiza la física del nodo
      nodo.actualizar();
    }
  }


// --- RENDERIZADO DE LA MEMBRANA ---

  // desactiva el borde de las figuras
  noStroke();
  
  // recorre todos los anillos excepto el último
  for (let i = 0; i < anillos; i++) {

    // recorre los radios
    for (let j = 0; j < radios; j++) {

      // obtiene cuatro nodos para formar un cuadrilátero
      let n1 = redNodos[i][j];
      let n2 = redNodos[i + 1][j];
      let n3 = redNodos[i + 1][(j + 1) % radios];
      let n4 = redNodos[i][(j + 1) % radios];

      // calcula cuánto se deformó el nodo respecto a su origen
      let estiramiento = p5.Vector.dist(n1.pos, n1.origen);
      
      // calcula la energía acumulada dejada por los simbiontes
      let calorAcumulado = n1.cargaEnergia * 50; 
      
      // calcula el matiz del color según deformación, audio y energía
      let matiz = (colorBase + estiramiento * 2 + volSuavizado * 500 - calorAcumulado) % 360;

      // calcula el brillo del color
      let brillo = map(estiramiento, 0, 100, 30, 100) + (volSuavizado * 150) + (n1.cargaEnergia * 100);

      // calcula la transparencia según el anillo
      let alfa = map(i, 0, anillos, 90, 0); 

      // aplica color de relleno
      fill(matiz, 80, brillo, alfa);

      // aplica color del borde
      stroke(matiz, 50, brillo + 30, alfa * 0.6);

      // define grosor del borde
      strokeWeight(0.5);

      // comienza la figura
      beginShape();

      // dibuja los vértices del cuadrilátero
      vertex(n1.pos.x, n1.pos.y);
      vertex(n2.pos.x, n2.pos.y);
      vertex(n3.pos.x, n3.pos.y);
      vertex(n4.pos.x, n4.pos.y);

      // cierra la figura
      endShape(CLOSE);
    }
  }

  // --- REGLAS AUTÓNOMAS DE LOS SIMBIONTES ---
  for (let s of simbiontes) {
    s.actualizar(volSuavizado);
    s.mostrar();
  }

  // --- INTERFAZ ---
  fill(255);
  noStroke();
  textSize(13);
  text("SISTEMA GENERATIVO: ESTIGMERGIA Y FÍSICA", 20, 30);
  text("1. MANTÉN MOUSE para succionar. ESPACIO para repeler.", 20, 50);
  text("2. HABLA para inflar el tejido y alterar rutas nerviosas.", 20, 70);
  text("3. Flechas Der/Izq para cambiar paleta base.", 20, 90);
  
  fill(0, 100, 100, volSuavizado * 300);
  circle(25, 115, 10 + volSuavizado * 100);
  fill(255);
  text("Mic Input", 45, 119);
}

// --- CLASES DEL SISTEMA GENERATIVO ---

class NodoVidrio {
  constructor(x, y, anillo, radioIdx) {
    this.origenBase = createVector(x, y);
    this.origen = createVector(x, y);
    this.pos = createVector(x, y);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.masa = map(anillo, 0, anillos, 3, 1); 
    this.ruidoOffsetX = x * 0.01;
    this.ruidoOffsetY = y * 0.01;
    
    // REGLA GENERATIVA: Rastro de las visitas
    this.cargaEnergia = 0; 
  }

  aplicarFuerza(f) {
    let fuerza = p5.Vector.div(f, this.masa);
    this.acc.add(fuerza);
  }

  actualizar() {
    let nx = map(noise(this.ruidoOffsetX, frameCount * 0.005), 0, 1, -20, 20);
    let ny = map(noise(this.ruidoOffsetY, frameCount * 0.005), 0, 1, -20, 20);
    this.origen.x = this.origenBase.x + nx;
    this.origen.y = this.origenBase.y + ny;

    let resorte = p5.Vector.sub(this.origen, this.pos);
    resorte.mult(0.08); 
    this.aplicarFuerza(resorte);

    this.vel.add(this.acc);
    this.vel.mult(0.88); 
    this.pos.add(this.vel);
    this.acc.mult(0);
    
    // REGLA GENERATIVA: El rastro térmico se evapora con el tiempo
    this.cargaEnergia *= 0.95; 
  }
}

class Simbionte {
  constructor() {
    this.idxAnillo = floor(random(anillos));
    this.idxRadio = floor(random(radios));
    
    this.nodoActual = redNodos[this.idxAnillo][this.idxRadio];
    this.nodoDestino = this.nodoActual;
    
    this.t = 1; 
    this.velocidadBase = random(0.02, 0.06);
  }

  escogerDestino() {
    // 1. Identificar nodos vecinos posibles (Sintaxis topológica)
    let opciones = [];
    
    if (this.idxAnillo < anillos) opciones.push({a: this.idxAnillo + 1, r: this.idxRadio});
    if (this.idxAnillo > 0) opciones.push({a: this.idxAnillo - 1, r: this.idxRadio});
    opciones.push({a: this.idxAnillo, r: (this.idxRadio + 1) % radios});
    opciones.push({a: this.idxAnillo, r: (this.idxRadio - 1 + radios) % radios});

    // 2. REGLA GENERATIVA DE DECISIÓN: Buscar el camino menos transitado
    let mejorOpcion = opciones[0];
    let menorCarga = Infinity;

    for (let op of opciones) {
      let nodoCandidato = redNodos[op.a][op.r];
      // Si el nodo está más "frío", se convierte en la mejor opción
      if (nodoCandidato.cargaEnergia < menorCarga) {
        menorCarga = nodoCandidato.cargaEnergia;
        mejorOpcion = op;
      }
    }

    // A veces, introducimos una pequeña mutación (aleatoriedad) para que no se queden atrapados
    if (random(1) < 0.15) {
      mejorOpcion = random(opciones);
    }

    this.idxAnillo = mejorOpcion.a;
    this.idxRadio = mejorOpcion.r;
    
    this.nodoDestino = redNodos[this.idxAnillo][this.idxRadio];
    this.t = 0; 
    
    // Dejar rastro (Feromona/Calor) en el nuevo nodo
    this.nodoDestino.cargaEnergia += 0.8; 
  }

  actualizar(vol) {
    if (this.t >= 1) {
      this.nodoActual = this.nodoDestino;
      this.escogerDestino();
    } else {
      let velocidadActual = this.velocidadBase + (vol * 0.8);
      this.t += velocidadActual;
      
      // Seguro para no pasarse del destino
      if (this.t > 1) this.t = 1;
    }
  }

  mostrar() {
    let posX = lerp(this.nodoActual.pos.x, this.nodoDestino.pos.x, this.t);
    let posY = lerp(this.nodoActual.pos.y, this.nodoDestino.pos.y, this.t);

    let brillo = 80 + sin(frameCount * 0.2) * 20; 
    
    noFill();
    stroke(50, 20, 100, 40); 
    strokeWeight(10);
    point(posX, posY);

    stroke(50, 0, 100, 100); 
    strokeWeight(3);
    point(posX, posY);
  }
}

function mousePressed() {
  if (!audioActivo) {
    userStartAudio();
    mic.start();
    audioActivo = true;
  }
}

function keyPressed() {
  if (keyCode === RIGHT_ARROW) colorBase = (colorBase + 45) % 360;
  else if (keyCode === LEFT_ARROW) colorBase = (colorBase - 45 + 360) % 360;
}
```
### 3. Un enlace al proyecto en el editor de p5.js.

https://editor.p5js.org/Nikeal/sketches/DSwyZRCE1

### 4. Selecciona capturas de pantalla representativas de tu pieza de arte generativa.




## Bitácora de reflexión

