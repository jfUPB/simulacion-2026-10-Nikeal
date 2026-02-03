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


## Bitácora de aplicación 



## Bitácora de reflexión

