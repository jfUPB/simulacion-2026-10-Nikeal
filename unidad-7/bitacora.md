# Unidad 7

## Bitácora de proceso de aprendizaje

### Actividad 1

- 1. Para entender la tipografía semántica, analicé estas tres:

"TSUNAMI": La letra 'S' se escala y se curva dramáticamente sobre el resto de la palabra. La manipulación refuerza el significado al convertir la forma de la letra en la ola gigante misma a punto de estrellarse, haciendo que el texto sea simultáneamente el sustantivo y la imagen del desastre.

"ELEVATOR" (Ascensor): Las astas diagonales de la letra 'V' se separan ligeramente para formar dos triángulos (uno apuntando hacia arriba y otro hacia abajo), idénticos a los botones de un ascensor. Es una manipulación sutil que utiliza la anatomía existente de la letra para revelar su función mecánica.

"HORIZON" (Horizonte): La letra 'O' se corta por la mitad, alineando su base plana con la línea inferior del resto de las letras. Visualmente, transforma la palabra entera en un paisaje donde la 'O' es un sol naciente o poniente descansando exactamente sobre la línea del horizonte que forman las demás letras.

- 2. Propuestas 

Palabra 1: DERRUMBE

Visual y Física: Las letras no están alineadas horizontalmente, sino apiladas formando una torre precaria de cajas de colisión en Matter.js.

Reacción al audio: La torre se mantiene estable en el silencio. Sin embargo, los picos de las frecuencias graves (el bajo de la canción) actúan como temblores, aplicando fuerzas laterales a los cuerpos de las letras hasta que la gravedad hace su trabajo y la palabra colapsa físicamente por la pantalla.

Palabra 2: ATROFIA (o DESGASTE)

Visual y Física: La palabra se lee claramente al centro, flotando en un entorno sin gravedad o apoyada en el suelo.

Reacción al audio: A medida que avanza la canción y la amplitud (volumen) impacta las letras, la propiedad de "restitución" (rebote) y la masa de los cuerpos en Matter.js comienza a fallar o encogerse. Las letras pierden su forma original, desarmándose en polígonos más pequeños o perdiendo la fuerza para mantenerse erguidas.

Palabra 3: TENSIÓN

Visual y Física: Las letras están distribuidas por la pantalla, pero están unidas mecánicamente por "Constraints" (resortes o ligamentos elásticos) invisibles de Matter.js.

Reacción al audio: El ritmo de la música aplica fuerzas repulsivas a las letras, empujándolas a los bordes de la pantalla. Los resortes se estiran al máximo, obligando a las letras a jalonarse entre sí frenéticamente al ritmo de la música, luchando por no separarse.

3. Elección y justificación

De estas opciones, la palabra que más me interesa desarrollar es TENSIÓN. Me atrae porque me permite explorar a fondo la herramienta de "Constraints" (restricciones elásticas) en Matter.js. En lugar de simplemente dejar que las letras caigan por la gravedad (lo cual es el uso más básico del motor de físicas), TENSIÓN exige que el sistema esté en un estado de estrés constante. Semánticamente, es muy potente ver cómo las letras intentan huir del centro impulsadas por el audio, pero están condenadas a mantenerse atadas, convirtiendo la tipografía en una coreografía de lucha y resistencia que puedo interpretar en vivo.

### Actividad 2

- 1. Conceptos fundamentales de Matter.js
Para entender cómo darle vida a la tipografía, es necesario comprender la arquitectura del motor físico. Así es como funcionan sus partes principales:

* **Engine (El Motor):** Es el "cerebro" o la calculadora del sistema. Su trabajo es procesar las matemáticas detrás de escena en cada *frame* (60 veces por segundo). Calcula la gravedad, las colisiones y las fuerzas antes de que nosotros las dibujemos en p5.js.
* **World (El Mundo / El Escenario):** Es el contenedor invisible donde existen todos nuestros objetos físicos. Si creas una letra pero no la agregas al *World*, el *Engine* la ignorará y no tendrá físicas. Es como el lienzo, pero para las matemáticas.
* **Bodies (Los Cuerpos):** Son los "actores" de la simulación. En lugar de dibujar un simple `rect()` en p5.js, creamos un *Body* rectangular en Matter.js. Estos cuerpos tienen propiedades del mundo real: masa, fricción, inercia y restitución (rebote). Pueden ser estáticos (como el suelo) o dinámicos (como letras cayendo).
* **Constraint (La Restricción / El Resorte):** Es una conexión mecánica entre dos *Bodies* (o entre un cuerpo y un punto fijo en el espacio). Funciona como un hueso, una cuerda o un elástico. Nos permite definir qué tan rígida o flexible es la unión entre dos elementos.
* **MouseConstraint (La Mano del Usuario):** Es el puente entre el mouse de p5.js y el mundo de Matter.js. Le da al cursor la capacidad física de agarrar, arrastrar y lanzar los *Bodies* como si estuviéramos tocándolos a través de la pantalla.



- 2. Experimentos básicos integrando Matter.js con p5.js

*(Nota para la bitácora: Para ejecutar estos códigos en el editor web de p5.js, asegúrate de haber agregado la librería de Matter.js en tu archivo `index.html`: `<script src="https://cdnjs.cloudflare.com/ajax/libs/matter-js/0.19.0/matter.min.js"></script>`)*

**Experimento A: Gravedad, Rebote y MouseConstraint (Cuerpos Libres)**
Este experimento prueba cómo crear letras básicas (cajas), darles una propiedad elástica (restitution) y permitir que el usuario interactúe con ellas.

```javascript
// Experimento A: Caída libre e interacción
const { Engine, World, Bodies, Composite, Mouse, MouseConstraint } = Matter;

let motor, mundo;
let cajas = [];
let suelo;
let mConstraint;

function setup() {
  let canvas = createCanvas(800, 600);
  
  // 1. Iniciar el Motor y el Mundo
  motor = Engine.create();
  mundo = motor.world;
  
  // 2. Crear un suelo estático
  suelo = Bodies.rectangle(width/2, height, width, 50, { isStatic: true });
  Composite.add(mundo, suelo);
  
  // 3. Crear algunas "Letras" (Cajas) con rebote
  for (let i = 0; i < 5; i++) {
    let caja = Bodies.rectangle(random(width), random(-200, 0), 60, 60, {
      restitution: 0.8 // 80% de rebote
    });
    cajas.push(caja);
    Composite.add(mundo, caja);
  }
  
  // 4. Agregar interacción con el Mouse
  let canvasMouse = Mouse.create(canvas.elt);
  canvasMouse.pixelRatio = pixelDensity();
  mConstraint = MouseConstraint.create(motor, { mouse: canvasMouse });
  Composite.add(mundo, mConstraint);
}

function draw() {
  background(20);
  Engine.update(motor); // Actualizar las físicas
  
  // Dibujar el suelo
  fill(100); noStroke();
  rectMode(CENTER);
  rect(suelo.position.x, suelo.position.y, width, 50);
  
  // Dibujar las cajas
  fill(255, 50, 50);
  for (let caja of cajas) {
    push();
    translate(caja.position.x, caja.position.y);
    rotate(caja.angle);
    rect(0, 0, 60, 60);
    // Simular que es una letra
    fill(255); textAlign(CENTER, CENTER); textSize(30);
    text("A", 0, 0);
    pop();
  }
}
```

**Experimento B: Uniones Elásticas (Constraints)**
Este experimento prueba cómo conectar dos cuerpos físicos usando un elástico. Si arrastras uno con el mouse, el otro lo seguirá rebotando.

```javascript
// Experimento B: Vínculos elásticos (Constraints)
const { Engine, World, Bodies, Composite, Constraint, Mouse, MouseConstraint } = Matter;

let motor, mundo;
let cuerpo1, cuerpo2;
let conexion;
let mConstraint;

function setup() {
  let canvas = createCanvas(800, 600);
  motor = Engine.create();
  mundo = motor.world;
  
  // Crear dos cuerpos
  cuerpo1 = Bodies.circle(300, 200, 40);
  cuerpo2 = Bodies.circle(500, 200, 40);
  
  // Crear el Constraint (resorte) que los une
  conexion = Constraint.create({
    bodyA: cuerpo1,
    bodyB: cuerpo2,
    length: 200,    // Distancia deseada entre ellos
    stiffness: 0.05 // Qué tan rígido es el resorte (0 a 1)
  });
  
  Composite.add(mundo, [cuerpo1, cuerpo2, conexion]);
  
  // MouseConstraint para probar tirar de ellos
  let canvasMouse = Mouse.create(canvas.elt);
  canvasMouse.pixelRatio = pixelDensity();
  mConstraint = MouseConstraint.create(motor, { mouse: canvasMouse });
  Composite.add(mundo, mConstraint);
}

function draw() {
  background(20);
  Engine.update(motor);
  
  // Dibujar la conexión (Constraint)
  stroke(255); strokeWeight(2);
  line(cuerpo1.position.x, cuerpo1.position.y, cuerpo2.position.x, cuerpo2.position.y);
  
  // Dibujar los cuerpos
  noStroke(); fill(50, 150, 255);
  circle(cuerpo1.position.x, cuerpo1.position.y, 80);
  fill(255, 150, 50);
  circle(cuerpo2.position.x, cuerpo2.position.y, 80);
}
```

- 3. Comportamiento físico a explorar en el proyecto


El comportamiento físico que me interesa explorar es la **lucha de fuerzas opuestas** utilizando extensamente la propiedad de *Constraint* (restricciones elásticas). 

No quiero que mi palabra simplemente caiga por la gravedad, sino que viva en un estado de constante estrés. Las letras de la palabra "TENSIÓN" estarán unidas entre sí por *Constraints* invisibles con una rigidez media. El objetivo es usar el audio para aplicar fuerzas externas (como pequeñas explosiones o viento direccional en Matter.js) que empujen las letras lejos del centro en direcciones aleatorias. El comportamiento visual será el de las letras intentando separarse violentamente debido a la música, pero siendo jaladas de vuelta por sus uniones físicas, creando un "baile" frenético, estirado y resistente que encarna perfectamente el significado de la palabra.

### Actividad 3

- 1. Experimentos de audio-reactividad

**Experimento A: Respuesta Continua (Amplitud global)**
En este primer experimento utilizo el micrófono para leer el volumen general del entorno y traducirlo en una expansión física y continua.

* **¿Qué dato estoy leyendo?** La *Amplitud* (el nivel de volumen general, un valor entre 0.0 y 1.0).
* **¿Qué comportamiento visual activa?** Un comportamiento de mapeo continuo. El volumen modifica directamente el diámetro de un círculo. No hay saltos bruscos; la figura respira y se expande en tiempo real siguiendo la envolvente del sonido.

```javascript
// Experimento A: Amplitud Continua (Micrófono)
let mic;

function setup() {
  createCanvas(400, 400);
  // Iniciamos la entrada de audio (micrófono)
  mic = new p5.AudioIn();
  mic.start();
  noStroke();
}

function draw() {
  background(30, 30, 40, 50); // Efecto de rastro suave
  
  // Obtenemos el volumen actual (0 a 1)
  let vol = mic.getLevel();
  
  // Mapeamos el volumen a un tamaño visual visible (de 50px a 350px)
  let tamaño = map(vol, 0, 0.5, 50, 350);
  
  fill(255, 100, 150);
  circle(width/2, height/2, tamaño);
}

// Habilitar el audio al hacer clic (política de navegadores)
function mousePressed() {
  userStartAudio();
}
```

**Experimento B: Respuesta Puntual por Umbral (Frecuencias Graves / FFT)**
En este experimento simulo la lectura de una pista de audio (o el mismo micrófono) pero separando el sonido en frecuencias para reaccionar solo a los "golpes" fuertes.

* **¿Qué dato estoy leyendo?** La energía específica de las frecuencias bajas (*bass*), usando el algoritmo FFT (Fast Fourier Transform). Este dato devuelve un valor de 0 a 255.
* **¿Qué comportamiento visual activa?** Un evento condicional (trigger). Si la energía de los graves supera un umbral específico (ej. > 200), el fondo "estalla" en color rojo y la figura da un salto violento. Es perfecto para marcar los *drops* o golpes de percusión de una canción.

```javascript
// Experimento B: Análisis FFT y Umbrales (Bajos)
let mic, fft;

function setup() {
  createCanvas(400, 400);
  mic = new p5.AudioIn();
  mic.start();
  
  // Iniciamos el analizador de frecuencias conectado al mic
  fft = new p5.FFT();
  fft.setInput(mic);
}

function draw() {
  background(20);
  
  // Analizamos el espectro de sonido actual
  fft.analyze();
  
  // Aislamos solo la energía de los graves (0 a 255)
  let graves = fft.getEnergy("bass");
  
  // Comportamiento de Umbral (Trigger)
  if (graves > 200) {
    background(200, 40, 40); // Estallido rojo si el bajo es muy fuerte
  }
  
  fill(255);
  noStroke();
  // El círculo central también reacciona solo al bajo
  let radio = map(graves, 0, 255, 50, 300);
  circle(width/2, height/2, radio);
}

function mousePressed() {
  userStartAudio();
}
```

- 2. Relación con mi palabra ("TENSIÓN")

Para la palabra "TENSIÓN", me serviría más una **combinación de respuesta continua (Amplitud) y eventos puntuales (FFT Bajos)**. 

Por un lado, usaría la *Amplitud general* para controlar la "rigidez" (*stiffness*) de los *Constraints* (resortes) en Matter.js. Si la canción es suave, los resortes son relajados y las letras se acercan. Si el volumen sube, la rigidez aumenta y el sistema se vuelve rígido e incómodo. 

Por otro lado, usaría la *energía de los Bajos (FFT)* como un disparador (umbral) para aplicar fuerzas explosivas laterales a las cajas de las letras. Cada vez que haya un golpe de batería, el código inyectará una fuerza física a cada letra, empujándolas violentamente hacia afuera para estirar los resortes, materializando así el concepto de tensión extrema provocada por el ritmo musical.

### Actividad 4

- 1. Muestra de la prueba inicial (Código)

```javascript
const { Engine, World, Bodies, Composite, Constraint, Mouse, MouseConstraint, Body } = Matter;

let motor, mundo;
let letras = [];
let uniones = [];
let mic, fft;

function setup() {
  createCanvas(600, 400);
  motor = Engine.create();
  mundo = motor.world;
  motor.world.gravity.y = 0; // Gravedad cero para que floten en tensión
  
  // Audio
  mic = new p5.AudioIn();
  mic.start();
  fft = new p5.FFT();
  fft.setInput(mic);

  // Crear 3 letras (T, E, N) como cajas físicas
  let caracteres = ["T", "E", "N"];
  for (let i = 0; i < 3; i++) {
    let caja = Bodies.rectangle(200 + (i * 80), 200, 50, 50, { frictionAir: 0.05 });
    caja.letra = caracteres[i]; // Guardamos la letra en el objeto
    letras.push(caja);
    Composite.add(mundo, caja);
  }

  // Unir T con E, y E con N mediante Constraints (resortes)
  for (let i = 0; i < letras.length - 1; i++) {
    let resorte = Constraint.create({
      bodyA: letras[i],
      bodyB: letras[i + 1],
      length: 80,       // Distancia en reposo
      stiffness: 0.01   // Rigidez inicial muy baja
    });
    uniones.push(resorte);
    Composite.add(mundo, resorte);
  }

  // Interacción con mouse
  let mConstraint = MouseConstraint.create(motor, { mouse: Mouse.create(canvas.elt) });
  Composite.add(mundo, mConstraint);
}

function draw() {
  background(20);
  Engine.update(motor);

  let vol = mic.getLevel();
  fft.analyze();
  let bajos = fft.getEnergy("bass");

  // A. EL AUDIO AFECTA LA PROPIEDAD FÍSICA DE RIGIDEZ
  // El volumen general tensa los resortes
  let nuevaRigidez = map(vol, 0, 0.5, 0.005, 0.1); 
  for (let u of uniones) {
    u.stiffness = nuevaRigidez;
  }

  // B. EL AUDIO GENERA FUERZAS REPULSIVAS
  // Si hay un golpe fuerte, las letras intentan huir a los extremos
  if (bajos > 200) {
    Body.applyForce(letras[0], letras[0].position, { x: -0.02, y: random(-0.01, 0.01) }); // La 'T' huye a la izq
    Body.applyForce(letras[2], letras[2].position, { x: 0.02, y: random(-0.01, 0.01) });  // La 'N' huye a la der
  }

  // Dibujar resortes
  stroke(255, 100); strokeWeight(2);
  for (let u of uniones) {
    line(u.bodyA.position.x, u.bodyA.position.y, u.bodyB.position.x, u.bodyB.position.y);
  }

  // Dibujar letras
  textAlign(CENTER, CENTER); textSize(32); textStyle(BOLD);
  for (let l of letras) {
    push();
    translate(l.position.x, l.position.y);
    rotate(l.angle);
    fill(200, 50, 50); noStroke();
    rectMode(CENTER); rect(0, 0, 50, 50); // La caja física
    fill(255); 
    text(l.letra, 0, 0); // El texto
    pop();
  }
}

function mousePressed() { userStartAudio(); }
```

**2. ¿Qué parte de la palabra construí?**
Construí un fragmento inicial de la palabra (las letras "T - E - N"). Decidí no hacer la palabra completa aún para poder aislar y evaluar la mecánica de los resortes elásticos entre múltiples cuerpos sin que el lienzo se saturara. 

**3. ¿Qué propiedad física manipulé?**
Manipulé tres propiedades de Matter.js:
* **Gravedad (`gravity.y = 0`):** La anulé para que las letras floten y la atención se centre en la tensión horizontal.
* **Rigidez (`stiffness`):** Alteré la capacidad de los *Constraints* (resortes) para mantener las letras juntas.
* **Fuerza Aplicada (`Body.applyForce`):** Inyecté vectores de fuerza física directamente en los cuerpos de las letras de los extremos (T y N) para empujarlas en direcciones opuestas.

**4. ¿Qué aspecto del audio afecta qué comportamiento?**
Implementé un mapeo dual:
* **La Amplitud (Volumen):** Está mapeada continuamente a la rigidez (`stiffness`) de los resortes. A mayor volumen, más duro y rígido es el enlace entre las letras.
* **La Energía de Frecuencias (FFT Bass):** Funciona como un disparador (trigger). Cuando la música da un golpe fuerte de bajo (mayor a 200), el motor aplica una fuerza repulsiva a las letras, simulando una explosión que intenta separarlas, mientras los resortes luchan por devolverlas al centro.

**5. Evaluación: ¿Qué funcionó y qué no para el significado?**
* **Lo que SÍ funcionó:** La sensación de "jalón y rebote" es excelente. Visualmente transmite la idea de un estrés interno; se siente como si el sonido fuera una fuerza disruptiva que quiere destruir la palabra, pero la palabra se niega a romperse. El concepto semántico de "TENSIÓN" se lee perfectamente en la física.
* **Lo que NO funcionó:** Al inyectar fuerzas aleatorias, los cuerpos (cajas) rotan libremente sin control (`l.angle`). Después de un par de golpes de bajo fuertes, las letras quedan de cabeza o totalmente torcidas, lo que destruye la legibilidad de la palabra y arruina la función tipográfica. 
* **Solución para la pieza final:** Deberé bloquear la rotación de los cuerpos en Matter.js usando la propiedad `{ inertia: Infinity }` al crear las cajas, obligando a las letras a mantenerse siempre verticales, asegurando que la palabra soporte la violencia física pero siga siendo legible como texto.

## Bitácora de aplicación 


## Bitácora de reflexión
