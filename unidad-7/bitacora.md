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

1. Palabra elegida.

- Pulso

2. Justificación conceptual.

Elegí "PULSO" porque es una palabra que denota vitalidad, ritmo y la tensión inherente entre la expansión y la contracción (como un corazon). El pulso es la evidencia física de la vida y funciona a través de opuestos: la sístole (latido) y la diástole (relajación). Conceptualmente, busco transformar un elemento tipográfico estático en un organismo vivo (como un corazon, un musculo), donde el motor de físicas y el analizador de espectro de audio actúen literalmente como el sistema cardiovascular de la pieza tipográfica.

3. Análisis de su significado visual y comportamental.

Visual: La palabra necesita sentirse como un núcleo muscular pesado, por lo que utilizo una tipografía condensada y gruesa (Impact). Para transmitir el "latido", su estado base es oscuro (casi negro con tintes rojo sangre), y solo en el milisegundo exacto de la percusión detona en un destello incandescente blanco/rosado, generando un contraste visual extremo que refuerza la idea de un bombeo de sangre repentino.

Comportamental: La palabra no "cae" ni flota libremente. Su estado natural es estar fuertemente anclada al centro bajo extrema tensión. Cuando la música golpea, las letras sufren una explosión radial hacia afuera, pero son obligadas a retraerse violentamente a su posición original, simulando el comportamiento elástico de un músculo cardíaco.

4. Moodboard o referencias.

Para orientar el estilo, el color y la atmósfera de la pieza, me basé en los siguientes referentes visuales:

Monitores de signos vitales (ECG)

<img width="1100" height="860" alt="image" src="https://github.com/user-attachments/assets/89494d2b-767d-4f57-974c-026e6c2991a7" />

Fibras musculares/Tendones

<img width="621" height="254" alt="image" src="https://github.com/user-attachments/assets/75f905f3-6e53-468d-ba15-4f28d008dbb3" />

Estroboscopios

<img width="600" height="600" alt="image" src="https://github.com/user-attachments/assets/b1082a35-c2d7-42df-bb37-71eb5a44f847" />

5. Bocetos.

<img width="1112" height="1600" alt="image" src="https://github.com/user-attachments/assets/5e3e5db1-288f-4d87-83df-2fd6bd1f8db8" />
<img width="1112" height="1600" alt="image" src="https://github.com/user-attachments/assets/969056d8-cb2f-4946-b1e7-6f5db9718f6f" />

6. Mapa de decisiones.

Para mí, era importante el apartado visual y sustematico tuviera una razón de ser conectada con el concepto de un "corazon". No quería cosas moviéndose al azar; todo debía sentirse orgánico y tensionado. Así conecté la forma con el significado:

Tipografía pesada y condensada (Impact): Visualmente, necesitaba que la palabra tuviera "masa". Letras delgadas se verían frágiles, pero letras gruesas y apretadas transmiten la sensación de músculo, de un núcleo denso que está acumulando presión.

Físicas de ingravidez (Gravedad cero e Inercia infinita): Decidí apagar la gravedad porque no quería que la palabra "cayera". Quería que la atención del espectador se centrara exclusivamente en la fuerza radial (el latido hacia afuera). Además, bloqueé la rotación de las letras; el músculo se estira, pero no pierde su forma, manteniendo la palabra siempre legible en medio del caos.

Resortes virtuales de alta rigidez (Constraints): Esta es la clave de la elasticidad de la pieza. Las letras no se separan suavemente; están atadas al centro por fuerzas casi inflexibles. Esto imita la sístole del corazón: la explosión hacia afuera es violenta, pero el regreso al centro es un latigazo brutal.

Fondo de alto contraste (Motion blur): Oscurecí el entorno al máximo y le dejé un rastro visual a las letras. Esto hace que el estado de reposo (diástole) se sienta pesado y oscuro, para que cuando ocurra el latido (el flash luminoso), el impacto visual te golpee directamente.

Controles manuales (Mouse y Espacio): Perturban el sistema natural. Son mi forma de inyectar caos humano (una arritmia o un colapso total) en un sistema que por lo demás late de forma autónoma con la música.

7. Mapa de interpretación.

La diseñé como un instrumento audiovisual para tocar en vivo. Así es como la interpreto durante una presentación:

El latido base (Audio autónomo): Durante la mayor parte de la canción, dejo que el algoritmo haga su trabajo. La música controla el pulso natural de la pieza, marcando el ritmo de la respiración visual sin que yo tenga que intervenir.

El clic del Mouse (La Arritmia): Este es mi acento manual. Si en la canción hay un corte repentino, un drop inesperado o un silencio que el algoritmo no lee con tanta fuerza, hago clic. Esto inyecta una onda expansiva masiva desde mi puntero, sobreescribiendo el sistema y creando un latido hiperviolento a voluntad.

La barra espaciadora (El Infarto): Es el momento de clímax narrativo de mi performance. La uso en esos puentes musicales donde la tensión sube antes de la explosión final. Al presionarla, "corto" literalmente los tendones físicos (bajo la rigidez a cero). La palabra se desangra, se desarma y las letras quedan flotando a la deriva. Cuando la canción finalmente rompe, suelto la barra y la palabra choca de nuevo en el centro con toda su fuerza.

8. Explicación de la relación entre audio y comportamiento.

Quería evitar el cliché de "poner música y que todo vibre". Necesitaba que el audio y el comportamiento físico estuvieran acoplados anatómicamente. Para lograrlo, dividí la escucha del audio en dos canales con propósitos totalmente distintos:

El impulso mecánico (Frecuencias Graves / p5.FFT): Aislar los bajos fue crucial. Configuré un umbral muy estricto para que la palabra no se mueva con melodías suaves, sino únicamente cuando golpea la percusión pesada. Cuando ese bajo supera el límite, el código inyecta una fuerza física real (Matter.Body.applyForce()) calculada vectorialmente desde el centro hacia afuera. Es un empuje mecánico, no una simple animación.

El flujo de sangre (Amplitud / p5.Amplitude): El volumen general de la canción no mueve las letras, sino que controla su iluminación. Utilicé una interpolación matemática (lerp) con un decaimiento muy rápido para mapear el volumen al color de las letras. Cuando hay silencio, las letras son de un rojo abisal, casi muertas. Pero en la fracción de segundo que suena el impacto, el volumen inyecta un "flash" blanco y rosado incandescente. Esta dualidad es lo que hace que la pieza realmente se sienta como un latido cardíaco y no como una linterna encendida.

9. Evidencia del uso de IA.

Para este proyecto, tenía claro lo que quería lograr a nivel conceptual y escénico. Toda la metáfora de la relación sístole/diástole, la elección de la palabra "PULSO", la decisión de crear un contraste visual y las lógicas de cómo iba a interpretar la obra en vivo (la "arritmia" y el "infarto") son mis ideas y diseño.

Utilicé a la inteligencia artificial como un asistente técnico y materializador para traducir esas intenciones a matemáticas y código en JavaScript. Específicamente, me apoyé en la IA para:

- Formular la matemática vectorial compleja que calcula la dirección exacta hacia la que debe salir disparada cada letra desde el centro de la pantalla.
- Configurar la sintaxis de las uniones elásticas (Constraints) de Matter.js para lograr ese "latigazo" rápido sin que el motor de físicas colapsara o se rompiera por el exceso de fuerza.
- Estructurar el sistema de renderizado (los push() y pop()) en p5.js para que el efecto visual del rastro (motion blur) funcionara correctamente sin manchar o apagar el brillo del núcleo de las letras.

10. Código fuente.

```js

const WORD             = "PULSO";
const FONT_SIZE        = 148;
const LETTER_GAP       = 22;

const BASS_THRESHOLD   = 160;  
const BEAT_FORCE       = 0.085; 
const BEAT_COOLDOWN    = 40;   
const SCALE_BEAT       = 1.55; 
const CLICK_FORCE      = 0.35; 

const TRAIL_ALPHA      = 40; // Fondo limpia un poco más rápido para oscurecer
const SCALE_IDLE       = 1.0;
const STIFFNESS_NORMAL = 0.10;
const STIFFNESS_OFF    = 0;
const WALL_T           = 80;

let MBodies, MBody, MWorld, MConstraint, MVector;
let mEngine, mWorld;

let letterBodies       = [];
let letterSprings      = [];
let letterData         = [];
let letterScales       = [];
let walls              = [];
let letterTrails       = [];
let satellitesByLetter = [];
let shockwaves         = [];
let wallSplashes       = [];
let lastVelocities     = [];
let ecgHistory         = [];
const ECG_LEN          = 340;
let fibers             = [];
const NUM_FIBERS       = 26;

let soundFile;
let fftAnalyzer, amplitudeAnalyzer;
let audioStarted       = false;
let infartoMode        = false;
let lastBeatTime       = 0;
let redChannel         = 38;
let globalBeat         = 0;
let noiseT             = 0;

function preload() {
  soundFile = loadSound("audio.mp3");
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(RGB, 255);
  textFont("Impact");
  textAlign(CENTER, CENTER);
  noStroke();

  amplitudeAnalyzer = new p5.Amplitude();
  fftAnalyzer       = new p5.FFT(0.85, 512);

  MBodies     = Matter.Bodies;
  MBody       = Matter.Body;
  MWorld      = Matter.World;
  MConstraint = Matter.Constraint;
  MVector     = Matter.Vector;

  mEngine = Matter.Engine.create();
  mWorld  = mEngine.world;
  mEngine.gravity.x = 0;
  mEngine.gravity.y = 0;

  initFibers();
  initECG();
  buildLetters();
  buildWalls();
}

function initFibers() {
  fibers = [];
  for (let i = 0; i < NUM_FIBERS; i++) {
    fibers.push({
      yBase : (i / NUM_FIBERS) * height,
      phase : random(TWO_PI),
      amp   : random(20, 60),
      freq  : random(0.003, 0.009),
      col   : random(0.3, 1.0)
    });
  }
}

function initECG() {
  ecgHistory = [];
  for (let i = 0; i < ECG_LEN; i++) ecgHistory.push(0);
}

function buildWalls() {
  for (let w of walls) MWorld.remove(mWorld, w);
  walls = [];
  let opts = { isStatic: true, restitution: 0.85, friction: 0.0 };
  let t    = WALL_T;
  walls.push(MBodies.rectangle(width / 2,   -t / 2,        width,  t,      opts));
  walls.push(MBodies.rectangle(width / 2,    height + t/2, width,  t,      opts));
  walls.push(MBodies.rectangle(-t / 2,       height / 2,   t,      height, opts));
  walls.push(MBodies.rectangle(width + t/2,  height / 2,   t,      height, opts));
  MWorld.add(mWorld, walls);
}

function buildLetters() {
  letterBodies       = [];
  letterSprings      = [];
  letterData         = [];
  letterScales       = [];
  letterTrails       = [];
  satellitesByLetter = [];
  lastVelocities     = [];

  textSize(FONT_SIZE);
  let widths     = [];
  let totalWidth = 0;
  for (let i = 0; i < WORD.length; i++) {
    let w = textWidth(WORD[i]);
    widths.push(w);
    totalWidth += w + LETTER_GAP;
  }
  totalWidth -= LETTER_GAP;

  let startX  = width  / 2 - totalWidth / 2;
  let centerY = height / 2;
  let cursorX = startX;

  for (let i = 0; i < WORD.length; i++) {
    let ch = WORD[i];
    let lw = widths[i];
    let lh = FONT_SIZE * 1.1;
    let ox = cursorX + lw / 2;
    let oy = centerY;

    let body = MBodies.rectangle(ox, oy, lw * 0.8, lh * 0.82, {
      restitution : 0.88,
      friction    : 0.0,
      frictionAir : 0.04,
      inertia     : Infinity,
      label       : ch
    });
    MWorld.add(mWorld, body);
    letterBodies.push(body);

    let spring = MConstraint.create({
      bodyA     : body,
      pointB    : MVector.create(ox, oy),
      length    : 0,
      stiffness : STIFFNESS_NORMAL,
      damping   : 0.065
    });
    MWorld.add(mWorld, spring);
    letterSprings.push(spring);

    letterData.push({ char: ch, ox, oy, lw, lh });
    letterScales.push(SCALE_IDLE);
    letterTrails.push([]);
    lastVelocities.push({ x: 0, y: 0 });

    let sats    = [];
    let numSats = floor(random(5, 10));
    for (let s = 0; s < numSats; s++) {
      sats.push({
        angle      : random(TWO_PI),
        radius     : random(lw * 0.6, lw * 1.1),
        orbitSpeed : random(0.012, 0.032) * (random() > 0.5 ? 1 : -1),
        size       : random(1.5, 4.5),
        alpha      : random(30, 90),
        phase      : random(TWO_PI)
      });
    }
    satellitesByLetter.push(sats);
    cursorX += lw + LETTER_GAP;
  }
}

function draw() {
  push();
    noStroke();
    fill(5, 1, 3, TRAIL_ALPHA);
    rect(0, 0, width, height);
  pop();

  Matter.Engine.update(mEngine, deltaTime);
  noiseT += 0.004;

  let lvl        = 0;
  let bassEnergy = 0;
  
  if (audioStarted) {
    lvl        = amplitudeAnalyzer.getLevel();
    
    // Mapeo más suave para el estado general (Diástole)
    redChannel = constrain(map(lvl, 0, 0.40, 20, 150), 20, 150);
    
    // DECAIMIENTO RÁPIDO para que el latido se apague de inmediato
    globalBeat = lerp(globalBeat, 0, 0.15); 

    fftAnalyzer.analyze();
    bassEnergy = fftAnalyzer.getEnergy("bass");

    ecgHistory.push(lvl);
    if (ecgHistory.length > ECG_LEN) ecgHistory.shift();

    if (bassEnergy > BASS_THRESHOLD && millis() - lastBeatTime > BEAT_COOLDOWN) {
      beatPulse(BEAT_FORCE);
      globalBeat   = 1.0; // Inyecta el flash al 100%
      lastBeatTime = millis();
    }
  } else {
    ecgHistory.push(0);
    if (ecgHistory.length > ECG_LEN) ecgHistory.shift();
  }

  drawMuscleTexture(globalBeat);
  drawFibers(globalBeat);
  drawVignette(globalBeat); 
  
  drawShockwaves();
  drawWallSplashes();
  detectBounces();

  // DECAIMIENTO DE ESCALA MÁS RÁPIDO (se desinfla seco como un músculo)
  for (let i = 0; i < letterScales.length; i++) {
    letterScales[i] = lerp(letterScales[i], SCALE_IDLE, 0.18); 
  }

  drawLetterTrails();
  drawSatellites();
  drawLetters(globalBeat); 
  drawECG();

  if (infartoMode) drawInfartoEdges();
  if (!audioStarted) drawStartMessage();
}

function drawMuscleTexture(beat) {
  push();
    noStroke();
    let step = 38;
    for (let x = 0; x < width; x += step) {
      for (let y = 0; y < height; y += step) {
        let n = noise(x * 0.006, y * 0.006, noiseT);
        if (n > 0.58) {
          let a  = map(n, 0.58, 0.85, 1, 5) + beat * 15; // Oscuro en reposo, flash con el beat
          let sz = map(n, 0.58, 0.85, 1, 3);
          fill(constrain(redChannel * 0.45, 10, 120), 3, 6, a);
          ellipse(
            x + noise(x * 0.02, noiseT) * 18,
            y + noise(y * 0.02, noiseT + 50) * 18,
            sz, sz
          );
        }
      }
    }
  pop();
}

function drawFibers(beat) {
  push();
    noFill();
    for (let f of fibers) {
      let baseAlpha = map(f.col, 0.3, 1.0, 1, 8) + beat * 20; 
      let r         = constrain(redChannel * f.col * 0.6, 10, 180);
      stroke(r, 3, 6, baseAlpha);
      strokeWeight(0.5 + beat * 0.8);
      beginShape();
      for (let x = 0; x <= width; x += 6) {
        let n1 = noise(x * f.freq,       f.yBase * 0.003, noiseT + f.phase);
        let n2 = noise(x * f.freq * 2,   f.yBase * 0.005, noiseT * 1.3 + f.phase);
        let dy = (n1 - 0.5) * f.amp * 2 + (n2 - 0.5) * f.amp * 0.5;
        dy *= (1 + beat * 0.4);
        curveVertex(x, f.yBase + dy);
      }
      endShape();
    }
  pop();
}

function detectBounces() {
  for (let i = 0; i < letterBodies.length; i++) {
    let b   = letterBodies[i];
    let vx  = b.velocity.x;
    let vy  = b.velocity.y;
    let lvx = lastVelocities[i].x;
    let lvy = lastVelocities[i].y;
    let dvx = abs(vx - lvx);
    let dvy = abs(vy - lvy);

    if (dvx > 5 || dvy > 5) {
      let px  = b.position.x;
      let py  = b.position.y;
      let mag = sqrt(dvx * dvx + dvy * dvy);

      shockwaves.push({
        x       : px, y: py,
        rx      : 8,  ry: 8,
        targetRx: letterData[i].lw * 2.2 + mag * 3,
        targetRy: letterData[i].lh * 1.4 + mag * 2,
        alpha   : 200,
        col     : constrain(redChannel + 60, 80, 255)
      });

      let splashX = px;
      let splashY = py;
      if (dvx > dvy) {
        splashX = (px < width / 2) ? random(0, 40) : random(width - 40, width);
      } else {
        splashY = (py < height / 2) ? random(0, 40) : random(height - 40, height);
      }
      for (let s = 0; s < 8; s++) {
        wallSplashes.push({
          x    : splashX + random(-35, 35),
          y    : splashY + random(-35, 35),
          r    : random(2, 9),
          alpha: random(80, 200),
          life : random(60, 160)
        });
      }
      letterScales[i] = min(letterScales[i] + 0.3, SCALE_BEAT);
    }
    lastVelocities[i] = { x: vx, y: vy };
  }
}

function drawShockwaves() {
  push();
    noFill();
    for (let i = shockwaves.length - 1; i >= 0; i--) {
      let sw    = shockwaves[i];
      sw.rx     = lerp(sw.rx, sw.targetRx, 0.12);
      sw.ry     = lerp(sw.ry, sw.targetRy, 0.12);
      sw.alpha  = lerp(sw.alpha, 0, 0.07);

      strokeWeight(map(sw.alpha, 200, 0, 2.5, 0.2));
      stroke(sw.col, 10, 15, sw.alpha);
      ellipse(sw.x, sw.y, sw.rx * 2, sw.ry * 2);

      stroke(255, 60, 40, sw.alpha * 0.4);
      strokeWeight(0.8);
      ellipse(sw.x, sw.y, sw.rx, sw.ry);

      if (sw.alpha < 1.5) shockwaves.splice(i, 1);
    }
  pop();
}

function drawWallSplashes() {
  push();
    noStroke();
    for (let i = wallSplashes.length - 1; i >= 0; i--) {
      let s    = wallSplashes[i];
      s.life  -= 1;
      s.alpha  = map(s.life, 0, 160, 0, s.alpha);
      fill(constrain(redChannel + 30, 80, 255), 5, 8, s.alpha);
      ellipse(s.x, s.y, s.r * 2.4, s.r);
      if (s.life <= 0) wallSplashes.splice(i, 1);
    }
  pop();
}

function drawLetterTrails() {
  push();
    noStroke();
    textFont("Impact");
    textAlign(CENTER, CENTER);
    textSize(FONT_SIZE);
    for (let i = 0; i < letterBodies.length; i++) {
      let b  = letterBodies[i];
      let tr = letterTrails[i];
      tr.push({ x: b.position.x, y: b.position.y });
      if (tr.length > 10) tr.shift();

      for (let t = 0; t < tr.length - 1; t++) {
        let a  = map(t, 0, tr.length - 1, 0, 16);
        let sc = map(t, 0, tr.length - 1, 0.75, 0.95);
        fill(constrain(redChannel, 40, 150), 4, 6, a);
        push();
          translate(tr[t].x, tr[t].y);
          scale(sc);
          text(letterData[i].char, 0, 0);
        pop();
      }
    }
  pop();
}

function drawSatellites() {
  push();
    noStroke();
    for (let i = 0; i < letterBodies.length; i++) {
      let b      = letterBodies[i];
      let sats   = satellitesByLetter[i];
      let velMag = MVector.magnitude(b.velocity);
      let sc     = letterScales[i];

      for (let s of sats) {
        s.angle += s.orbitSpeed * (1 + velMag * 0.04);
        let currentRadius = s.radius * sc;
        let sx = b.position.x + cos(s.angle) * currentRadius;
        let sy = b.position.y + sin(s.angle) * currentRadius * 0.55;
        
        let pBeat = map(sc, SCALE_IDLE, SCALE_BEAT, 0, 1);
        pBeat = constrain(pBeat, 0, 1);
        let a  = constrain(s.alpha * pBeat * 1.5, 0, 180); // Solo brillan con el latido

        fill(255, 100, 100, a);
        ellipse(sx, sy, s.size * 2.2, s.size);
        fill(255, 200, 200, a * 0.8);
        ellipse(sx, sy, s.size * 1.0, s.size * 0.35);
      }
    }
  pop();
}

function drawLetters(beat) {
  push();
    textFont("Impact");
    textAlign(CENTER, CENTER);
    textSize(FONT_SIZE);

    for (let i = 0; i < letterBodies.length; i++) {
      let b   = letterBodies[i];
      let d   = letterData[i];
      let sc  = letterScales[i];
      let px  = b.position.x;
      let py  = b.position.y;

      let velMag  = MVector.magnitude(b.velocity);
      let glowAmt = constrain(map(velMag, 0, 22, 0, 150), 0, 150);

      // Flash pBeat nos dice qué tan estirada/latida está la letra (0 a 1)
      let pBeat = map(sc, SCALE_IDLE, SCALE_BEAT, 0, 1);
      pBeat = constrain(pBeat, 0, 1);

      // ── Halo exterior: Solo brilla intenso en el latido ──
      noStroke();
      for (let g = 5; g >= 1; g--) {
        let gs = sc * (1 + g * 0.10);
        let ga = map(g, 5, 1, 0, 15) + (pBeat * 40); 
        let gr = constrain(80 + (pBeat * 175), 30, 255);
        fill(gr, 10, 15, ga);
        push();
          translate(px, py);
          scale(gs);
          text(d.char, 0, 0);
        pop();
      }

      // ── Letra principal: Oscura en reposo, Destello blanco/rosa en el latido ──
      // Interpolación directa de color dependiendo del estado físico de la letra
      let coreR = lerp(80, 255, pBeat); // De rojo oscuro a blanco
      let coreG = lerp(10, 220, pBeat); // De rojo puro a rosa/blanco
      let coreB = lerp(15, 220, pBeat); 
      
      fill(coreR, coreG, coreB, 255);
      
      // Borde: Rojo sangre en reposo, rosado encendido en el latido
      stroke(lerp(120, 255, pBeat), lerp(20, 150, pBeat), lerp(20, 150, pBeat), 150); 
      strokeWeight(1.5);
      
      push();
        translate(px, py);
        scale(sc);
        text(d.char, 0, 0);
      pop();
    }
  pop();

  push();
    noFill();
    for (let i = 0; i < letterBodies.length; i++) {
      let b    = letterBodies[i];
      let d    = letterData[i];
      let px   = b.position.x;
      let py   = b.position.y;
      let dVal = calcDist(px, py, d.ox, d.oy);

      if (dVal > 30) {
        let lineAlpha = constrain(map(dVal, 30, 300, 0, 80), 0, 80);
        stroke(255, 50, 50, lineAlpha); 
        strokeWeight(map(dVal, 30, 300, 2, 0.5));
        line(px, py, d.ox, d.oy);
      }
    }
  pop();
}

function calcDist(x1, y1, x2, y2) {
  return sqrt((x2 - x1) * (x2 - x1) + (y2 - y1) * (y2 - y1));
}

function drawECG() {
  push();
    let ecgH = 65;
    let ecgY = height - ecgH - 16;
    let ecgX = 40;
    let ecgW = width - 80;

    noStroke();
    fill(4, 0, 2, 100);
    rect(ecgX - 8, ecgY - 8, ecgW + 16, ecgH + 16, 6);

    stroke(50, 4, 6, 55);
    strokeWeight(0.5);
    line(ecgX, ecgY + ecgH / 2, ecgX + ecgW, ecgY + ecgH / 2);

    noFill();
    beginShape();
    for (let i = 0; i < ecgHistory.length; i++) {
      let x = map(i, 0, ecgHistory.length - 1, ecgX, ecgX + ecgW);
      let v = ecgHistory[i];
      let distorted = v < 0.1 ? v * 0.5 : v * 3.5 - v * v * 8;
      distorted = constrain(distorted, -0.2, 1.2);
      let y     = ecgY + ecgH / 2 - distorted * (ecgH * 0.44);
      let alpha = map(i, 0, ecgHistory.length, 20, 210);
      let r     = constrain(redChannel + map(v, 0, 0.6, 0, 80), 38, 255);
      stroke(r, 40, 40, alpha); 
      strokeWeight(map(v, 0, 0.6, 1.2, 3.0));
      vertex(x, y);
    }
    endShape();

    let lv = ecgHistory[ecgHistory.length - 1];
    let ld = lv < 0.1 ? lv * 0.5 : lv * 3.5 - lv * lv * 8;
    ld = constrain(ld, -0.2, 1.2);
    let headY = ecgY + ecgH / 2 - ld * (ecgH * 0.44);
    noStroke();
    fill(255, 100, 100, 255);
    ellipse(ecgX + ecgW, headY, 6, 6);
    fill(255, 50, 50, 80);
    ellipse(ecgX + ecgW, headY, 15, 15);
  pop();
}

function drawVignette(beat) {
  push();
    noStroke();
    let steps = 20;
    for (let i = steps; i >= 0; i--) {
      let t = i / steps;
      let a = (60 + beat * 40) * (1 - t) * (1 - t) * (1 - t); 
      fill(4, 0, 2, a);
      let m = t * min(width, height) * 0.6;
      rect(m, m, width - m * 2, height - m * 2);
    }
  pop();
}

function drawInfartoEdges() {
  push();
    noStroke();
    let a     = map(sin(frameCount * 0.09), -1, 1, 30, 120);
    let thick = 5;
    fill(255, 50, 50, a); 
    rect(0, 0, width, thick);
    rect(0, height - thick, width, thick);
    rect(0, 0, thick, height);
    rect(width - thick, 0, thick, height);

    fill(255, 100, 100, map(sin(frameCount * 0.07), -1, 1, 100, 200));
    textFont("Impact");
    textSize(11);
    textAlign(RIGHT, TOP);
    text("INFARTO", width - 14, 14);
    textAlign(CENTER, CENTER);
  pop();
}

function beatPulse(forceMag) {
  let cx = width  / 2;
  let cy = height / 2;
  for (let i = 0; i < letterBodies.length; i++) {
    let b    = letterBodies[i];
    let px   = b.position.x;
    let py   = b.position.y;
    let dx   = px - cx;
    let dy   = py - cy;
    let d    = sqrt(dx * dx + dy * dy);
    if (d < 6) {
      dx = letterData[i].ox - cx;
      dy = letterData[i].oy - cy;
      d  = sqrt(dx * dx + dy * dy) || 1;
    }
    let nx = dx / d;
    let ny = dy / d;
    let tx = -ny * 0.3;
    let ty =  nx * 0.3;
    MBody.applyForce(b, b.position, MVector.create(
      (nx + tx) * forceMag,
      (ny + ty) * forceMag * 0.65
    ));
    letterScales[i] = SCALE_BEAT;
  }
}

function mousePressed() {
  if (!audioStarted) {
    if (!fullscreen()) fullscreen(true);
    soundFile.loop();
    audioStarted = true;
    return;
  }
  for (let i = 0; i < letterBodies.length; i++) {
    let b  = letterBodies[i];
    let dx = b.position.x - mouseX;
    let dy = b.position.y - mouseY;
    let d  = sqrt(dx * dx + dy * dy) || 1;
    MBody.applyForce(b, b.position,
      MVector.create((dx / d) * CLICK_FORCE, (dy / d) * CLICK_FORCE)
    );
    letterScales[i] = SCALE_BEAT * 1.3;
  }
  shockwaves.push({
    x: mouseX, y: mouseY,
    rx: 10, ry: 10,
    targetRx: width * 0.6, targetRy: height * 0.5,
    alpha: 180, col: 255
  });
}

function keyPressed() {
  if (key === ' ') {
    infartoMode = !infartoMode;
    for (let s of letterSprings) {
      s.stiffness = infartoMode ? STIFFNESS_OFF : STIFFNESS_NORMAL;
    }
    if (!infartoMode) {
      for (let i = 0; i < letterBodies.length; i++) {
        MBody.applyForce(
          letterBodies[i], letterBodies[i].position,
          MVector.create(random(-0.012, 0.012), random(-0.012, 0.012))
        );
        letterScales[i] = SCALE_BEAT;
      }
      shockwaves.push({
        x: width/2, y: height/2,
        rx: 10, ry: 10,
        targetRx: width * 1.1, targetRy: height * 1.1,
        alpha: 220, col: constrain(redChannel + 80, 100, 255)
      });
    }
  }
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
  MWorld.clear(mWorld);
  Matter.Engine.clear(mEngine);
  mEngine = Matter.Engine.create();
  mWorld  = mEngine.world;
  mEngine.gravity.x = 0;
  mEngine.gravity.y = 0;
  infartoMode  = false;
  shockwaves   = [];
  wallSplashes = [];
  initFibers();
  buildLetters();
  buildWalls();
}

function drawStartMessage() {
  push();
    noStroke();
    fill(4, 0, 2, 215);
    rect(0, 0, width, height);
  pop();

  push();
    noFill();
    for (let f of fibers) {
      stroke(100, 20, 30, 20); 
      strokeWeight(0.5);
      beginShape();
      for (let x = 0; x <= width; x += 8) {
        let n = noise(x * f.freq, f.yBase * 0.003, noiseT + f.phase);
        curveVertex(x, f.yBase + (n - 0.5) * f.amp * 1.6);
      }
      endShape();
    }
  pop();

  push();
    noStroke();
    let p  = map(sin(frameCount * 0.045), -1, 1, 0, 1);
    let sc = lerp(1.0, 1.06, p);
    let r  = lerp(60, 120, p); // Color de espera también reposado
    fill(r, 15, 20, 255);
    textFont("Impact");
    textSize(FONT_SIZE);
    translate(width / 2, height / 2);
    scale(sc);
    text(WORD, 0, 0);
  pop();

  push();
    stroke(150, 50, 50, 150);
    strokeWeight(1);
    let divW = 300;
    line(width/2 - divW/2, height/2 + FONT_SIZE * 0.65,
         width/2 + divW/2, height/2 + FONT_SIZE * 0.65);
  pop();

  push();
    noStroke();
    let aPulse = map(sin(frameCount * 0.08), -1, 1, 160, 255);
    fill(255, aPulse); 
    textFont("Impact");
    textSize(19);
    text("HAZ CLIC PARA INICIAR", width / 2, height / 2 + FONT_SIZE * 0.86);

    fill(200, 100, 100, 200);
    textSize(11);
    textFont("Arial");
    text("ESPACIO · infarto / recuperación   ·   CLIC · arritmia",
         width / 2, height / 2 + FONT_SIZE * 0.86 + 32);
  pop();
}
```

11. Enlace al sketch.

- Audio latido: https://editor.p5js.org/Nikeal/sketches/5sWmMT_hA
- Audio Cancion: https://editor.p5js.org/Nikeal/sketches/onluH9KjI

12. Capturas o registros de la pieza.

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/fe525746-87da-47c3-a334-2185e0a1ff40" />
<img width="851" height="773" alt="image" src="https://github.com/user-attachments/assets/353772d7-26fd-4cac-ae67-3865999da517" />
<img width="854" height="770" alt="image" src="https://github.com/user-attachments/assets/63b11ea2-4a96-4e0b-9edc-e3d14e99ea5d" />


## Bitácora de reflexión
