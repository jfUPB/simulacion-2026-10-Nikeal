# Unidad 8

## Bitácora de proceso de aprendizaje

### Actividad 1

**1. Herramienta a explorar y por qué**
He decidido utilizar **Blender**, específicamente su entorno de programación visual **Geometry Nodes**. Elijo esta herramienta porque representa la evolución natural de la programación creativa hacia el modelado y la animación 3D. En p5.js logré entender la lógica matemática detrás de sistemas complejos (fuerzas, físicas, ruido), y Geometry Nodes me permite aplicar esa misma lógica procedural para manipular mallas, generar partículas y deformar tipografías en un entorno tridimensional con calidad de renderizado profesional. 

**2. Relación con mi línea de énfasis profesional**
Mi interés profesional dentro de la Ingeniería de Entretenimiento Digital incluye la animación, la dirección de arte y la creación de experiencias audiovisuales. Geometry Nodes es el estándar actual para el "Proceduralismo" (creación de arte mediante algoritmos). Aprender a transferir mis conocimientos a esta herramienta me perfila hacia roles como *Technical Artist* o *Motion Designer*, perfiles altísimamente valorados porque no solo saben "animar a mano", sino que saben construir sistemas inteligentes y automatizados que resuelven problemas visuales complejos.

**3. Referentes realizados con la herramienta**
1.  **Tipografía Procedural y Destrucción (Motion Graphics 3D):** Trabajos de artistas de la comunidad de Blender donde palabras en 3D se desintegran en miles de partículas flotantes utilizando campos de ruido (Noise Textures) y vectores, o se construyen a sí mismas a partir de hilos o cables.
2.  **Visualizadores Audio-Reactivos en 3D:** Entornos abstractos generados con Geometry Nodes donde atributos como la escala de las instancias, la extrusión de las caras de una malla o el comportamiento de un *flow field* están conectados directamente (mediante *Bake Sound to F-Curves* o *drivers*) a las frecuencias de una pista de audio.

**4. ¿Qué me interesa de estos referentes?**
De la tipografía procedural me fascina la materialidad. En p5.js mi tipografía era plana, pero en Blender puedo hacer que la palabra "PULSO" o "TENSIÓN" parezca hecha de cristal que se quiebra, o de carne orgánica que palpita, sumándole la dimensión de los materiales (shaders) y la luz. De los visualizadores audio-reactivos, me interesa la capacidad de generar una atmósfera inmersiva completa; ya no es solo mover objetos en una pantalla, es hacer que todo un ecosistema 3D respire con el diseño sonoro.

**5. Posibles contextos profesionales para la pieza final**
* **Contexto 1 (Dirección de Arte / Animación):** Una secuencia de créditos iniciales (*Title Sequence*) para un proyecto audiovisual (cortometraje o serie) de ciencia ficción o thriller psicológico. La tipografía de los créditos se comportará como un sistema vivo (ej. un campo de flujo o tensión física) que reacciona de forma inquietante a la banda sonora.
* **Contexto 2 (Industria Musical / VJing):** Un *Visualizer* 3D de alta gama o un "Spotify Canvas" iterativo para el lanzamiento del sencillo de un artista musical. El sistema estará diseñado en Geometry Nodes para que, con solo cambiar la pista de audio y la semilla matemática (*seed*), genere un ecosistema visual completamente nuevo pero coherente con la marca del artista.

### Actividad 2

**1. Sistema a transferir**
Voy a transferir el sistema de **Tipografía Semántica Animada**, integrándolo con la lógica de **Sistemas de Partículas y Ruido (Fuerzas/Turbulencia)**. 

**2. Cómo funcionaba ese sistema en p5.js**
En p5.js (y en Matter.js, como en mi entrega de la Unidad 7), este sistema funcionaba creando arreglos (*arrays*) para almacenar las letras o los puntos que conformaban una palabra. En cada *frame* (dentro de la función `draw`), utilizábamos bucles `for` para iterar sobre cada elemento, aplicando vectores matemáticos de fuerza, aceleración o físicas elásticas para alterar sus posiciones (x, y) de forma individual.

**3. Justificación de la transferencia a Blender (Geometry Nodes)**
Tiene sentido transferirlo a Geometry Nodes porque este entorno está diseñado específicamente para hacer esto de forma masiva y optimizada. En lugar de escribir bucles manuales en código para mover punto por punto, Geometry Nodes me permite aplicar una fuerza (por ejemplo, un campo vector de ruido 3D) a miles de vértices simultáneamente mediante nodos matemáticos (como `Set Position`). Es mucho más intuitivo para el diseño visual, y me permite sumar propiedades que en p5.js eran muy costosas de programar: volumen real, profundidad de campo fotográfica (DoF), y materiales emisivos (luz).

**4. Qué tipo de pieza visual imagino construir**
Me imagino una secuencia de apertura (*Title Sequence*) o un *Visualizer* cinemático para la industria musical. La palabra clave (por ejemplo, un concepto como "PULSO", "ECOS" o "TENSIÓN") estará en el centro, materializada en 3D. A medida que avanza una pista de audio, la geometría de la palabra comenzará a desintegrarse suavemente, convirtiéndose en miles de partículas bioluminiscentes o metálicas que flotan y se arremolinan en el espacio tridimensional guiadas por campos de flujo (ruido). Será una metáfora visual sobre la disolución de la forma por el impacto del sonido.

**5. Dificultades técnicas anticipadas**
Anticipo dos retos técnicos principales en esta transferencia:
1.  **La audio-reactividad en Blender:** A diferencia de `p5.sound` donde podía leer la Amplitud o el FFT directamente en una variable en tiempo real, en Blender tendré que aprender a extraer (*bakear*) las frecuencias del audio hacia curvas de animación (*F-Curves*) y conectarlas matemáticamente a los parámetros de mis nodos (drivers) para que el ruido de las partículas reaccione al ritmo de la música.
2.  **Optimización del rendimiento:** Aunque mi hardware actual (procesador de 11ª generación y GPU RTX 3050 Ti con 32GB de RAM) tiene mucha potencia para renderizado 3D, instanciar demasiadas esferas o geometría de alta resolución en cada uno de los miles de puntos de la tipografía podría saturar el *viewport* de Blender. Tendré que ser cuidadoso gestionando la densidad de los vértices para mantener el flujo de trabajo en tiempo real.

### Actividad 3

**1. Componentes o módulos necesarios a aprender**
Para transferir la lógica de "Tipografía animada por partículas y fuerzas", necesito dominar la equivalencia entre el código de p5.js y los nodos de Blender:
* **Creación de texto y arreglos:** En p5.js usaba `text()` y arreglos (`arrays`) para guardar posiciones. En Blender necesito aprender la cadena de nodos: **`String to Curves`** (crea el texto) $\rightarrow$ **`Curve to Mesh`** (le da geometría) $\rightarrow$ **`Distribute Points on Faces`** (convierte la malla en un "arreglo" masivo de puntos/partículas).
* **Fuerzas y vectores (Flow Fields):** En p5.js usaba `noise()` y matemática vectorial (`p5.Vector`) en un bucle `for` para empujar las partículas. En Blender esto se reemplaza por el nodo **`Noise Texture`** combinado con **`Vector Math`** y conectado a un nodo **`Set Position`**, lo que aplica la matemática simultáneamente a miles de vértices.
* **Lectura de Audio:** En p5.js usaba `p5.Amplitude` y `p5.FFT`. En Blender no hay nodos de audio en tiempo real nativos en Geometry Nodes, así que debo aprender a usar la herramienta **`Bake Sound to F-Curves`** en el *Graph Editor* para convertir las frecuencias del MP3 en valores de animación, y luego usar **Drivers** (expresiones matemáticas, ej. `#frame`) para inyectar ese valor dentro del árbol de nodos.

**2. Pruebas técnicas realizadas y qué resuelven**

* **Prueba A: Materialización (Texto a Nube de Puntos)**
    * *Ejecución:* Conecté el texto a un nodo de distribución de puntos y luego utilicé un nodo `Instance on Points` para colocar una pequeña esfera emisiva en cada punto.
    * *Qué resuelve:* Resuelve la transición de un string plano a un elemento 3D de alta densidad que puedo manipular. Al subir la densidad de puntos drásticamente para formar bien las letras, el *viewport* se mantuvo completamente fluido; los 32GB de RAM y la VRAM de la RTX 3050 Ti manejaron la masiva cantidad de instancias sin generar *lag*, confirmando que la base técnica del hardware soporta la visión artística.
    

* **Prueba B: Desplazamiento Vectorial (El "Viento" o "Ruido")**
    * *Ejecución:* Inserté un nodo `Set Position` después de los puntos. En su entrada de *Offset* (desplazamiento), conecté un nodo `Noise Texture`, y utilicé un nodo `Math (Multiply)` impulsado por el tiempo de la escena (`#frame / 24`) para animar la evolución del ruido.
    * *Qué resuelve:* Resuelve el comportamiento físico del sistema. Comprueba que puedo simular turbulencia o campos de flujo (*Flow Fields*) deformando la estructura original de las letras de forma orgánica, exactamente como lo hacíamos al calcular vectores de fuerza en p5.js.

**3. Qué parte del sistema ya logré reconstruir**
Logré reconstruir con éxito el **estado visual y físico base del sistema**. Ya tengo la tipografía parametrizada (puedo cambiar la palabra en un segundo y todo el sistema se adapta) y logré que se fragmente en miles de partículas flotantes que reaccionan a un campo de turbulencia continuo. Visualmente, el concepto de disolución y partículas está resuelto.

**4. Qué parte sigue sin resolverse**
Me faltan resolver dos lógicas críticas para completar la "Tipografía Semántica Audio-reactiva":
1.  **La tensión semántica (El elástico):** Actualmente, el ruido dispersa las partículas infinitamente. Necesito descubrir cómo mezclar (*Mix Node*) la posición deformada por el ruido con la posición original de las letras para crear ese comportamiento de "PULSO" donde las partículas intentan huir pero regresan violentamente a formar la palabra.
2.  **La conexión con el audio:** Aún no he logrado vincular correctamente las curvas de animación (*F-Curves* extraídas del audio) al nodo que controla la escala o la fuerza del `Noise Texture` para que la destrucción tipográfica ocurra exactamente con los bajos de la canción.

### Actividad 4

**1. Tabla Comparativa: Sistema de Partículas y Fuerzas (Ruido / Flow Fields)**

| Dimensión a evaluar | p5.js + Matter.js (Código Imperativo) | Blender Geometry Nodes (Nodal Declarativo) |
| :--- | :--- | :--- |
| **Cómo funcionaba el sistema** | Usábamos arreglos (`arrays`) para guardar objetos. La actualización dependía de un bucle `for` dentro de la función `draw()`, calculando vectores de posición y fuerza iterativamente frame a frame. | Todo el sistema se evalúa simultáneamente en una sola ejecución del árbol de nodos. No hay arreglos visibles; el flujo de datos (Geometría) viaja a través de modificadores espaciales. |
| **Cómo se implementa ahora** | Lógica de texto. `let v = p5.Vector.random2D()`, seguido de `applyForce()`. Modificadores de audio como `p5.FFT` operando en tiempo real. | Lógica de conexión de datos. Nodos matemáticos (`Vector Math`) se conectan a `Set Position`. El audio se "hornea" (*Bake to F-Curve*) como un valor numérico para controlar la escala del ruido. |
| **Qué se mantiene** | **El núcleo matemático universal.** El uso del Ruido Perlin, la suma y multiplicación de vectores espaciales, y la lógica de interpolación (*lerp* o *map*) para restringir valores, siguen siendo exactamente los mismos. |
| **Qué cambia** | **El paradigma estructural y espacial.** Pasamos de escribir instrucciones secuenciales en un lienzo 2D (x, y) a diseñar un flujo de datos continuo en un espacio 3D (x, y, z). Además, el manejo de condicionales es distinto: en p5.js controlábamos la interacción mediante sentencias directas (por ejemplo, evaluar si el clic izquierdo del mouse se mantiene presionado para que el usuario actúe como un pozo gravitacional que arrastra las partículas), mientras que en nodos esto requiere construir complejas mallas de lógica booleana y matemáticas de proximidad para afectar un atributo. |
| **Nuevas ventajas** | **Rendimiento masivo y fidelidad visual.** Al ejecutarse nativamente en C++ y aprovechar la GPU, el software puede instanciar millones de partículas sin *lag*. Se integran automáticamente propiedades ópticas del mundo real: profundidad de campo fotográfica, texturas PBR y emisividad (luz), dándole a la pieza un acabado de industria. |
| **Nuevas limitaciones** | **Pérdida de la interpretación en vivo ("Live Performance").** Mientras que p5.js interactuaba con el micrófono y el mouse en milisegundos, Blender (por defecto) no está diseñado para audio-reactividad *live*. Extraer las frecuencias a curvas requiere que el audio esté pregrabado, restándole la naturaleza improvisada e instrumental que tenía el sketch original. |

---

**2. ¿Qué aprendiste sobre el sistema al tener que reconstruirlo fuera de p5.js?**

Al transferir este sistema fuera de p5.js, aprendí que **el código y los lenguajes visuales son solo interfaces para la misma verdad matemática**. 

Un campo de ruido (*Flow Field*) que empuja una partícula funciona bajo el mismo principio algorítmico, ya sea que yo escriba la función de Perlin noise en JavaScript multiplicada por un vector, o que conecte un nodo de `Noise Texture` a un nodo `Math: Multiply` en Blender. Aprendí a separar el "concepto" (la fuerza, la turbulencia, la tensión elástica) de su "sintaxis". 

Esta transferencia me enseñó que mi verdadero valor profesional no radica en saber de memoria las funciones de un programa específico, sino en dominar las lógicas físicas y matemáticas universales, porque esas me permiten reconstruir cualquier comportamiento orgánico en el motor gráfico, entorno o herramienta que la industria demande.

## Bitácora de aplicación 

1. Herramienta elegida.

- Blender 4.5 (Geometry Nodes + Shader Editor).
- Elegí esta herramienta porque representa un estándar en la industria de la animación y es la heramienta con la que mas estoy familiarizado. Dentro de mi formación en Ingeniería de Entretenimiento Digital, me interesa profundamente la intersección entre la dirección de arte y la programación técnica (Technical Art).

2. Sistema transferido.

- Sistemas de Partículas con Estado Continuo (Fuerzas y Flow Fields) / Tipografía semántica animada.
- En lugar de hacer una simple deformación espacial, transferí un sistema donde las partículas tienen "memoria" de su movimiento. Al igual que en p5.js usábamos arreglos para guardar la velocidad y aceleración de agentes autónomos en cada frame, aquí utilicé la Simulation Zone para que las partículas reaccionen de manera fluida y continua a campos vectoriales, acumulando datos de velocidad con el tiempo.

3. Contexto profesional concreto.

La pieza está diseñada como una Secuencia de Apertura o un Visualizer de alta gama para un producto audiovisual cinematográfico o musical, similar a los ejemplos de palabras que se nos mostro de ejemplo en su momento. En la industria actual, la tipografía semántica ya no se sobrepone simplemente sobre el video; se busca que sea mas impactante y se combine con el entorno. Esta topografía granular responde a esa necesidad, ofreciendo un entorno inmersivo donde el texto es revelado por la naturaleza misma del material.

4. Concepto visual.

"OCEAN"
El concepto visual se aleja de modelar letras en 3D sólidas que interrumpan el flujo del océano. En su lugar, el océano está construido por un tejido denso de micro-geometría (cubos). La palabra "OCEAN" se manifiesta como una proyección o una mancha en el agua, revelándose a través de los colores que se convierte despues en espuma generada por la fricción de la corriente.

5. Referencias.

TeamLab y Ouchhh: Referentes de instalaciones de arte generativo masivo, donde el agua y los fluidos no se renderizan como un líquido tradicional, sino como millones de partículas.
https://www.teamlab.art/es/e/kyoto/

Projection Mapping Escénico: La técnica de usar la luz y el color para proyectar información gráfica sobre superficies topográficas altamente irregulares y en movimiento, integrando el mensaje visual con la textura física.
https://www.heavym.net/es/top10-3d-projection-mapping/

6. Bocetos.



7. Explicación de la transferencia.

El paso de p5.js a Blender 4.5 requirió una traducción de lógicas imperativas a declarativas (flujo de datos nodal):

El Bucle draw(): En p5.js, la actualización continua de la posición se hacía en el draw(). En Blender, esto lo reconstruí utilizando la Simulation Zone de Geometry Nodes, que itera y guarda el estado de la geometría en cada cuadro.

Vectores y Orientación (p5.Vector): En código calculábamos el vector de dirección (velocidad) para rotar un agente. Aquí extraje el vector de movimiento y utilicé el nodo Align Rotation to Vector para que cada cubo instanciado mire físicamente hacia donde fluye la corriente.

Variables Globales y map(): En p5.js guardaba la magnitud de la velocidad en una variable y usaba map() para pintar la partícula (ej. más rápido = más blanco). En Blender, usé Store Named Attribute para guardar el vector velocidad con el nombre v. Luego, en el Shader Editor, extraje ese atributo, calculé su magnitud con el nodo Vector Math: Length, y usé un Color Ramp (el equivalente exacto a map()) para colorear el océano desde azul oscuro hasta la espuma clara.

8. Mapa de decisiones.

- La Tipografía como Textura (Shader) y no como Malla (Geometría): * La decisión: En lugar de usar el nodo String to Curves para generar texto 3D físico, decidí importar la imagen "Palabra oceano.jpg" directamente en el Shader Editor y proyectarla sobre el material.
- Si ponía letras sólidas en medio de la cuadrícula, iba a interrumpir la topología del océano; el fluido chocaría contra ellas rompiendo la inmersión. Al hacerlo mediante texturas, logro que la palabra "OCEAN" se comporte como una sombra bajo el agua, un reflejo o una acumulación de espuma. Esto resuelve el problema de integración: la tipografía no compite con el océano, sino que es el océano revelando el mensaje.

- Decidí no conformarme con mover puntos usando un ruido estático, sino encapsular el movimiento dentro de una Simulation Zone en Geometry Nodes.
- En p5.js, la magia de los flow fields o los agentes autónomos radica en el bucle draw(), donde cada partícula tiene inercia y recuerda su velocidad del frame anterior. Si solo usaba un nodo Noise normal en Blender, perdía esa "memoria" física. La Simulation Zone me permitió recrear exactamente el estado continuo de p5.js, dándole al agua una inercia real y un flujo orgánico y pesado.

- En lugar de intentar simular agua fotorrealista (lo cual es costoso y cliché) o usar esferas básicas, instancié miles de pequeños cubos y utilicé el nodo Align Rotation to Vector conectado a mi atributo de velocidad.
- A nivel técnico, esto optimiza el rendimiento en mi equipo (RTX 3050 Ti), permitiéndome manejar una densidad altísima sin colapsar la VRAM. A nivel estético, al rotar los cubos en la dirección de la corriente, la superficie del mar adquiere una textura de "escamas" o un tejido granular denso. Esto le da a la pieza el aire de instalación de arte generativo de alta gama (estilo TeamLab), separándola de una simple simulación física genérica.

- Decidí guardar el vector de movimiento bajo el atributo v en Geometry Nodes y llamarlo en el Shader Editor para calcular su magnitud (Length) y conectarlo a un Color Ramp.
- En código, era muy fácil decir "si la velocidad es alta, pinta la partícula de blanco; si es baja, de azul oscuro". En un entorno de nodos, la geometría y los materiales viven en mundos separados. Tomé esta decisión para crear un puente de datos entre ambos. El resultado es que el color del océano no es un material estático puesto encima al azar; la pintura del mar es una respuesta física directa a la turbulencia de la simulación. El caos genera la espuma.

- Implementé un nodo Relax Points justo antes de instanciar la geometría.
- Al usar fuerzas continuas, los puntos tienden a amontonarse en los "pozos" gravitacionales del ruido, dejando parches vacíos y zonas superpuestas (un problema clásico del flocking y flow fields). Este nodo actúa como una fuerza de separación (Separation en algoritmos de Boids), obligando a las partículas a mantener una distancia mínima entre ellas, garantizando una superficie legible y limpia para poder proyectar la tipografía sin artefactos visuales.

9. Mapa de presentación.

Dado que mi herramienta es Blender 4.5, diseñé mi presentación en dos actos. El objetivo es que el jurado y el público primero experimenten la obra como una pieza terminada, y luego comprendan la ingeniería generativa que hay detrás.

Acto 1: El Render a Pantalla Completa

- La presentación comenzará con las luces bajas y la proyección a pantalla completa del archivo de video final (Render0001-0132.mp4), sincronizado con el audio. No habrá interfaces, cursores, ni explicaciones previas.
- Blender brilla en su motor de render y en el cálculo físico avanzado. Mostrar el video final permite que el concepto visual de "OCEAN" (la atmósfera melancólica, la refracción de los cubos y el peso del fluido) se transmita de forma inmersiva. Si mostrara el software de entrada, la atención se desviaría hacia los menús y se perdería el impacto emocional de la tipografía revelándose entre las sombras del mar.

Acto 2: Demostración de la arquitectura

- Una vez finalice el video, encenderé las luces y abriré la escena original directamente en Blender. En lugar de dar un paseo aleatorio por la interfaz, haré un recorrido guiado y quirúrgico por los dos "cerebros" del sistema:
- Abriré la ventana de Geometry Nodes. Mostraré específicamente la Simulation Zone, explicando visualmente que este bloque de nodos morados es la traducción exacta del bucle draw() que usábamos en p5.js para mantener la memoria y la inercia de las partículas.
- Abriré el Shader Editor. Mostraré el nodo Attribute llamando a la variable v (velocidad).
- Esta fase es crucial para evidenciar la transferencia de aprendizaje. No me limitaré a mostrar los nodos estáticos; demostraré que entiendo el sistema haciendo una alteración en vivo. Desconectaré temporalmente el nodo Image Texture (el que contiene el archivo "Palabra oceano.jpg") en el Shader Editor. Al hacerlo, el público verá cómo la palabra desaparece y solo queda el océano azul. Esto probará empíricamente mi decisión de diseño más importante: la palabra "OCEAN" no es geometría modelada en 3D, es una pintura generada matemáticamente por la topografía del fluido.

10. Evidencia del uso de IA.

La dirección de arte, la decisión estética de convertir el texto en una textura proyectada y la lógica subyacente de transferencia de vectores y estados de simulación son de mi estricta autoría. Utilicé la IA (Gemini) como un tutor de sintaxis para comprender la estructura de la API de nodos de Blender. Me apoyé en la herramienta para entender cómo un concepto abstracto como p5.Vector.mag() se traducía en el uso combinado de Store Named Attribute en Geometry Nodes y el nodo Length dentro del ecosistema de Shader Nodes.

### 11. Código, archivo, proyecto o documentación técnica según la herramienta.

<img width="1589" height="621" alt="Captura de pantalla 2026-05-12 140513" src="https://github.com/user-attachments/assets/4f47789b-92a2-4e9d-b141-273430e5b846" />

<img width="1587" height="616" alt="Captura de pantalla 2026-05-12 140528" src="https://github.com/user-attachments/assets/0ed91601-98f0-4aac-91ef-7f36e6f6d64e" />

[Archivos.zip](https://github.com/user-attachments/files/27680283/Archivos.zip)

### 12. Registro visual de la pieza.


https://github.com/user-attachments/assets/eae2809a-60a7-42bf-915b-c66226a5bffe


## Bitácora de reflexión
