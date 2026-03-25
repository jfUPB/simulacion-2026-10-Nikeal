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

### Actividad 2

Comparación con Example 4.2:

¿Qué responsabilidades que antes estaban en draw() ahora están dentro de la clase Emitter?

Responsabilidades trasladadas: En el Ejemplo 4.2, el ciclo principal draw() era el encargado absoluto de todo: instanciar cada nueva partícula, guardarla en el arreglo, recorrer ese arreglo al revés, llamar a las funciones de actualización y dibujo, y eliminar las partículas muertas con splice(). En el 4.4, todas estas tareas micro-administrativas se delegan a la clase ParticleSystem (o Emisor). El draw() ahora solo hace una cosa: decirle a los emisores que ejecuten su ciclo (run()).

¿Cuál es la ventaja de encapsular la lógica de emisión en una clase separada?

Ventaja de encapsular (El Emisor): La mayor ventaja es la modularidad y escalabilidad. Al encapsular la lógica, el emisor se convierte en una "caja negra" independiente. Esto te permite tener docenas de emisores distintos en la misma escena (una fogata aquí, una cascada allá, chispas de una explosión en otro lado) sin que sus partículas se mezclen y sin convertir tu código principal en un espagueti inmanejable.

En este ejemplo hay un array de emitters. ¿Quién crea los emitters? ¿Quién crea las partículas dentro de cada emitter?

Creación en cadena: El programa principal (el sketch global) es quien crea los Emisores y los almacena en un arreglo maestro (por ejemplo, cada vez que haces clic con el mouse). Luego, cada Emisor individual es responsable de crear las Partículas internamente y guardarlas en su propio sub-arreglo privado.

Dibuja un diagrama que muestre la jerarquía: sketch → [emitters] → [partículas]. ¿Cuántos niveles de “colección” hay?

Jerarquía de Colecciones: Hay exactamente dos niveles de colección en esta arquitectura (un arreglo dentro de otro arreglo).

Nivel 0: Sketch (El mundo global)

Nivel 1: Arreglo de [Emitters] (Colección gestionada por el mundo)

Nivel 2: Arreglo de [Partículas] (Colección gestionada internamente por cada Emisor)

Transferencia conceptual:

Describe este ejemplo usando palabras que NO mencionen p5.js, JavaScript, ni ninguna herramienta específica. Usa solo términos como: entidad, estado, colección, emisor, ciclo de vida, fuerza.

Para entender verdaderamente un sistema, debemos poder explicarlo sin depender del lenguaje de programación. Aquí tienes la descripción conceptual del modelo:

El sistema se compone de una colección principal de emisores. Cada emisor funciona como un gestor localizado en el espacio que tiene la única responsabilidad de administrar una colección secundaria de entidades.

A un ritmo constante, el emisor genera nuevas entidades y les asigna un estado inicial (una coordenada espacial y un impulso). Continuamente, el emisor orquesta a todas sus entidades activas: aplica fuerzas externas que modifican el estado dinámico de cada una (su trayectoria y aceleración) y reduce gradualmente su ciclo de vida. Cuando el ciclo de vida de una entidad se agota por completo, el emisor se encarga de destruirla y liberarla de la memoria del sistema, garantizando que el ecosistema se mantenga estable y eficiente.

### Actividad 3

¿Qué tienen en común las subclases de partículas? ¿Qué tienen de diferente?

Gracias a la herencia (class Confetti extends Particle), todas las subclases comparten el estado físico y vital (posición, velocidad, aceleración y lifespan). También comparten la capa de comportamiento base, es decir, el método update() que aplica el patrón Motion 101 y la condición de muerte isDead().

Difieren en la capa de visualización y en pequeñas alteraciones físicas. Por ejemplo, la subclase Confetti sobrescribe el método display() para dibujarse como un cuadrado rotatorio en lugar de un círculo. Para lograr esa rotación, también añade propiedades únicas a su clase (como un ángulo).

¿Por qué es importante que el Emitter no necesite saber qué tipo específico de partícula está gestionando? Explica esto con tus propias palabras.

¿Por qué es importante que el Emitter no sepa qué tipo específico gestiona?
Esto es el núcleo del Polimorfismo. Si el Emisor tuviera que saber exactamente qué partícula está actualizando, el código estaría lleno de condicionales gigantescos (ej. si es chispa haz esto, si es humo haz esto otro).
Al tratar a todas las entidades genéricamente como "Partículas", el Emisor simplemente itera por su arreglo y les grita: "¡Actualícense y muéstrense!" (p.update(); p.display();). Cada partícula "sabe" cómo dibujarse a sí misma. Esto permite que el sistema sea infinitamente escalable. En un proyecto de entretenimiento digital complejo, un solo emisor puede lanzar escombros, fuego y humo simultáneamente sin que su lógica de gestión colapse.

Si mañana quisieras agregar un tercer tipo de partícula, ¿Qué tendrías que crear y qué NO tendrías que modificar?

¿Qué tendrías que crear? 1. Una nueva clase (ej. class Triangulo extends Particle).
2. Sobrescribir su método display() para que se dibuje de forma distinta.
3. Una pequeña modificación en el punto de creación del Emisor (ej. un random() para decidir si genera una partícula normal, un Confetti o un Triángulo).

¿Qué NO tendrías que modificar? Absolutamente nada de la lógica de gestión. No tocas el bucle inverso que elimina entidades, no modificas la física base (Motion 101), ni alteras cómo el Emisor recorre el arreglo. El motor del sistema permanece ciego a la nueva estética.

Compara con Example 4.2: ¿Cambió la lógica del Emitter? ¿Cambió la lógica de muerte? ¿Qué capa del sistema se modificó y cuáles permanecieron intactas?

¿Cambió la lógica del Emitter o la lógica de muerte? No. La lógica de gestión (recorrer el arreglo al revés, usar splice()) y la lógica de muerte (verificar si lifespan < 0) son idénticas al Ejemplo 4.2.

¿Qué capa se modificó y cuáles quedaron intactas?

Intacta: La Capa de Estructura (el Emisor gestionando la memoria) y la base de la Capa de Comportamiento (la física).

Modificada: La Capa de Visualización (gracias a que cada subclase dibuja algo distinto) y la forma en que el Emisor instancia las partículas (decidiendo aleatoriamente qué subclase meter al arreglo).

### Actividad 4

Entendido. Para que la información te quede perfecta para copiar y organizar en tu bitácora, aquí tienes la Actividad 04 estructurada estrictamente pregunta por pregunta:

### Fuerzas globales vs. locales

**Pregunta:** En Example 4.6, ¿Dónde se define la gravedad? ¿Quién la aplica a las partículas? ¿Es una fuerza global o local?
**Respuesta:** La gravedad se define en el entorno principal (usualmente al inicio del `draw()` o como variable global). Es el ciclo principal el que se la pasa al `ParticleSystem`, y este sistema itera sobre el arreglo para aplicarla a cada partícula mediante el método `applyForce()`. Es una fuerza **global**, ya que afecta a todas las partículas por igual en cada frame.

**Pregunta:** En Example 4.7, ¿Qué diferencia hay entre la gravedad y la fuerza del repeller? ¿Dónde “vive” cada una?
**Respuesta:** La principal diferencia es su alcance y cálculo. La gravedad "vive" en el entorno general y es constante (global). La fuerza del repeller "vive" dentro del objeto `Repeller` y se calcula de forma dinámica y **local** para cada partícula, dependiendo de dónde se encuentre dicha partícula en relación al repulsor.

**Pregunta:** La fuerza del repeller depende de la distancia entre la partícula y el repeller. ¿Qué principio físico se está modelando?
**Respuesta:** Se está modelando la **Ley de Gravitación Universal de Newton** (o leyes inversocuadráticas similares, como la fuerza electrostática de Coulomb). El principio establece que la magnitud de la fuerza es inversamente proporcional al cuadrado de la distancia ($F = \frac{G \cdot m_1 \cdot m_2}{d^2}$). A menor distancia, la fuerza de repulsión se dispara.

**Pregunta:** ¿Cambió la clase Particle entre Example 4.6 y 4.7? ¿Qué implica esto sobre la separación entre comportamiento de la partícula y fuerzas externas?
**Respuesta:** No, la clase `Particle` permaneció exactamente igual. Esto implica que hay una **separación perfecta de responsabilidades (encapsulamiento)**. La partícula solo necesita saber cómo sumar un vector a su aceleración a través de su método `applyForce()`; no le importa de dónde viene la fuerza ni quién la calculó.

---

Tabla comparativa

| Aspecto | 4.2 | 4.4 | 4.5 | 4.6 | 4.7 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **¿Quién crea partículas?** | Bucle `draw()` | `ParticleSystem` | `ParticleSystem` | `ParticleSystem` | `ParticleSystem` |
| **¿Hay clase Emitter?** | No | Sí | Sí | Sí | Sí |
| **¿Hay herencia?** | No | No | Sí | No | No |
| **¿Hay fuerzas externas?** | No | No | No | Sí | Sí |
| **¿Hay interacción entre elementos?**| No | No | No | No | Sí |
| **¿Cómo mueren las partículas?** | `lifespan <= 0` | `lifespan <= 0` | `lifespan <= 0` | `lifespan <= 0` | `lifespan <= 0` |

---

Modificación quirúrgica

**Modificación elegida:** (b) Cambiar las fuerzas sin cambiar la estructura ni la visualización (Transformar el Repulsor en un Atractor).

**Pregunta:** ¿Qué líneas de código tocaste?
Modifiqué la línea donde se normaliza y se define la dirección del vector de fuerza calculada. Le agregué una multiplicación por `-1` para invertir el vector: `dir.normalize(); dir.mult(-1);`.

**Pregunta:** ¿Qué clases/funciones modificaste?
Modifiqué únicamente el método `repel(particle)` dentro de la clase `Repeller`.

**Pregunta:** ¿Qué partes del programa NO necesitaste modificar?
No tuve que modificar la clase `Particle` (ni su física ni su visualización), tampoco la clase `ParticleSystem`, ni la lógica de iteración dentro del `draw()`.

**Pregunta:** ¿Por qué fue posible hacer este cambio sin afectar las demás capas?
Porque el cálculo matemático de la fuerza está aislado en su propia clase. El sistema completo funciona pasando vectores resultantes de un lado a otro. Al cambiar la ecuación en el origen (el Repulsor), las demás capas simplemente reciben un vector diferente y lo procesan con la misma física (Motion 101) de siempre, demostrando un diseño de software altamente modular.


## Bitácora de aplicación 

Documenta el proceso completo:

1. Concepto: 2-3 frases sobre qué ciclo de vida representarás y qué emoción o idea quieres comunicar.

**Ecos y Supernovas**

Esta pieza interactiva representa el ciclo de vida estelar, donde las estrellas nacen en el vacío, se atraen para formar constelaciones y alcanzar su máximo esplendor justo antes de morir. A través de este sistema, se busca comunicar la melancolía de la ley cósmica: la idea de que en el universo, las cosas brillan con mayor intensidad y desesperación en los instantes previos a fragmentarse y apagarse en el vacio infinito, representando lo insignificante que se es en el vasto universo que esta en un constante ciclo de vida y muerte.

2. Bocetos: al menos 2 bocetos (pueden ser a mano) que muestren cómo imaginas la pieza antes de programarla.

<img width="544" height="651" alt="image" src="https://github.com/user-attachments/assets/41fe2e55-30bb-4747-9086-ca4f62d43328" />

3. Mapa de decisiones: para cada elemento del sistema, explica la decisión de diseño: ¿Por qué esa emisión, esas fuerzas, esa condición de muerte, esa visualización, qué significa la interacción del usuario dentro del concepto?

* 1. El Emisor: 
* Elemento: Generación en la base (`height + 30`) y flujo ascendente.
* Decisión de Diseño: Una gravedad inversa que empuja a las estrellas hacia el infinito superior. Representa el surgimiento de la materia desde la nada con el flujo constante que simboliza que el universo no se detiene ante la muerte; es un ciclo de renovacion que no se detiene ante nada, que garantiza que, mientras unas estrellas se apagan en tus manos, otras ya están naciendo, mostrando el infinito frente a lo insignificante del individuo.

* 2. Las Fuerzas:
* Elemento: Campo de flujo (`noise`) cuya velocidad aumenta con el volumen del micrófono.
* Decisión de Diseño: Sustituir el movimiento lineal por corrientes orgánicas y caóticas. El ruido Perlin representa que el espacio no es un vacío muerto, sino un fluido con memoria. Al vincular la voz al "viento cósmico", el usuario se convierte en el **catalizador del caos**. El silencio permite que las estrellas sigan su curso natural, pero el ruido humano agita el éter, obligándolas a bailar con una desesperación que ellas no eligieron. Es la representación de cómo fuerzas externas e incontrolables dictan nuestro camino.

* 3. Las Constelaciones (estrellas)
* Elemento: Líneas de luz y fuerza de atracción por proximidad.
* Decisión de Diseño: Generar conexiones dinámicas según proximidad, dandole un comportamiento mas emergente a los agentes; una estrella sola no dice mucho, pero cuando se conecta con otras empieza a formar algo reconocible. Las líneas aparecen cuando están cerca, como una forma natural de agruparse y representan esa necesidad de conexión. No es algo fijo ni permanente: las conexiones cambian todo el tiempo, simboliza el consuelo de no estar solo en la inmensidad. Sin embargo, estas conexiones son frágiles y desaparecen tan pronto como una de las partes se extingue o se aleja.

* 4. Visualización: brillo y deformaciön
* Elemento: Cambio de tamaño y brillo según el volumen (expansion = vol * 120).
* Decisión de Diseño:  El sonido del usuario afecta directamente cómo se ven las estrellas. Cuando hay más volumen, crecen y brillan más; cuando hay silencio, vuelven a un estado más estable. Esto hace que el sistema se sienta vivo y reactivo, la idea es que el usuario se convierta en un "Dios" o persona externa que afecte al sistema con sus interacciones, en este caso "estresa" o estimula a los agentes del sistema con el sonido irrumpiendo en la paz del mismo.

* 5. Condición de muerte (supernova)

Elemento: Cambio de color y fragmentación en partículas.
Decisión de Diseño: Transición de color para indicar el final del ciclo.

Por qué:
Las estrellas no duran para siempre. A medida que se acercan a su fin, cambian de color hasta desaparecer en fragmentos.
Es una forma visual simple de mostrar el ciclo completo: nacen, evolucionan y terminan. Los fragmentos quedan como rastro, pero no duran mucho, reforzando la idea de que todo es temporal.

* 5. Condición de Muerte: El Ocaso Dorado (Supernova)
* **Elemento:** Cambio de HUE (Azul a Rojo/Dorado) y estallido en `Fragmentos`.
* **Decisión de Diseño:** El color como cronómetro de la **entropía**.
* **El porqué del porqué:** El paso del cian frío al dorado cálido es el indicador visual de que la estrella ha llegado a su límite de vida. Los fragmentos finales (el polvo estelar) son el residuo melancólico de la estructura organizada. Esta fragmentación en el "vacío infinito" refuerza la idea de que todo lo que brilla está destinado a romperse, dejando solo un rastro de luz que pronto se desvanece.

### 6. Interacción: La Paradoja del Observador (Click)
* **Elemento:** Pozo gravitacional que succiona estrellas y resta vida (`lifespan -= 3.5`).
* **Decisión de Diseño:** Una mecánica de control con **costo existencial**.
* **El porqué del porqué:** Esta es la pieza clave de tu narrativa. El usuario, en su afán de "poseer" la belleza o formar la constelación perfecta con el mouse, termina **consumiendo la vida de los astros**. Representa lo insignificante y a la vez destructivo que puede ser el ser humano: en el intento de acercarnos a lo divino (las estrellas), aceleramos su fin. Es una interacción agridulce donde el control es sinónimo de pérdida.

4. Implementación: enlace al código en el editor de p5.js + código fuente en la bitácora.

https://editor.p5js.org/Nikeal/sketches/1dUzZFB4L

Clase Ecosistema
```js
class Ecosistema {
  constructor() {
    this.entidades = [];
  }

  emitirLagrima(x, y) {
    this.entidades.push(new Lagrima(x, y));
  }

  emitirEstallido(x, y, hueBase) {
    let cantidad = floor(random(5, 10));
    for (let i = 0; i < cantidad; i++) {
      this.entidades.push(new Fragmento(x, y, hueBase));
    }
  }

  dibujarConstelaciones() {
    for (let i = 0; i < this.entidades.length; i++) {
      for (let j = i + 1; j < this.entidades.length; j++) {
        let e1 = this.entidades[i];
        let e2 = this.entidades[j];
        if (e1 instanceof Lagrima && e2 instanceof Lagrima) {
          let d = dist(e1.pos.x, e1.pos.y, e2.pos.x, e2.pos.y);
          if (d < 120) {
            let opacidad = map(d, 0, 120, 120, 0);
            stroke(e1.hue, 50, 100, opacidad);
            strokeWeight(map(d, 0, 120, 1.5, 0.1));
            line(e1.pos.x, e1.pos.y, e2.pos.x, e2.pos.y);
            
            if (d > 30) {
              let atraccion = p5.Vector.sub(e2.pos, e1.pos).setMag(0.02);
              e1.aplicarFuerza(atraccion);
              e2.aplicarFuerza(atraccion.mult(-1));
            }
          }
        }
      }
    }
  }

  run(vol) {
    this.dibujarConstelaciones();

    for (let i = this.entidades.length - 1; i >= 0; i--) {
      let ent = this.entidades[i];

      // MECÁNICA: POZO GRAVITACIONAL ACTIVO SOLO CON CLIC
      if (mouseIsPressed) {
        let mouseVec = createVector(mouseX, mouseY);
        let d = p5.Vector.dist(ent.pos, mouseVec);
        
        if (d < 300) { // Radio de alcance un poco más amplio
          let atraccionMouse = p5.Vector.sub(mouseVec, ent.pos);
          let fuerzaMag = map(d, 0, 300, 2.5, 0.1, true);
          atraccionMouse.setMag(fuerzaMag);
          
          ent.aplicarFuerza(atraccionMouse);
          
          // Robo de vida (solo ocurre mientras mantienes el clic)
          ent.lifespan -= 3.5; 
          ent.vel.mult(0.96); // Las frena un poco para que se sientan "atrapadas"
        }
      }

      ent.actualizar(vol);
      ent.mostrar(vol);

      if (ent.isDead()) {
        if (ent instanceof Lagrima) this.emitirEstallido(ent.pos.x, ent.pos.y, ent.hue);
        this.entidades.splice(i, 1);
      }
    }
  }
}
```

Clase Entidades
```js
class EntidadVital {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.masa = 1;
    this.lifespan = random(350, 550);
  }

  aplicarFuerza(fuerza) {
    let f = p5.Vector.div(fuerza, this.masa);
    this.acc.add(f);
  }

  actualizar() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.lifespan -= 1;
  }

  isDead() { return this.lifespan <= 0; }
}

class Lagrima extends EntidadVital {
  constructor(x, y) {
    super(x, y);
    this.masa = random(1.5, 3);
    this.hueBase = random(180, 240);
    this.hue = this.hueBase;
    this.ruidoOffset = random(1000);
    this.radioBase = this.masa * 8;
  }

  actualizar(vol) {
    let angulo = noise(this.pos.x * 0.005, this.pos.y * 0.005, zoff) * TWO_PI * 4;
    let corriente = p5.Vector.fromAngle(angulo);
    corriente.mult(0.04 * this.masa + (vol * 2.5));
    this.aplicarFuerza(corriente);
    this.aplicarFuerza(createVector(0, -vol * 0.8));
    this.vel.limit(1.8 + (vol * 10)); 
    super.actualizar();
  }

  mostrar(vol) {
    push();
    translate(this.pos.x, this.pos.y);
    let alfa = map(this.lifespan, 0, 300, 0, 100);
    
    // Si la vida es baja, el color se torna cálido (desgaste)
    if (this.lifespan < 120) {
      this.hue = lerp(this.hue, 20, 0.04); 
    } else {
      this.hue = lerp(this.hue, this.hueBase, 0.01);
    }
    
    fill(this.hue, 80, 100, alfa + (vol * 150));
    noStroke();
    let expansion = vol * 120; 
    beginShape();
    for (let a = 0; a <= TWO_PI + 0.5; a += 0.5) {
      let r = this.radioBase + expansion + map(noise(a, zoff), 0, 1, -4, 4);
      vertex(r * cos(a), r * sin(a));
    }
    endShape(CLOSE);
    fill(255, alfa + 50);
    circle(0, 0, this.radioBase * 0.4);
    pop();
  }
}

class Fragmento extends EntidadVital {
  constructor(x, y, hueBase) {
    super(x, y);
    this.masa = random(0.5, 1.2);
    this.vel = p5.Vector.random2D().mult(random(2, 5));
    this.hue = hueBase + random(-20, 20);
    this.lifespan = 80;
  }
  actualizar() {
    this.aplicarFuerza(createVector(0, 0.15));
    this.vel.mult(0.97);
    super.actualizar();
  }
  mostrar() {
    push();
    translate(this.pos.x, this.pos.y);
    let alfa = map(this.lifespan, 0, 80, 0, 100);
    fill((this.hue + 40) % 360, 60, 100, alfa);
    noStroke();
    triangle(-2, -2, 2, -2, 0, 4);
    pop();
  }
}
```

Clase sketch
```js
let ecosistema, mic, zoff = 0;
let audioActivo = false;

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100, 100);
  mic = new p5.AudioIn();
  ecosistema = new Ecosistema();
  for(let i = 0; i < 30; i++) ecosistema.emitirLagrima(random(width), random(height));
}

function mousePressed() {
  if (!audioActivo) {
    userStartAudio();
    mic.start();
    audioActivo = true;
  }
}

function draw() {
  blendMode(BLEND);
  background(15, 10, 10, 45);

  let vol = audioActivo ? map(mic.getLevel(), 0, 0.1, 0, 1, true) : 0;
  zoff += 0.005 + (vol * 0.1);

  blendMode(ADD); 
  ecosistema.run(vol);

  if (frameCount % 45 === 0 && ecosistema.entidades.length < 90) {
    ecosistema.emitirLagrima(random(width), height + 30);
  }

  blendMode(BLEND);
  fill(255, 60);
  noStroke();
  textSize(12);
  text(audioActivo ? "AUDIO ON" : "CLICK PARA ACTIVAR", 25, 35);
  
  // Monitor de volumen
  stroke(255, 20);
  noFill();
  rect(25, 45, 100, 4);
  fill(180, 100, 100);
  noStroke();
  rect(25, 45, vol * 100, 4);
  
  fill(255, 40);
  text("MANTÉN CLICK: Pozo gravitacional (Conssume vida).", 25, 70);
}
```

5. Capturas: al menos 3 capturas de momentos diferentes del ciclo de vida.

<img width="785" height="770" alt="image" src="https://github.com/user-attachments/assets/1c3d15aa-ba18-4bb1-a325-aa62f551dd26" />
<img width="784" height="769" alt="image" src="https://github.com/user-attachments/assets/f129cfab-4078-4916-9e69-ab853e03cf8a" />
<img width="787" height="774" alt="image" src="https://github.com/user-attachments/assets/739aa014-16e6-4a83-889e-db674fff436c" />


## Bitácora de reflexión
// https://editor.p5js.org/Nikeal/sketches/YZ7mydzkZ
