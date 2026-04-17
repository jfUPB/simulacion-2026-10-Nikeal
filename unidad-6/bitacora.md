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

1. Concepto visual.

Para este proyecto, quise interpretar visualmente la canción "Vivarium" de Ado, centrándome en esa sensación de estar atrapado por el pasado y el proceso de aprender a aceptarlo. La obra comienza mostrándonos a una criatura que se arrastra con dificultad, similar a un ciempiés. Esta forma inicial representa el peso de nuestros recuerdos y de las versiones anteriores de nosotros mismos que a veces sentimos como un lastre. A medida que la intensidad de la canción sube, vemos cómo las partes de esta criatura intentan separarse violentamente, como si quisieran huir o desgarrarse por la presión, pero una tensión invisible las obliga a mantenerse unidas, mostrándonos que no podemos simplemente borrar o destruir lo que fuimos.

El momento clave de la pieza ocurre en el clímax de la cancion. En lugar de colapsar bajo esa tensión, la criatura sufre una metamorfosis y se puede transforma en una medusa. Ya no se arrastra pesadamente por el fondo, sino que flota y fluye con las corrientes. Lo más importante de esta transformación es que el organismo no desechó las partes que lo anclaban, sino que las reconfiguró para crear su nueva forma. Con esto, busco transmitir que aceptar nuestro pasado y nuestra memoria no tiene por qué ser una carga; de hecho, es precisamente esa estructura la que nos da la libertad y la belleza para seguir adelante.

2. Relación entre la visual y la canción.

Para mí, era fundamental que la canción de Ado no fuera solo un adorno o un sonido de fondo, sino el verdadero motor físico de la obra. Cada elemento de la canción dicta una regla en este entorno. Por ejemplo, el ritmo y los agudos (como la percusión rápida) controlan la "adrenalina" del organismo: en los momentos tranquilos y melancólicos, la criatura y las corrientes del mar levitan casi congeladas en el tiempo, pero cuando el ritmo se acelera, todo el ecosistema se acelera mas.

Por otro lado, la intensidad del volumen dicta la tensión del cuerpo de la criatura. En los silencios o susurros, el organismo es un núcleo denso, apretado y contenido. Sin embargo, cuando la voz de Ado estalla o hay un clímax musical, esa energía empuja a las células hacia afuera. Visualmente, el cuerpo se expande de forma violenta, estirando los hilos hasta su límite, creando una sensación de desgarro que nunca llega a romperse. Finalmente, la estructura de la pieza es performática: la metamorfosis de ciempiés a "medusa" la ejecuto en vivo, asegurándome de que esta "liberación" visual ocurra exactamente en un punto de quiebre emocional de la canción y viceversa.

3. Moodboard o referencias.

- https://reas.com/process
<img width="1306" height="690" alt="image" src="https://github.com/user-attachments/assets/732f8425-b1ee-43c3-87e4-51787a03e858" />

- TeamLab — "Moving Creates Vortices and Vortices Create Movement"
https://www.teamlab.art/w/vortices/

- Theo Jansen — "Strandbeests"
https://www.strandbeest.com/

- Ryoji Ikeda — "supercodex"
https://www.youtube.com/watch?v=QZMdymqChj4

4. Dos o más bocetos.

<img width="711" height="551" alt="image" src="https://github.com/user-attachments/assets/60131160-5846-4f51-8718-130a8eb8532b" />

<img width="874" height="601" alt="image" src="https://github.com/user-attachments/assets/a127af6f-2687-4102-84ed-d4ed4ec80f48" />

5. Mapa de decisiones.

- El Entorno y la Atmósfera
* Decisión: Fondo de tonos oscuros (azul oscuro/negro) que no se borran por completo, dejando un rastro translúcido (efecto motion blur).
    * Porque?: Crea la sensación de densidad y profundidad extrema. La oscuridad representa lo desconocido y el encierro del "Hakoniwa" (el jardín de cajas de la canción), mientras que el rastro emula la viscosidad del agua profunda.
* Decisión: Un mar de partículas gobernadas por un Campo de Flujo (Flowfield de ruido Perlin).
    * Porque?: Quería que el entorno no estuviera vacío, sino vivo. Las corrientes representan las fuerzas externas de la vida que no podemos controlar, abrumadoras y constantes, obligando al individuo a reaccionar ante ellas.

- Morfología y Forma del Organismo
* Decisión: Diseño inicial de "Ciempiés" (Extremidades ancladas a lo largo de una espina dorsal mediante Cinemática Inversa).
    * Porque?: Representa la carga del pasado y la resistencia a dejarlo. Su movimiento se siente como un arrastre lánguido y pesado, simbolizando la fricción y la dificultad de avanzar cuando uno carga con traumas o versiones anteriores de sí mismo.
* Decisión: Diseño secundario de "Medusa" de dos capas (Extremidades re-ancladas a la cabeza en un radio geométrico).
    * Porque?: Simboliza la aceptación y la liberación. Elegí no "destruir" las extremidades del ciempiés, sino reconfigurarlas, porque el mensaje central es que aceptar nuestro pasado nos da la estructura para ser libres, permitiendo al organismo flotar y fluir.

- Física y Respuesta al Sonido 
* Decisión: "Tensión Magnética" extremadamente controlada por el Volumen/Amplitud (Los nodos se repelen violentamente pero están atados por ligamentos elásticos).
    * Porque?: Queria transmitir el estrés psicológico. Cuando la voz de Ado estalla o la música sube, quería que la criatura pareciera estar a punto de desgarrarse por la presión emocional. Que los ligamentos resistan y no se rompan demuestra resiliencia; la criatura se expande y sufre, pero sobrevive al final o se reconstruye en algunos casos.
* Decisión: Aceleración de la criatura y del mar atada a los Agudos/Ritmo.
    * Porque?: Dicta el ritmo de la obra. En los silencios, la criatura levita en cámara lenta (melancolía, pausa reflexiva). Cuando entran los *hi-hats* o la percusión rápida, el límite de velocidad se dispara, creando un frenesí visual que iguala la ansiedad y la energía de la música.

- Renderizado y Estética Visual
* Decisión: Uso de un sistema "Plexus" (dibujar hilos finos entre partículas solo si están cerca) en lugar de formas sólidas.
    * Porque?: Da la sensación de un tejido nervioso frágil, translúcido y orgánico. Representa cómo estamos compuestos por recuerdos y células interconectadas. Cuando el volumen sube y el cuerpo se expande, estos hilos se adelgazan visualmente, acentuando el sentimiento de tensión.
* Decisión: Emisión de un rastro de partículas bioluminiscentes que se encogen y desvanecen lentamente.
    * Porque?: Funciona como una metáfora del tiempo, la memoria y el desgaste. El organismo deja partes de su energía (su luz) en el mar a medida que avanza, marcando su existencia en un entorno grande.
* Decisión: Cambio dinámico de paletas de color (4 emociones).
    * Porque? Para acompañar la evolución emocional de la canción, e interactuar un poco con la criatura.

- Interactividad y Performance
* Decisión: La metamorfosis no es automática; se acciona manualmente presionando la barra espaciadora.
    * Porque?: Convierte el código en un instrumento performático. Me permite a mí (como VJ o artista) "tocar" la visual en vivo, asegurándome de que el impacto visual de la liberación ocurra en el momento que quiero, dando presencia y conexión con la obra.

6. Mapa de interpretación.

* Audio (Amplitud / Volumen): Controla la tensión estructural de la pieza. De forma autónoma, el volumen dicta qué tanto se expanden y estiran los "huesos" de la criatura (efecto magnético) y qué cantidad de rastro bioluminiscente deja a su paso.
* Audio (Agudos / Ritmo): Controla la energía cinética general. Regula de forma automática la velocidad límite a la que nada la criatura y la turbulencia de las corrientes marinas, pasando del la calma al frenesí.
* Barra Espaciadora: Cambia la morfologia de la criatura. Se activa manualmente y es para enfatizar ne la metamorfosis del Ciempiés (represión) a la Medusa fractal (liberación).
* Tecla [ G ]: Cambia entre las paletas de colores (Azul, Rojo, Magenta, Verde). Lo utilizo para cambiar de expresión y marcar manualmente los cambios de estación emocional o las nuevas secciones de la canción, asegurando que el color coincida con la intención de la voz.
* Tecla [ F ]: Control de inmersión. Permite aislar la pieza en pantalla completa.

*** ¡Con este texto cubres a la perfección el requisito de tu entrega, demostrando que tú eres el director de la pieza visual en todo momento! ¿Seguimos con alguna otra sección de tu bitácora?

7. Justificación del algoritmo elegido.

* Cinemática Inversa (IK) en lugar de Flocking (comportamiento de enjambre): Elegí Cinemática Inversa para construir el cuerpo de la criatura porque necesitaba una sensación de estructura, tensión y restricción. Si hubiera usado un algoritmo de *flocking*, las partículas se habrían dispersado libremente por el lienzo al subir el volumen de la música. IK, en cambio, obliga a los nodos a actuar como "huesos" conectados a ligamentos. Esto me permitió programar el efecto de "repulsión magnética": el volumen empuja las partículas hacia afuera, pero la Cinemática Inversa las obliga a mantenerse unidas. Esta física es la traducción exacta de mi concepto narrativo: la presión psicológica de querer huir del pasado, pero estar atado a él. Además, IK es lo que permite que el anclaje cambie suavemente y la criatura mute de Ciempiés a Medusa de forma orgánica.

* Flow Fields (Campos de Flujo con Ruido Perlin) en lugar de movimiento aleatorio:
    Elegí un *flow field* para el fondo y las partículas del agua porque necesitaba transmitir la sensación de un entorno acuatico continuo. Un movimiento aleatorio (*random walk*) se habría visto como un poco simple sin sentido. El campo de flujo crea "corrientes" invisibles que arrastran tanto a la criatura como a las partículas de agua de una manera fluida.

* Algoritmo Plexus (Trazado de líneas por umbral de distancia) en lugar de formas sólidas:
    Para dibujar la criatura, elegí un algoritmo que mide la distancia entre todos los nodos y traza líneas solo entre aquellos que están cerca. Elegí esto en lugar de dibujar esferas sólidas o polígonos cerrados porque quería que la criatura se sintiera frágil, translúcida y compleja. Este algoritmo genera una textura de "telaraña" o sistema nervioso que refuerza la idea de que la criatura es un ser que "siente".


8. Explicación de la relación audio-visual.

* Amplitud (Volumen general) = Tensión estructural:
     Tomé el nivel de volumen y lo suavicé mediante interpolación lineal (`lerp`) para crear resistencia elástica. El volumen controla la expansión magnética del esqueleto: a bajo volumen, los "huesos" miden apenas 3 píxeles; en los picos de intensidad, los nodos se repelen violentamente y la distancia se multiplica por 8.
* Frecuencias Agudas (Treble) =
    Mapeé los agudos al límite de velocidad de la criatura (`velMax`) y a la turbulencia del entorno (*z-offset* del ruido Perlin). Cuando entra la percusión rápida o los sonidos estridentes, la criatura deja de levitar lánguidamente a medio píxel por frame y sale disparada hasta 20 píxeles por frame.
* Frecuencias Bajas (Bass) = 
    Los bajos no afectan la anatomía de la criatura, sino la gravedad de las corrientes marinas (*Flowfield*). Cada golpe de bajo fuerte, como un bombo de batería, incrementa la magnitud de los vectores del mar. Esto arrastra las partes de la criatura y el rastro de partículas con más violencia, enfatizando la lucha constante del organismo contra un entorno que lo supera en tamaño y fuerza.
* Frecuencias Medias (LowMid)
    Mapeé estas frecuencias centrales a la opacidad y el brillo de los hilos de luz (el tejido *Plexus*). Esto permite que la "telaraña" geométrica que forma a la criatura titile orgánicamente, asegurando que el cuerpo principal de la melodía se refleje en la textura visual de la pieza.

9. Evidencia del uso de IA.

"El concepto narrativo (la metáfora de la relación de Ado con su pasado), la dirección visual (la metamorfosis de ciempiés a medusa), las lógicas de interacción como performance y la decisión de mapear variables específicas del audio a la tensión estructural y la velocidad, fueron 100% mis ideas y diseño conceptual. Utilicé la IA (Gemini) para traducir mis directrices de diseño a código funcional en `p5.js`. Específicamente, utilicé la herramienta para:

1. Implementar la matemática compleja de la Cinemática Inversa (IK) que me permitiera lograr la tensión elástica que yo buscaba.
2. Optimizar el rendimiento del algoritmo de proximidad (Plexus) para dibujar la estructura fractal sin que el navegador colapsara.
3. Formular la curva matemática exponencial (`pow`) necesaria para que el salto de color de azul a rojo fuera agresivo y no un degradado suave.
4. Depurar errores de sintaxis y referencias en la consola (como un `ReferenceError` con la variable de los bajos del espectro de audio). 

10. Código fuente.

```js
/*
Autor: David Nivia

VIVARIUM - Instrumento Visual Performativo

Año:  2026-10

CONTROLES:
 [ Clic ]  -> Iniciar Obra 
 [Espacio] -> Metamorfosis (Arrastrarse vs Flotar)
 [ F ]     -> Pantalla Completa
 [ G ]     -> Cambiar Paleta de Bioluminiscencia

Narrativa del proyecto:

El concepto del arte generativo,interactivo y performático que interpreta visualmente la canción “Vivarium” de la cantante japonesa Ado. La idea central es representar la tensión psicológica entre el encierro y la liberación a través de la metáfora de una criatura. Este macro-organismo muta de un ciempiés (el "Hakoniwa" o jardín cerrado) a una medusa que flota libremente (de forma manual), simbolizando la lucha por no dejar atras parte de si mismo y aprender a aceptarla.

Para lograrlo, el proyecto utiliza las bibliotecas p5.js y p5.sound para analizar el audio en profundidad y en tiempo real, vinculando características musicales específicas a un sistema de físicas matemáticas extremas:

Ritmo, amplitud y frecuencias: la amplitud (volumen general) controla directamente la tensión elástica del organismo. Los picos de intensidad expanden la estructura de la criatura simulando repulsión magnética, estirando su tejido sin dejar que se rompa, mientras deja a su paso un rastro de esporas bioluminiscentes. En paralelo, los agudos dictan la velocidad de movimiento y la turbulencia del entorno, pasando de una calma a una aceleración fuerte, mientras que los bajos y medios rigen la fuerza de la corriente oceánica y los destellos lumínicos que responden a la voz.

Algoritmo generativo: el núcleo de la obra es un motor físico híbrido. Utiliza Cinemática Inversa (IK) para construir un esqueleto morfológico dinámico que permite la metamorfosis en tiempo real. Este cuerpo es arrastrado por un campo de flujo (flowfield) de ruido Perlin que simula una marea viva y caótica. Finalmente, un algoritmo de red Plexus renderiza la criatura, trazando líneas geométricas y traslúcidas entre sus células según su proximidad. Esto crea un tejido orgánico y fractal que garantiza que cada movimiento de la medusa sea irrepetible y reaccione visceralmente al sonido.
*/

let cancion, fft, amplitud;
let audioIniciado = false;

let campoFlujo;
let quimera; 
let gotasMar = [];
let rastroCriatura = []; // NUEVO: Arreglo para el rastro de la criatura

// Estado Performático
let modoMedusa = false; 
let zoff = 0;
let paletas;
let estacionActual = 0;

let volumenSuavizado = 0;

function preload() {
  cancion = loadSound("Vivarium.mp3");
}

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100, 100);
  background(2, 5, 12);
  
  fft = new p5.FFT(0.8, 128);
  fft.setInput(cancion);
  amplitud = new p5.Amplitude();
  amplitud.setInput(cancion);
  
  configurarPaletas();
  campoFlujo = new CampoFlujo(35);
  
  for(let i=0; i<800; i++) gotasMar.push(new GotaMar());
  
  quimera = new Quimera(width/2, height/2, 30, 16, 14);
}

function draw() {
  if (!audioIniciado) { pantallaEspera(); return; }

  blendMode(BLEND);
  background(2, 5, 12, 35); 

  fft.analyze();
  let bajos = fft.getEnergy("bass");     
  let medios = fft.getEnergy("lowMid");  
  let agudos = fft.getEnergy("treble");  
  
  let volActual = amplitud.getLevel();
  volumenSuavizado = lerp(volumenSuavizado, volActual, 0.2); 

  campoFlujo.actualizar(agudos, bajos);

  blendMode(ADD); 

  // 1. EL MAR
  for (let gota of gotasMar) {
    gota.seguir(campoFlujo);
    gota.actualizar(agudos);
    gota.mostrar(bajos);
  }

  // 2. EL RASTRO DE LA CRIATURA (NUEVO)
  for (let i = rastroCriatura.length - 1; i >= 0; i--) {
    rastroCriatura[i].actualizar();
    rastroCriatura[i].mostrar();
    if (rastroCriatura[i].muerta()) {
      rastroCriatura.splice(i, 1);
    }
  }

  // 3. LA QUIMERA
  quimera.actualizar(campoFlujo, bajos, agudos, volumenSuavizado);
  quimera.mostrar(medios, volumenSuavizado);
}

// ==========================================
// CONTROLES VJ
// ==========================================
function mousePressed() { if (!audioIniciado) { cancion.play(); audioIniciado = true; noCursor(); } }
function keyPressed() {
  if (keyCode === 32) modoMedusa = !modoMedusa; 
  if (key === 'f' || key === 'F') fullscreen(!fullscreen()); 
  if (key === 'g' || key === 'G') {
    estacionActual = (estacionActual + 1) % paletas.length;
    quimera.actualizarColor();
  }
}
function windowResized() { resizeCanvas(windowWidth, windowHeight); campoFlujo.iniciar(); }

function pantallaEspera() {
  background(2, 5, 12); fill(255, 80); noStroke(); textAlign(CENTER, CENTER); textSize(16);
  text("EL VIVARIUM ESTÁ LISTO\nClick para sumergirse", width / 2, height / 2);
}

function configurarPaletas() {
  paletas = [
    { c1: color(210, 90, 80), c2: color(180, 80, 100) }, // Azul/Cian
    { c1: color(350, 90, 80), c2: color(15, 80, 100) },  // Sangre (Clímax)
    { c1: color(320, 90, 90), c2: color(280, 80, 70) },  // Magenta/Morado
    { c1: color(140, 90, 70), c2: color(90, 80, 90) }    // Verde Tóxico
  ];
}

// ==========================================
// EL MAR FLUIDO
// ==========================================
class CampoFlujo {
  constructor(escala) { this.escala = escala; this.iniciar(); }
  iniciar() {
    this.cols = floor(width / this.escala) + 1;
    this.rows = floor(height / this.escala) + 1;
    this.vectores = new Array(this.cols * this.rows);
  }
  actualizar(agudos, bajos) {
    zoff += modoMedusa ? map(agudos, 0, 255, 0.005, 0.04) : map(agudos, 0, 255, 0.002, 0.02);
    let fuerzaCorriente = map(bajos, 0, 255, 0.1, 2.0);
    let xoff = 0;
    for (let i = 0; i < this.cols; i++) {
      let yoff = 0;
      for (let j = 0; j < this.rows; j++) {
        let angulo = noise(xoff, yoff, zoff) * TWO_PI * 4;
        let v = p5.Vector.fromAngle(angulo);
        v.setMag(fuerzaCorriente);
        this.vectores[i + j * this.cols] = v;
        yoff += 0.05;
      }
      xoff += 0.05;
    }
  }
  fuerzaEn(x, y) {
    let col = floor(constrain(x / this.escala, 0, this.cols - 1));
    let row = floor(constrain(y / this.escala, 0, this.rows - 1));
    return this.vectores[col + row * this.cols].copy();
  }
}

class GotaMar {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = createVector(0, 0); this.acc = createVector(0, 0);
    this.velMaxBase = random(1, 4);
    this.posPrevia = this.pos.copy();
  }
  seguir(campo) {
    let f = campo.fuerzaEn(this.pos.x, this.pos.y); this.acc.add(f);
  }
  actualizar(agudos) {
    this.posPrevia.set(this.pos);
    this.vel.add(this.acc);
    let impulsoRitmo = map(agudos, 0, 255, 0, 8);
    this.vel.limit(this.velMaxBase + impulsoRitmo);
    this.pos.add(this.vel);
    this.acc.mult(0);
    if (this.pos.x > width) { this.pos.x = 0; this.posPrevia.set(this.pos); }
    if (this.pos.x < 0) { this.pos.x = width; this.posPrevia.set(this.pos); }
    if (this.pos.y > height) { this.pos.y = 0; this.posPrevia.set(this.pos); }
    if (this.pos.y < 0) { this.pos.y = height; this.posPrevia.set(this.pos); }
  }
  mostrar(bajos) {
    stroke(200, 90, map(bajos, 0, 255, 10, 40), 20); 
    strokeWeight(1);
    line(this.pos.x, this.pos.y, this.posPrevia.x, this.posPrevia.y);
  }
}

// ==========================================
// NUEVO: SISTEMA DE RASTRO DE LA CRIATURA
// ==========================================
class ParticulaRastro {
  constructor(x, y, colorBase) {
    this.pos = createVector(x, y);
    // Leve movimiento aleatorio para que flote en el agua
    this.vel = p5.Vector.random2D().mult(random(0.1, 0.5));
    this.vida = 255; // Nivel de opacidad inicial
    this.colorBase = colorBase;
    this.radio = random(1.5, 3.5);
    this.ritmoDesvanecimiento = random(3, 8); // Qué tan rápido desaparece
  }
  actualizar() {
    this.pos.add(this.vel);
    this.vida -= this.ritmoDesvanecimiento;
  }
  mostrar() {
    noStroke();
    let h = hue(this.colorBase);
    let s = saturation(this.colorBase);
    // Transparencia máxima de 40 para que sea un rastro "leve"
    fill(h, s, 100, map(this.vida, 0, 255, 0, 40)); 
    circle(this.pos.x, this.pos.y, this.radio * 2);
  }
  muerta() {
    return this.vida <= 0;
  }
}

// ==========================================
// LA QUIMERA EXTREMA
// ==========================================
class Quimera {
  constructor(x, y, numEspina, numPatas, largoPataMax) {
    this.cabeza = createVector(x, y);
    this.velCabeza = createVector(0, 0);
    
    this.distanciaNodosBase = 3; 
    this.espina = new CadenaIK(x, y, numEspina); 
    
    this.apendices = [];
    let pasoEspina = floor(numEspina / numPatas); 
    
    for (let i = 0; i < numPatas; i++) {
      let anclajeCienpies = constrain((i * pasoEspina) + 1, 0, numEspina - 1);
      let anguloMedusa = map(i, 0, numPatas, 0, TWO_PI);
      let longitudReal = (i % 2 === 0) ? largoPataMax : floor(largoPataMax * 0.5);
      this.apendices.push(new Apendice(x, y, longitudReal, anclajeCienpies, anguloMedusa));
    }
    
    this.actualizarColor();
  }

  actualizarColor() {
    let p = paletas[estacionActual];
    this.colorBase = lerpColor(p.c1, p.c2, 0.5);
  }

  actualizar(campo, bajos, agudos, volumen) {
    let expansionMagnetica = map(volumen, 0, 0.5, 1, 10); 
    let distanciaNodosActual = this.distanciaNodosBase * expansionMagnetica;

    let corriente = campo.fuerzaEn(this.cabeza.x, this.cabeza.y);
    let aceleracion = corriente.copy().mult(2.5);
    
    if (mouseIsPressed) {
      let jalonMouse = p5.Vector.sub(createVector(mouseX, mouseY), this.cabeza);
      jalonMouse.setMag(2.0);
      aceleracion.add(jalonMouse);
    }
    
    this.velCabeza.add(aceleracion);
    let velMax = map(agudos, 0, 255, 0.5, 20); 
    this.velCabeza.limit(velMax);
    this.cabeza.add(this.velCabeza);
    
    if (this.cabeza.x < 0) this.cabeza.x += width;
    if (this.cabeza.x > width) this.cabeza.x -= width;
    if (this.cabeza.y < 0) this.cabeza.y += height;
    if (this.cabeza.y > height) this.cabeza.y -= height;

    this.espina.seguir(this.cabeza.x, this.cabeza.y, distanciaNodosActual);

    let radioCampana = 20 + (expansionMagnetica * 8); 

    for (let i = 0; i < this.apendices.length; i++) {
      let ap = this.apendices[i];
      let objetivoAnclaje;
      
      if (modoMedusa) {
        let radioDinamico = (i % 2 === 0) ? radioCampana : radioCampana * 0.4;
        let offsetCampana = p5.Vector.fromAngle(ap.anguloCampana).mult(radioDinamico);
        objetivoAnclaje = p5.Vector.add(this.cabeza, offsetCampana);
      } else {
        objetivoAnclaje = this.espina.segmentos[ap.indiceAnclajeBase];
      }
      
      ap.posAnclajeVirtual.x = lerp(ap.posAnclajeVirtual.x, objetivoAnclaje.x, 0.07);
      ap.posAnclajeVirtual.y = lerp(ap.posAnclajeVirtual.y, objetivoAnclaje.y, 0.07);

      ap.cadena.seguir(ap.posAnclajeVirtual.x, ap.posAnclajeVirtual.y, distanciaNodosActual);
      ap.cadena.aplicarFuerzaExterna(campo);
    }
  }

  mostrar(medios, volumen) {
    let h = hue(this.colorBase);
    let s = saturation(this.colorBase);
    
    let todosLosNodos = [];
    todosLosNodos.push(this.cabeza);
    todosLosNodos = todosLosNodos.concat(this.espina.segmentos);
    for (let ap of this.apendices) {
      todosLosNodos = todosLosNodos.concat(ap.cadena.segmentos);
    }

    let distanciaConexionPlexus = modoMedusa ? 
                                  map(volumen, 0, 0.5, 50, 180) : 
                                  map(volumen, 0, 0.5, 30, 100);

    for (let i = 0; i < todosLosNodos.length; i++) {
      for (let j = i + 1; j < todosLosNodos.length; j++) {
        let nA = todosLosNodos[i];
        let nB = todosLosNodos[j];
        let dSq = (nA.x - nB.x)**2 + (nA.y - nB.y)**2;
        
        if (dSq > 0 && dSq < distanciaConexionPlexus**2) {
          let d = sqrt(dSq);
          let grosor = map(d, 0, distanciaConexionPlexus, 3, 0.05);
          let opacidad = map(d, 0, distanciaConexionPlexus, 100, 5);
          
          stroke(h, s, map(medios, 0, 255, 60, 100), opacidad);
          strokeWeight(grosor);
          line(nA.x, nA.y, nB.x, nB.y);
        }
      }
    }

    noStroke();
    let brillo = map(medios, 0, 255, 50, 100);
    
    // Probabilidad de soltar rastro basada en el volumen musical
    let probabilidadRastro = map(volumen, 0, 0.5, 0.01, 0.08);

    for (let nodo of todosLosNodos) {
      let tamañoParticula = map(volumen, 0, 0.5, 1.5, 5); 
      
      fill(h, s, brillo, 20);
      circle(nodo.x, nodo.y, tamañoParticula * 5); 
      fill(h, s, 100, 90);
      circle(nodo.x, nodo.y, tamañoParticula);     

      // NUEVO: La criatura va dejando partículas de rastro al moverse
      if (random() < probabilidadRastro) {
        rastroCriatura.push(new ParticulaRastro(nodo.x, nodo.y, this.colorBase));
      }
    }

    fill(0, 0, 100, 100); 
    circle(this.cabeza.x, this.cabeza.y, map(volumen, 0, 0.5, 5, 20));
  }
}

// ==========================================
// CINEMÁTICA INVERSA ADAPTABLE
// ==========================================
class CadenaIK {
  constructor(x, y, numSegmentos) {
    this.segmentos = [];
    for (let i = 0; i < numSegmentos; i++) {
      this.segmentos.push(createVector(x, y));
    }
  }

  seguir(targetX, targetY, longitudDinamica) {
    let objetivo = createVector(targetX, targetY);
    this.segmentos[0] = objetivo;

    for (let i = 1; i < this.segmentos.length; i++) {
      let dir = p5.Vector.sub(this.segmentos[i-1], this.segmentos[i]);
      dir.setMag(longitudDinamica); 
      this.segmentos[i] = p5.Vector.sub(this.segmentos[i-1], dir);
    }
  }

  aplicarFuerzaExterna(campo) {
    for (let i = 1; i < this.segmentos.length; i++) {
      let f = campo.fuerzaEn(this.segmentos[i].x, this.segmentos[i].y);
      this.segmentos[i].add(f.mult(0.8));
    }
  }
}

class Apendice {
  constructor(x, y, numSegmentos, indiceEspina, anguloCampana) {
    this.cadena = new CadenaIK(x, y, numSegmentos);
    this.indiceAnclajeBase = indiceEspina; 
    this.anguloCampana = anguloCampana;    
    this.posAnclajeVirtual = createVector(x, y); 
  }
}
```

11. Enlace al sketch.

https://editor.p5js.org/Nikeal/sketches/5oBOHkeDT

12. Capturas o registros de momentos importantes de la pieza.

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/bbb3fda8-1fa4-479d-9806-0eb8402a7134" />

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/1be6b3d7-701a-4f10-a207-87862d4c4272" />

<img width="1912" height="1079" alt="image" src="https://github.com/user-attachments/assets/8ec5b533-cf13-4b82-a724-4f17455873c9" />

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/a42567e5-ff00-4ea1-b993-dfa7964fb261" />


## Bitácora de reflexión
