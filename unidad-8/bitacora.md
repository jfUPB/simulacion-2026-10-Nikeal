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

## Bitácora de aplicación 


## Bitácora de reflexión
