# Unidad 1

## Bitácora de proceso de aprendizaje

### **Actividad 1**
- "Piensa y describe en una sola frase y en tus propias palabras cómo la aleatoriedad influye en el arte generativo."
- La aleatoriedad influye en la creacion de ideas y patrones, que surgen de la misma fuente donde pueden surgir distintas posibilidades que contienen la esencia, dandole asi al arte y codigo su propio caracter.

### **Actividad 2**

- Realiza el siguiente experimento y reporta los resultados en tu bitácora:

Codigo inicial:

let walker; // Declara una variable global llamada `walker`. Aquí se guardará una instancia (objeto) de la clase `Walker`.

function setup() {  // Función especial de p5.js que se ejecuta una sola vez al inicio del programa.
  createCanvas(640, 240); // Crea un lienzo (canvas) de 640 píxeles de ancho y 240 de alto.
  walker = new Walker(); // Crea un nuevo objeto de la clase Walker y lo guarda en la variable walker.
  background(255); // Pinta el fondo del lienzo de blanco
}

function draw() { // Función que se ejecuta en bucle*, muchas veces por segundo
  walker.step(); // Llama al método step() del objeto walker, que cambia su posición.
  walker.show(); // Llama al método show() del objeto walker, que lo dibuja en pantalla.
}

class Walker { // Define una clase llamada Walker.
  constructor() { // Función especial que se ejecuta cuando se crea un nuevo Walker.
    this.x = width / 2; // Inicializa la posición del caminante en el centro del canvas, variables de p5.js
    this.y = height / 2; // coordenadas del caminante
  }

  show() { // Método que se encarga de dibujar el caminante.
    stroke(0); // Define el color del dibujo
    point(this.x, this.y); // Dibuja un punto en la posición actual del caminante.
  }

  step() { // Método que hace que el caminante se mueva.
    const choice = floor(random(4)); // Genera un número aleatorio entero entre 0 y 3: random(4) → número aleatorio entre 0 y 3.999… / floor() → lo redondea hacia abajo
    if (choice == 0) { // Si el número es 0, el caminante se mueve 1 píxel a la derecha.
      this.x++;
    } else if (choice == 1) { // Si es 1, se mueve 1 píxel a la izquierda.
      this.x--;
    } else if (choice == 2)  // Si es 2, se mueve 1 píxel hacia abajo.
      this.y++;
    } else { // Si es 3, se mueve 1 píxel hacia arriba.
      this.y--;
    }
  }
}

1. Modifica el código del ejemplo Example 0.1: A Traditional Random Walk.

Modifique el modulo step, para que tenga una mayor probabilidad de moverse a la derecha sobre otras direcciones.

step() {
  const choice = random(1);

  if (choice < 0.4) {
    this.x++;
  } else if (choice < 0.6) {
    this.x--;
  } else if (choice < 0.8) {
    this.y++;
  } else {
    this.y--;
  }

2. Antes de ejecutar el código, escribe en tu bitácora qué esperas que suceda.

- Espero que se mueva con mas frecuencia hacia la derecha, de forma aleatoria pero con una tendencia notable.

3. Ejecuta el código y escribe en tu bitácora qué sucedió realmente.

Al ejecutar el codigo, el puntero se seguia moviendo de forma aleatoria, pero no esperaba que la tendencia de que fuera a la derecha fuera tan notable.

4. Ocurrió lo que esperabas? ¿Por qué crees que sí o por qué crees que no? Aunque el resultado fue un poco "extremo", si ocurrio el resultado que esperaba, porque cumplio con la hipotesis que habia planteado, ya que se movia de forma aleatoria pero con una tendencia de ir a la derecha.

### **Actividad 3**

1. En tus propias palabras cuál es la diferencia entre una distribución uniforme y una no uniforme de números aleatorios.
   
-La distribucion uniforme es donde todos los valores tienen la misma probabilidad de que ocurran, en el ejemplo pasado cuando se usa random(100), significa que cualquier valor entre 0 y 100 puede pasar con la misma probabilidad, haciendo que se vean de forma "homogenea".

- La distribucion no uniforme ocurre cuando algunos valores tienen mas probabilidad de ocurrir que otro, un ejemplo de esto es la modificacion que hice en la actividad 2, donde el puntero tiende a la derecha. En la distribucion gaussiana, los valores cerca a la media pasan con mas frecuencia pero los valores externos pasan pocas veces, haciendo que se vean patrones en ciertas zonas.

2. Modifica el código de la caminata aleatoria para que utilice una distribución no uniforme, favoreciendo el movimiento hacia la derecha.

step() {
  let r = random(10);

  if (r < 3) {
    this.x++;        
  } else if (r < 5) {
    this.x--;        
  } else if (r < 7) {
    this.y++;       
  } else {
    this.y--;        
  }

Probabilidades de movimiento:

r < 3 → movimiento a la derecha (this.x++)
30% de probabilidad

3 ≤ r < 5 → movimiento a la izquierda (this.x--)
20% de probabilidad

5 ≤ r < 7 → movimiento hacia abajo (this.y++)
20% de probabilidad

r ≥ 7 → movimiento hacia arriba (this.y--)
30% de probabilidad

<img width="203" height="130" alt="image" src="https://github.com/user-attachments/assets/ecfb0519-4704-4012-b97f-126843341e2b" />

### **Actividad 4**

1. Crea un nuevo sketch en p5.js que represente una distribución normal.

2. Copia el código en tu bitácora.

function setup() {
  createCanvas(640, 240);
  background(255);
}

function draw() {
  translate(width / 2, height / 2);

  // Distancia con distribución normal
  let r = abs(randomGaussian(0, 40));

  // Invertir la distribución: empujar hacia los bordes
  let maxR = min(width, height) / 2;
  let invertedR = map(r, 0, 120, maxR, 0);

  // Ángulo que apunta hacia esquinas
  let angle = random([PI / 4, 3 * PI / 4, 5 * PI / 4, 7 * PI / 4]);

  let x = invertedR * cos(angle);
  let y = invertedR * sin(angle);

  // Color aleatorio en escala de azules
  let blueValue = random(0, 255);
  let redValue = random(0, 255);
  fill(redValue, 0, blueValue, 30);
  noStroke();

  circle(x, y, 4);
}


3. Coloca en enlace a tu sketch en p5.js en tu bitácora.

- https://editor.p5js.org/Nikeal/sketches/NddHBQ3SO

4. Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.

<img width="279" height="198" alt="image" src="https://github.com/user-attachments/assets/c940b70b-5fcd-4ee8-b454-1cfdbf8430f6" />

### **Actividad 5**

1. Explica por qué usaste esta técnica y qué resultados esperabas obtener.

Use esta tecnica porque me permite introducir movimientos "inesperados" y poco repetitivos que no se asemejan al movimiento uniforme ni gaussiano, con lo cual buscaba un movimiento mas "natural, acercandose al comportamiento de algunos sistemas fisicos y biologicos, agregando ese suceso inesperado que no se suele ver en sistemas controlados o computacionales.

2. Copia el código en tu bitácora.

let walker;

function setup() {
  createCanvas(640, 240);
  background(255);
  walker = new Walker();
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  step() {
    let r = random(1);
    let xstep, ystep;

    // Lévy flight: pequeña probabilidad de salto grande
    if (r < 0.01) {
      xstep = random(-100, 100);
      ystep = random(-100, 100);
    } else {
      xstep = random(-2, 2);
      ystep = random(-2, 2);
    }

    this.x += xstep;
    this.y += ystep;

    // Mantener dentro del lienzo
    this.x = constrain(this.x, 0, width);
    this.y = constrain(this.y, 0, height);
  }

  show() {
    noStroke();
    fill(0, 30);
    circle(this.x, this.y, 4);
  }
}

3. Coloca en enlace a tu sketch en p5.js en tu bitácora.

https://editor.p5js.org/Nikeal/sketches/ZzKCG_M8A

4. Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.

<img width="633" height="238" alt="image" src="https://github.com/user-attachments/assets/db628a6f-8a2c-4868-9b80-58fb9351d015" />

### Actividad 6

*Ruido Perlin*

1. Crea un nuevo sketch en p5.js donde los visualices. 

2. Explica el concepto qué resultados esberabas obtener.

- En lugar de usar el ruido Perlin solo para mover un punto en el eje X, voy a verlo como una onda orgánica que se mueve de forma natural y suave a lo largo del tiempo como un conjunto de líneas verticales donde la altura de cada línea está controlada por ruido Perlin, esto crea una forma parecida a montañas u olas o un paisaje vivo.

3.. Copia el código en tu bitácora.

let t = 0;

function setup() {
  createCanvas(640, 240);
  background(255);
}

function draw() {
  background(255);
  stroke(0);
  noFill();

  beginShape();
  let xoff = 0;

  for (let x = 0; x < width; x += 5) {
    let y = noise(xoff, t);
    y = map(y, 0, 1, height, 0);
    vertex(x, y);
    xoff += 0.02;
  }

  endShape();

  t += 0.01;
}

Estoy usando el ruido perlin en el noise() que genera valores suaves y continuos entre 0 y 1 usando dos dimensiones:

xoff → posición horizontal

t → tiempo

Esto me permite que de la forma horizontal sea suave y verticalmente cambie lentamente con el tiempo

4. Coloca en enlace a tu sketch en p5.js en tu bitácora.

https://editor.p5js.org/Nikeal/sketches/mxH5_FalK

5. Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.

<img width="634" height="219" alt="image" src="https://github.com/user-attachments/assets/79c6162a-2ed9-4f16-af37-e728a326a770" />

## Bitácora de aplicación 

### Actividad 7

**Creación de obra generativa**

Condiciones: 
1. Usar al menos tres conceptos estudiados en esta unidad COMBINADOS de manera creativa y coherente.
2. Tu obra de ser interactiva y generativa en tiempo real. Puedes usar el mouse, el teclado o cualquier otro sensor de entrada para interactuar con la obra.

Entregable:
1. Un texto donde expliques el concepto de obra generativa.

- Una obra generativa es una pieza de arte que sigue un sistema de reglas programadas, donde el diseñador crea las bases o reglas del sistema por el cual se rige, en lugar de controlar directamente el resultado final. Esta obra se genera en tiempo real mediante procesos como la aleatoriedad, el ruido Perlin y la interacción con el usuario, haciendo que cada ejecución sea única. En este tipo de obras, el código funciona como un agente creativo que produce variaciones constantes dentro de un marco definido por el artista.

- Concepto de la obra:
Buscaba presentra a un conjunto de entidades "vivas" que se desplazan suavemente por el espacio, me inspire en los laboratorios de biologia, donde se veian bacterias en el microscopio. Cada partícula tiene un movimiento orgánico controlado por ruido Perlin, que trata de simular movimientos organicos. El usuario puede intervenir moviendo el mouse, lo que altera el comportamiento de las partículas y modifica la composición visual en tiempo real.

2. Copia el código en tu bitácora.

```
let walkers = []; // Variable Goblal
let maxWalkers = 80; // Array que impone y almacena el # maximo de "criaturas" del sistema.

function setup() { // Función que se ejecuta una sola vez al inicio.
  createCanvas(640, 240); // Crea el canva
  background(255); // Fondo blanco
  colorMode(HSB, 360, 100, 100, 100); // Se usa modo HSB en vez del RGB para controlar el color de forma más orgánica.

  for (let i = 0; i < 50; i++) { // Crea 50 criaturas iniciales en posiciones aleatorias.
    walkers.push(new Walker(random(width), random(height)));
  } // push agrega un nuevo elemento al final del arreglo
}

function draw() { // Función que se ejecuta continuamente en tiempo real.
  background(0, 0, 100, 20); // Fondo semitransparente que deja el rastro del movimiento.

  for (let w of walkers) { // Cada criatura calcula su movimiento y se dibuja en pantalla, la w representa una criatura distinta
    w.step(); // llama al metodo step
    w.show(); // llama al metodo show
  }

  if (random(1) < 0.01 && walkers.length > 20) {  // Ocasionalmente una criatura desaparece. Random genera un número aleatorio entre 0 y 1 y 0.01 =1% && walkers.length es la cantidad actual de criaturas.
    walkers.splice(floor(random(walkers.length)), 1); // # = walkers.lenght, genera un numero aleatorio y floor lo redondea, splice modifica walkers (primero argumento = posicion, 1 # a eliminar)
  }

  if (random(1) < 0.02 && walkers.length < maxWalkers) { // Ocasionalmente nace una nueva criatura.
    walkers.push(new Walker(random(width), random(height))); // crea una nueva instancia a la clase walker (aleatoria x y aleatoria Y), el push añade esto al final del arreglo walkers.
  }
}

class Walker {  // Define el comportamiento de cada criatura.
  constructor(x, y) { // Se ejecuta cuando nace una criatura.
    this.x = x; // Posición inicial.
    this.y = y; // Posición inicial.

    this.tx = random(1000); // Offsets del ruido Perlin.
    this.ty = random(1000); // Offsets del ruido Perlin.

    this.speed = random(0.5, 1.5); // Velocidad individual.
    this.size = random(2, 5); // Tamaño del punto.

    // Color inicial aleatorio.
    this.hue = random(360); // color base aleatorio
    this.sat = random(50, 100); // saturacion
    this.bri = random(60, 100); // brillo
  }

  step() { // define el método que controla el comportamiento
    let angle = noise(this.tx, this.ty) * TWO_PI * 2; // Calcula una dirección usando ruido Perlin, noise(this.tx, this.ty) devuelve un valor suave entre 0 y 1, TWO_PI = 360
    let vx = cos(angle) * this.speed; // Convierte el ángulo en movimiento. 
    let vy = sin(angle) * this.speed; // Convierte el ángulo en movimiento.
    // cos y sin convierten el ángulo en movimiento, this.speed controla la magnitud del desplazamiento

    let d = dist(this.x, this.y, mouseX, mouseY); // Calcula la distancia al cursor.
    if (d < 120) { // Si el mouse está cerca, zona de influencia
      let escapeAngle = atan2(this.y - mouseY, this.x - mouseX); // Calcula la dirección opuesta al mouse, atan2() calcula el ángulo entre dos puntos.
      let force = map(d, 0, 120, 3, 0); // Mientras más cerca el mouse, más fuerte es la reaccion, map() traduce distancia en fuerza
      vx += cos(escapeAngle) * force; // La criatura huye
      vy += sin(escapeAngle) * force; // La criatura huye
    } // Se suma la huida al movimiento base

    this.x += vx; // Se mueve la criatura segun su velocidad
    this.y += vy; // Se mueve la criatura segun su velocidad
    
    // this.x es la posición horizontal actual de la criatura, vx es la velocidad horizontal calculada antes, += significa sumar y guardar

    this.tx += 0.01; // Avanza el tiempo del ruido Perlin.
    this.ty += 0.01; // Avanza el tiempo del ruido Perlin.
    
    // tx es el offset del ruido Perlin, al incrementarlo, aumentamos el tiempo del ruido, el 0.01 hace un cambio suave

    if (this.x > width) this.x = 0; // Las criaturas reaparecen por el lado opuesto.
    if (this.x < 0) this.x = width;
    if (this.y > height) this.y = 0; // Las criaturas reaparecen por el lado opuesto.
    if (this.y < 0) this.y = height; 
    
    // width es el ancho del canvas, height es el alto del canvas

    // Salto de Lévy en el color
    if (random(1) < 0.01) { // Evento raro.
      this.hue = random(360); // Cambio aleatorio de color.
      this.sat = random(50, 100); // Cambio aleatorio de color.
      this.bri = random(60, 100); // Cambio aleatorio de color.
    } else { // Cambio suave la mayor parte del tiempo
      this.hue = (this.hue + 0.2) % 360; // El tono aumenta muy lentamente, % 360 evita que el valor se salga del rango, operador % devuelve el resto de una división
    }
  }

   show() { // Define el método encargado de dibujar la criatura
    strokeWeight(this.size); // Tamaño del punto.
    stroke(this.hue, this.sat, this.bri, 80); // Color con transparencia.
    point(this.x, this.y); // Dibuja la criatura.
  }
}
```

4. Coloca en enlace a tu sketch en p5.js en tu bitácora.

https://editor.p5js.org/Nikeal/sketches/1zTYVG7ku
   
5. Selecciona una captura de pantalla de tu sketch y colócala en tu bitácora.

<img width="666" height="242" alt="image" src="https://github.com/user-attachments/assets/09ac0e18-c48b-43c3-aa0d-c610c6b81d22" />

## Bitácora de reflexión


















