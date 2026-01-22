# Unidad 1

## Bitácora de proceso de aprendizaje

### **Actividad 1**
- "Piensa y describe en una sola frase y en tus propias palabras cómo la aleatoriedad influye en el arte generativo."
La aleatoriedad influye en la creacion de ideas y patrones, que surgen de la misma fuente donde pueden surgir distintas posibilidades que contienen la esencia, dandole asi al arte y codigo su propio caracter.

### **Actividad 2**

- Realiza el siguiente experimento y reporta los resultados en tu bitácora:

Codigo inicial:

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
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

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 1) {
      this.x--;
    } else if (choice == 2) {
      this.y++;
    } else {
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


## Bitácora de aplicación 



## Bitácora de reflexión







