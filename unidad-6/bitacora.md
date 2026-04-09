# Unidad 6

## Bitácora de proceso de aprendizaje

### Actividad 1

- Obra 1: Serie Fidenza (El pináculo del Flow Field estructurado)

1. Decisiones Visuales:
* Composición: Ocupa todo el lienzo pero respeta márgenes estrictos, dando la sensación de una pintura enmarcada.
* Densidad: Altamente variable. Contrapone áreas densamente pobladas por líneas muy finas con áreas de respiro donde dominan bloques gruesos y masivos.
* Dirección del movimiento: Curvas suaves, orgánicas y predecibles a nivel local, pero que a nivel global forman macro-estructuras, remolinos y corrientes complejas. 
* Color: Paletas de color estrictamente curadas (generalmente cálidos terrosos con acentos de colores primarios vibrantes). El fondo suele ser un tono papel o pergamino, no negro puro.
* Ritmo, repetición y variación: El ritmo está marcado por la longitud y el grosor de las curvas. La repetición del patrón direccional da cohesión, mientras que la variación extrema en el grosor rompe la monotonía algorítmica.

- 2. ¿Por qué son potentes estas decisiones?
Fidenza es poderosa porque destruye la perfección digital. A primera vista, parece arte abstracto expresionista hecho por un humano con cinta de enmascarar y acrílicos. La decisión de no dejar que las líneas se crucen crea una tensión visual increíble; parece que las formas compiten por el espacio en el lienzo, dándole un peso físico a algo puramente virtual.

- 3. Hipótesis del Sistema (Las Reglas):**
* El motor: Un Flow Field estático basado en Ruido de Perlin a baja frecuencia para garantizar curvas amplias y suaves.
* Los agentes: Se lanzan miles de agentes al campo, pero con una regla de detección de colisiones (Collision Avoidance) espacial: el agente avanza dibujando, pero si detecta que va a chocar con una línea ya dibujada, muere (detiene su trazo).
* Variabilidad: Antes de nacer, cada agente tira unos dados invisibles (`random()`) que deciden su grosor, su color (desde un arreglo predefinido) y su longitud máxima.

### Actividad 2

1. ¿Qué es un agente autónomo?
Un agente autónomo es una entidad computacional que posee un estado interno, capacidad para percibir su entorno y la habilidad de tomar decisiones para cumplir un objetivo. A diferencia de una partícula básica que solo obedece ciegamente a la física, un agente tiene "intención". Sabe dónde está, evalúa qué hay a su alrededor (como un objetivo, un obstáculo u otros agentes) y calcula hacia dónde quiere ir, limitándose por sus capacidades físicas máximas (su velocidad máxima y su fuerza máxima de giro).

2. ¿Qué es una steering force (fuerza de dirección)?
Es el cálculo matemático de la "intención" del agente. En lugar de simplemente teletransportarse o apuntar directamente al objetivo, el agente calcula la diferencia entre lo que está haciendo ahora y lo que desearía hacer. 
La fórmula maestra que define Reynolds es increíblemente simple pero poderosa: 
Steering Force = Velocidad Deseada - Velocidad Actual
Es, en esencia, una fuerza de corrección. Es el empujón que el agente debe darse a sí mismo para corregir su trayectoria actual y alinearla con su meta.



3. Comparación: Steering force vs. Gravedad o Viento
La diferencia radica en el origen y la pasividad.
* Fuerzas externas (Gravedad, Viento): Viven en el entorno. Son pasivas, ciegas e inevitables. Se aplican *sobre* el objeto, empujándolo sin importar lo que el objeto quiera. Una roca cae por la gravedad sin tomar ninguna decisión.
* Steering forces (Fuerza de dirección): Viven *dentro* del agente. Son activas y calculadas. Es una fuerza que el agente se auto-aplica basándose en su percepción del mundo. Es el pájaro batiendo las alas de forma específica para luchar contra el viento y la gravedad con el fin de llegar a la rama de un árbol.

4. ¿Por qué estas ideas son útiles para diseñar comportamiento visual?
Porque nos permiten diseñar sistemas emergentes y coreografías orgánicas. En lugar de usar herramientas tradicionales donde el diseñador dicta exactamente el camino que debe seguir un píxel (animación por *keyframes*), o usar `random()` que genera caos sin sentido, las steering forces nos permiten diseñar personalidades. 
Si programas a mil agentes con la "personalidad" de huir del cursor, buscar el centro de la pantalla y no chocar entre ellos, el resultado visual no será un código frío; será una danza fluida, viva y natural que el diseñador no tuvo que animar a mano. Es esculpir comportamiento en lugar de esculpir formas.

### Actividad 3

 1. Arquitectura del Sistema
* **¿Cómo está construido el campo de flujo?**
    Se construye como una cuadrícula (*grid*) que divide todo el lienzo. A nivel de código, es un Arreglo 2D (o un arreglo 1D indexado matemáticamente) donde cada "celda" de la cuadrícula almacena un solo valor: un vector apuntando en una dirección específica. Esa dirección suele calcularse usando el Ruido de Perlin (nuestro viejo amigo `noise()`) para que los ángulos cambien suavemente de una celda a la siguiente.
* **¿Qué representa cada celda o vector?**
    Representa la **"Velocidad Deseada"** ideal en esa coordenada exacta. Es el equivalente algorítmico a la corriente de un río o la dirección del viento en ese punto específico del espacio.
* **¿Cómo usa un agente su posición para consultar el campo?**
    El agente hace un cálculo de mapeo (*lookup*). Divide sus coordenadas actuales `(x, y)` entre la **resolución** de la cuadrícula para saber exactamente en qué fila y columna está parado. Luego, pide prestado el vector almacenado en ese índice del arreglo.
* **¿Cómo se convierte el vector en movimiento?**
    Aplicando la fórmula de Craig Reynolds:
    1.  El agente toma el vector de la celda y lo asume como su `Velocidad Deseada`.
    2.  Calcula la *Steering Force*: `Fuerza = Velocidad Deseada - Velocidad Actual`.
    3.  Aplica esa fuerza a su aceleración, provocando que su trayectoria se curve para intentar alinearse con el flujo del campo.

 2. Parámetros Críticos
* **Resolución:** Define qué tan detallado es el campo. Si es muy grande (baja resolución), el campo se ve como un tablero de ajedrez gigante y los agentes hacen giros bruscos al cruzar de una celda a otra. Si es muy pequeña (alta resolución), el movimiento es líquido y perfecto.
* **MaxSpeed:** Limita el tamaño del vector de velocidad. Define qué tan rápido puede navegar el agente por la corriente.
* **MaxForce:** ¡Este es el parámetro de la personalidad! Define qué tan obediente es el agente.
    * *MaxForce muy alto:* El agente gira de inmediato, acatando el campo de flujo como un esclavo. Parece limadura de hierro en un imán.
    * *MaxForce muy bajo:* El agente es pesado o "terco". Le cuesta girar, por lo que toma curvas muy abiertas y a veces ignora los cambios bruscos del viento. Se siente como un barco enorme intentando dar la vuelta.
* **Cantidad de agentes:** Define la densidad visual. Pocos agentes permiten ver trayectorias individuales. Miles de agentes (con opacidad baja) dibujan el volumen y las texturas del campo completo.

 3. Análisis Perceptual y Musical
* **¿Qué tipo de movimiento produce?**
    Produce un movimiento orgánico, laminar y determinista. Todo fluye. Es idéntico a cómo se mueve el humo al salir de un incienso, cómo fluye el agua alrededor de las rocas en un río, o cómo crece el cabello.
* **¿Qué sensaciones visuales te sugiere?**
    Sugiere inevitabilidad, calma y fluidez. Al no haber colisiones ni movimientos erráticos (el ruido de Perlin garantiza transiciones suaves), transmite una sensación hipnótica. Sin embargo, si se aumenta drásticamente la escala del ruido, puede sugerir turbulencia, tormenta o caos.
* **¿En qué tipo de pieza musical funcionaría bien?**
    Este algoritmo está hecho a la medida para música con texturas continuas, síntesis atmosférica o cuerdas sostenidas (Ambient, Post-Rock, o piezas orquestales neoclásicas). Piezas musicales de artistas como Jon Hopkins, Brian Eno o el mismísimo Hans Zimmer. 
    *¿Por qué?* Porque un *Flow Field* no tiene "golpes" rítmicos fuertes de por sí; es un barrido continuo. Por lo tanto, no suele encajar con techno rígido o ritmos súper marcados, sino con **dinámicas de volumen y tensión progresiva**.

### Actividad 4

 1. Las Tres Reglas Básicas
En lugar de programar una coreografía, a cada agente se le instalan solo tres "instintos" que debe calcular mirando únicamente a sus vecinos cercanos:
* **Separación (Espacio Personal):** "No chocar con nadie". El agente revisa quién está demasiado cerca y calcula una *steering force* para alejarse en la dirección contraria. Evita el amontonamiento.
* **Alineación (Ir con la corriente):** "Hacer lo que hacen los demás". El agente calcula el promedio de la dirección y velocidad de sus vecinos e intenta imitarla. Es lo que permite que el grupo viaje en la misma dirección sin un líder.
* **Cohesión (Miedo a la soledad):** "Mantenerse unido al grupo". El agente calcula la posición promedio (el centro de masa) de sus vecinos e intenta dirigirse hacia ese punto central.

 2. Parámetros de Control
El sistema se controla mediante dos frentes:
1.  **El Radio de Percepción (Vision):** ¿Qué tan lejos puede "ver" un agente? Si el radio es muy pequeño, el agente se siente solo y el enjambre se rompe en mini-grupos. Si es muy grande, todo el lienzo intenta actuar como un solo organismo masivo.
2.  **Los Pesos (Multiplicadores):** Una vez calculadas las tres fuerzas, cada una se multiplica por un peso antes de sumarse. Esto define la "prioridad" del agente. Si el peso de separación es 5 y el de cohesión es 1, el agente priorizará su espacio personal por encima de mantenerse en el grupo.

 3. Modificación de Pesos y Comportamiento Emergente
Al jugar con los parámetros del simulador, podemos catalogar los siguientes estados emergentes:
* **Alto peso de Cohesión + Baja Separación:** El enjambre se vuelve **compacto** y algo inestable. Los agentes colapsan en bolas densas, pareciendo un enjambre de abejas atacando un solo punto.
* **Alta Separación + Baja Cohesión/Alineación:** El sistema se vuelve **disperso, nervioso y caótico**. Los agentes huyen despavoridos unos de otros. Parece gas expandiéndose o insectos reaccionando a un repelente.
* **Alineación predominante (Separación y Cohesión equilibradas):** Se genera un movimiento **fluido y estable**. Es la simulación perfecta de un banco de peces cruzando el océano o una bandada de pájaros migratorios. Fluyen esquivándose con elegancia.

 4. Atmósfera Visual y Relación Musical
* **Atmósfera Visual:** El *flocking* produce una estética puramente biológica y orgánica. Transmite la sensación de estar observando la naturaleza bajo un microscopio (microorganismos) o desde un satélite (migraciones). Es un sistema vivo que respira; nunca se repite exactamente igual.
* **Relación con una canción:** Si el *Flow Field* era para música *ambient* sostenida, el *Flocking* es perfecto para música **evolutiva, polirrítmica y percusiva** (ej. IDM, Max Cooper, Jon Hopkins, o música electrónica experimental con texturas granulares). 
    * *¿Por qué?* Porque puedes mapear la energía de la música a los pesos del sistema. En las partes calmas de la canción, subes la alineación y la cohesión para un flujo pacífico. En los *drops* o picos de tensión rítmica (percusión fuerte), puedes disparar el multiplicador de **Separación**, haciendo que el enjambre "estalle" en un caos nervioso con cada golpe de bombo, para luego volver a unirse. Es un instrumento visual muy reactivo a los cambios de densidad musical.

### Actividad 5

 1. Tabla Comparativa: Flow Fields vs. Flocking

| Aspecto | Flow Fields (Campos de Flujo) | Flocking (Comportamiento de Bandadas) |
| :--- | :--- | :--- |
| **Tipo de movimiento** | Laminar, fluido, guiado y continuo. Sigue corrientes invisibles preestablecidas. | Orgánico, complejo, colectivo y reactivo. Se adapta instante a instante. |
| **Nivel de control visual** | **Alto.** El diseñador esculpe el espacio (con la escala del ruido). Sabes por dónde van a pasar las líneas y qué forma general tendrán. | **Indirecto / Medio.** Controlas las reglas y la personalidad (pesos), pero no controlas el resultado espacial exacto. La coreografía se hace sola. |
| **Nivel de emergencia** | **Bajo - Medio.** El atractivo visual es alto, pero la complejidad proviene de la textura del ruido, no de la interacción entre los agentes. | **Muy Alto.** La magia visual nace exclusivamente de la interacción. Reglas microscópicas generan macroestructuras impredecibles. |
| **Tipo de atmósfera o sensación** | Hipnótica, inevitable, estructurada, etérea, de inevitabilidad cósmica o calma oceánica. | Viva, nerviosa, biológica, animal. Transmite tensión, agrupación, pánico colectivo o fluidez cooperativa. |
| **Relación musical ideal** | Música *Ambient*, texturas sostenidas (drones), post-rock, progresiones lentas y dinámicas de volumen continuo. | Música IDM, *glitch*, electrónica experimental, polirritmias, percusiones marcadas o música orquestal con *staccatos*. |
| **Ventajas** | Altamente estético, computacionalmente económico ($O(N)$), perfecto para dibujar o dejar rastros texturizados (estilo Tyler Hobbs). | Realismo biológico inigualable. Altamente expresivo y dinámico al reaccionar al entorno en tiempo real. |
| **Limitaciones** | Puede sentirse mecánico o monótono si el campo no se actualiza o no hay variación en los agentes. | Computacionalmente muy costoso ($O(N^2)$ si no se optimiza). Es difícil obligar al sistema a formar figuras específicas. |

 2. Decisiones de Diseño según la Emoción Musical (El Cierre)

Si estuviera dirigiendo las visuales de un show en vivo, esta sería mi selección algorítmica para cada estado de ánimo:

* **Contemplativa ➔ Flow Field.**
    * *Por qué:* La contemplación requiere que la mente del espectador se relaje y siga un camino predecible pero hermoso. Un campo de flujo con ruido de Perlin a muy baja escala, donde los agentes dibujan líneas translúcidas lentamente, imita el humo de un incienso o las corrientes del fondo del mar, invitando a la reflexión sin alterar los sentidos.
* **Agresiva ➔ Flocking.**
    * *Por qué:* La agresión requiere cortes, choques y tensión. Usaría el *Flocking* mapeando los golpes de bajo o bombo al parámetro de **Separación**. En los momentos de mayor agresividad sonora, el enjambre "estallaría" en caos, con los agentes huyendo frenéticamente unos de otros, transmitiendo pánico y energía violenta.
* **Melancólica ➔ Flow Field.**
    * *Por qué:* La melancolía suele asociarse con la gravedad, el desvanecimiento y lo inevitable. Usaría un campo de flujo dirigido predominantemente hacia abajo, como lágrimas, hojas cayendo o lluvia lenta. La incapacidad de los agentes de cambiar su destino (al estar atados a la corriente del campo) refuerza esa sensación de tristeza poética.
* **Eufórica ➔ Flocking.**
    * *Por qué:* La euforia es energía colectiva, como un festival o una bandada de aves en primavera. Usaría el *Flocking* con una alta **Alineación** y velocidad, y lo acoplaría a un sistema de emisión en ráfagas. Cuando la canción llegue a su clímax (*drop*), el enjambre se movería al unísono a toda velocidad formando figuras masivas y arremolinadas, celebrando la cohesión y la vida.

## Bitácora de aplicación 


## Bitácora de reflexión
