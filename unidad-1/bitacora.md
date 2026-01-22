# Unidad 1

## Bitácora de proceso de aprendizaje

- **Actividad 1**
- "Piensa y describe en una sola frase y en tus propias palabras cómo la aleatoriedad influye en el arte generativo."
La aleatoriedad influye en la creacion de ideas y patrones, que surgen de la misma fuente donde pueden surgir distintas posibilidades que contienen la esencia, dandole asi al arte y codigo su propio caracter.

- **Actividad 2**

- Realiza el siguiente experimento y reporta los resultados en tu bitácora:

Codigo inicial:

// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

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

## Bitácora de aplicación 



## Bitácora de reflexión


