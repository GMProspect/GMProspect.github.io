# Dinámica neuronal, control y generación espacial

## Memoria de desarrollo de un banco experimental de simulación y entornos procedurales

**Gustavo Adolfo Matheus González · GMProspect**

Publicado el 1 de noviembre de 2026. Registros de desarrollo y ensayos revisados: 7–27 de octubre de 2026.

El trabajo reúne dos frentes de desarrollo: un entorno de experimentación con dinámica neuronal y control de agentes, y un subsistema gráfico procedural con navegación sobre superficies de orientación variable. Su relación se encuentra en el tratamiento del estado: cómo se produce una señal, cómo se transforma en una orden y cómo se representa su efecto en un espacio observable.

La memoria los presenta como una misma línea de ingeniería. El alcance comprende componentes ejecutables, ensayos de navegación y control, modificaciones del decodificador, visualización de actividad, fusión de entornos y construcción procedural de escenarios. Se documentan además la composición entre dos juegos y una variante que recrea mecánicas en un único anfitrión. El control neuronal del mashup completo continúa como objetivo de integración.

![Arquitectura conceptual y estado de integración](../assets/arquitectura-simulacion.svg)

*Figura 1. Organización funcional. Las flechas representan el recorrido de información propuesto; la unión con el entorno de gravedad variable corresponde a una etapa posterior. El diagrama no acredita una ejecución conjunta de todos los componentes.*

## 1. Reconstrucción y representación

El primer frente utiliza un modelo neuronal de integración y disparo con fuga, asociado a una reconstrucción conectómica de insecto. El estado de cada unidad evoluciona por integración temporal; los impulsos emitidos se propagan por conexiones ponderadas. Las poblaciones sensoriales reciben estímulos del entorno y las poblaciones descendentes alimentan canales de movimiento.

La procedencia local del conjunto MaleCNS registra **176.422 neuronas, 6.287.749 conexiones retenidas y 90.296.905 sinapsis representadas en sus pesos**. Son magnitudes diferentes: una conexión puede agrupar múltiples contactos sinápticos. La preparación conserva enlaces con al menos cinco sinapsis y excluye autoconexiones. Estas cifras describen el conjunto utilizado, no una medición nueva ni una reconstrucción anatómica propia. Fuente: [procedencia del modelo base](https://github.com/blendi-remade/fly-brain-minecraft/blob/main/PROVENANCE.md).

La representación espacial permite inspeccionar regiones y posiciones anatómicas; la telemetría funcional muestra tasas de disparo y canales motores. Una representación visible facilita el diagnóstico, pero no sustituye el registro numérico de un ensayo.

## 2. Observación de una asimetría motora

Durante los ensayos locales se registró una respuesta lateral desigual en el canal de giro. La estimulación olfativa a igual tasa nominal por neurona produjo actividad sostenida asimétrica en las poblaciones descendentes examinadas y un mando de giro persistente. La observación motivó el análisis del decodificador y de sus condiciones de arranque.

La igualdad de tasas de estimulación no asegura igualdad de entrada bilateral total ni trenes de impulsos idénticos. El análisis estructural aporta hipótesis sobre la propagación, pero no demuestra por sí solo una causa fisiológica. El resultado se trata como un comportamiento del modelo y de su configuración experimental.

Se introdujo una compensación de línea base en la salida de giro, conservando los pesos del conectoma. Su evolución puede expresarse como **dB/dt = (u − B)/τ**, donde u es el mando previo a la compensación, B la línea base y τ una constante de adaptación de 3 s. Tras una fase inicial de 1 s, la salida usa la diferencia entre el mando y su línea base.

La revisión detectó que el promedio inicial incorporaba la rampa de un suavizador. Esa condición sesgaba la calibración aun con una entrada constante. La tercera revisión estima la línea base desde la señal sin suavizar y limita la adaptación al canal investigado. Se añadieron estímulos deterministas de ambos signos y comprobaciones del mando acumulado.

El informe de pruebas conservado registra **14 casos ejecutados, sin fallos ni errores**, el 7 de octubre. La revisión también registra una compilación correcta. El criterio acumulado se expresa en **yaw·s**, la integral temporal de un mando normalizado; convertirlo en ángulo exige aplicar la ganancia de movimiento. No corresponde denominarlo radianes ni certificación formal.

El filtro mantiene dos límites conocidos: atenúa órdenes de giro legítimas si son sostenidas y puede producir una respuesta contraria cuando se retira una entrada persistente. La corrección del arranque está verificada en el decodificador; la trayectoria corporal con y sin adaptación y con distintas capas de reflejos sigue pendiente de comparación controlada.

## 3. Control, navegación y prioridades

El control se organiza en tres niveles funcionales: objetivos de recorrido, ejecución de navegación y señales reflejas derivadas del modelo neuronal. La instrumentación permite consultar el estado del entorno, solicitar desplazamientos y observar las salidas del piloto experimental.

Un ensayo de navegación registró llegada al objetivo en **8,2 s**. Es evidencia de una operación concreta de desplazamiento, bajo la configuración documentada. La atribución del recorrido corresponde al ejecutor de navegación; ese resultado no demuestra que el conectoma resolviera la planificación.

En otra observación, una aproximación rápida produjo eventos de escape lateralizados. Esto permitió explorar una prioridad de reacción sobre el movimiento ordinario. La evaluación posterior midió interrupciones, recuperación del recorrido y condiciones de ensayo repetibles, relacionando cada cambio corporal con la capa que lo ordenó.

La separación entre actividad neuronal, mando decodificado y desplazamiento efectivo es parte del método. Un canal centrado no garantiza ausencia de rotación cuando existen otros reflejos programados; una llegada tampoco permite atribuir la decisión a todas las capas presentes.

## 3.1. Ensayos posteriores de control neuronal

Los registros de la carpeta de trabajo vigente amplían el ensayo inicial. El 24 de octubre se realizaron series de **40, 30 y 30 ensayos**, con condiciones sin reflejo, reflejo programado y reflejo neuronal. En la primera serie, reducir daño mediante huida no aseguró alcanzar la meta: el árbitro entraba en un bucle ante perseguidores.

La segunda serie incorporó evasión con progreso: las tres condiciones alcanzaron la meta en 10 de 10 ensayos por condición. El daño medio fue 3,4 sin reflejo, 1,5 con reflejo programado y 3,2 con reflejo neuronal. La tercera serie añadió combate: los daños medios fueron 3,3, 2,4 y 1,8, respectivamente; no se estableció ventaja estadística del reflejo neuronal bajo el criterio previamente fijado.

Estos resultados documentan una integración funcional del modelo neuronal con el jugador y una evaluación comparativa del control. El modelo de mosca aporta la señal que desencadena una acción; la navegación y la selección del objetivo de ataque corresponden a otras capas. Los ensayos no establecen una superioridad general del control neuronal.

![Resultados comparados de tres series experimentales](../assets/control-trials.png)

*Figura 2. Metas alcanzadas y daño medio por condición a partir de los análisis conservados. Cada condición incluye diez ensayos; las barras describen las medias, sin representar intervalos de incertidumbre.*

## 3.2. Fusión entre dos entornos virtuales

El segundo desarrollo comprende una integración entre **dos entornos virtuales de distinta representación espacial**, con dos rutas técnicas distintas: una ejecución de ambos entornos enlazados y una recreación de mecánicas dentro de un único proceso anfitrión. La documentación local conserva fuentes, trazas y validaciones de ambas rutas.

En la ruta de dos procesos se intercambian estado de cámara, orientación, referencias del suelo y eventos de interacción. La imagen del entorno volumétrico se compone sobre el mundo anfitrión; acciones sobre entidades y bloques se trasladan a sus representaciones correspondientes. El trabajo incluye conversión de coordenadas, entrada de usuario, inventario, vehículos y tratamiento de colisiones. La implementación deriva parcialmente de un ejemplo de integración preexistente, adaptado al nuevo anfitrión.

El diagnóstico del 27 de octubre documentó **244 s de observación**, con un retraso mediano de un fotograma y máximo de cinco en la imagen compuesta. El desacoplamiento al girar motivó una reproyección rotacional desde la cámara usada para capturar la imagen hacia la cámara de presentación. La corrección se compiló y se verificaron sus cálculos fuera del juego; su resultado completo en ejecución seguía pendiente en el último registro revisado. No se atribuye al sistema una latencia submilisegundo.

La ruta nativa recrea construcción y rotura de bloques, una barra de selección, cámara en primera persona, criaturas, fuego y explosivos dentro del entorno anfitrión. El registro del 26 de octubre documenta una cadena de tres explosivos, daño sobre entidades, interacción con vehículos y recuperación de una construcción después de guardar y reiniciar. También conserva pruebas de geometría, colisiones, formatos de persistencia e instalación reversible.

Se trata de una **fusión funcional en desarrollo**, con comportamientos comprobados en escenas concretas. La compatibilidad suficiente con campaña, interiores y todas las interacciones sigue abierta. El cerebro de mosca y el mashup forman parte del mismo proyecto, pero los registros revisados no demuestran que el modelo neuronal haya completado misiones dentro del mashup.

![Dos rutas de integración entre los entornos](../assets/fusion-entornos.svg)

*Figura 3. Esquema elaborado para esta memoria a partir de las fuentes locales: composición entre dos procesos y recreación nativa en un proceso. Es una representación de arquitectura, no una captura de ejecución.*

## 3.3. Automatización de ensayos y comprobaciones

La compatibilidad se trabajó con una cadena de herramientas, además de las pruebas manuales. En el entorno neuronal se prepararon escenarios repetibles de aproximación, esquiva y combate, con registro de estímulos, activación de reflejos, trayectorias, daño y llegada a la meta. Las series comparativas se analizaron con condiciones fijadas antes del ensayo y resultados conservados por corrida.

Para la integración entre juegos se desarrollaron herramientas de construcción y restauración de recursos, comprobación de archivos, diagnóstico de estado en ejecución y pruebas de instalación reversible. En la ruta nativa, las sondas se vinculan a un proceso de juego concreto y registran respuestas de acciones individuales; las pruebas aisladas comprueban formatos, persistencia y recuperación ante fallos simulados. La automatización permitió repetir condiciones y detectar incompatibilidades, pero no reemplaza la validación visual y de misiones dentro de los juegos.

El registro distingue pruebas ejecutadas en el juego, pruebas aisladas y componentes preparados pero pendientes de ensayo. Esa distinción evita atribuir a una compilación o a una simulación lo que solo una sesión de juego puede demostrar.

## 4. Motor de escena y construcción procedural

El segundo frente desarrolla un subsistema de generación espacial: un componente construye escenario, iluminación, cámara, jugador, instrumento de interacción y elementos de interfaz durante la ejecución. El recorrido incluye un prototipo bidimensional de cambio de fase y una versión tridimensional de manipulación gravitatoria.

En el prototipo tridimensional, la superficie seleccionada define una dirección de atracción o repulsión. La cámara y el cuerpo se reorientan de forma progresiva respecto de la nueva vertical. El sistema incorpora desplazamiento, salto y recuperación de posición al abandonar el área de juego.

Esta capa procedural concentra la construcción del mundo y su comportamiento en código, sobre un entorno gráfico y físico existente. La evidencia disponible respalda un motor de escena a nivel de aplicación. La composición entre procesos del mashup es un subsistema distinto, descrito en la sección anterior. El prototipo procedural no se presenta como un renderizador independiente.

## 5. Relación entre los componentes

En el circuito de control, el entorno produce observaciones; el modelo neuronal las transforma en actividad; el decodificador produce mandos; el árbitro establece prioridades; el ejecutor aplica el movimiento y la telemetría permite comparar la orden con su efecto.

El entorno procedural aporta un espacio controlable para estudiar orientación, superficies y transformaciones de cámara. Su incorporación al mismo circuito exige definir unidades, referencias espaciales, tiempos de actualización y criterios de interrupción. Esta interfaz pertenece al siguiente tramo de integración.

La memoria describe las rutas de integración y sus resultados específicos. La unión del control neuronal con el mashup completo y el prototipo procedural sigue como objetivo experimental.

## 6. Estado de la evidencia

| Componente | Resultado documentado | Alcance pendiente |
| --- | --- | --- |
| Decodificación de giro | Calibración inicial corregida; informe de 14 pruebas sin fallos | Trayectorias comparadas y respuesta a órdenes sostenidas |
| Navegación instrumental | Llegada registrada en 8,2 s en un ensayo | Repeticiones, obstáculos y recuperación |
| Control neuronal del jugador | Escape lateralizado y tres series comparativas de 40/30/30 ensayos | Ventaja frente al control programado no establecida |
| Fusión de juegos | Puente visual y de eventos; recreación nativa con pruebas conservadas | Campaña, interiores y validación de las últimas correcciones |
| Generación procedural | Código local y captura de un entorno de gravedad variable | Integración con el circuito neuronal |
| Rendimiento neuronal | Registros entre 0,28× y 1,89× tiempo real bajo cargas distintas | Medición conjunta de latencia y rendimiento sostenido |

Las velocidades corresponden a condiciones de estimulación diferentes; no forman una comparación controlada ni garantizan tiempo real en cualquier escena. La compilación y las pruebas citadas son registros conservados de octubre, no una nueva ejecución realizada para esta publicación.

## 7. Continuidad del trabajo

Los siguientes ensayos deben registrar señal de entrada, actividad neuronal, mando previo y posterior al filtro, origen del movimiento y trayectoria final. La comparación debe variar por separado la adaptación, los reflejos y la navegación. Así será posible distinguir la contribución de cada componente y localizar fallos sin confundirlos con propiedades del sistema biológico.

El interés a largo plazo es profundizar en biología computacional y análisis de redes. Una transferencia hacia problemas vasculares requeriría modelos físicos, datos y validación propios; los ensayos descritos aquí se circunscriben a simulación y control experimental.

La integración resulta informativa cuando cada transición puede examinarse: qué se recibió, qué se calculó y qué cambió en el entorno. Conservar esa trazabilidad permite que una limitación se convierta en una pregunta concreta para el siguiente ensayo.

## Procedencia y distribución

La autoría de esta memoria y de la integración local corresponde a Gustavo Adolfo Matheus González. La base neuronal procede del [proyecto de referencia](https://github.com/blendi-remade/fly-brain-minecraft); el código base se distribuye bajo MIT y sus datos bajo CC BY 4.0. Los datos anatómicos proceden del proyecto [Male CNS](https://male-cns.janelia.org/), de FlyEM/Janelia y sus instituciones participantes. Las adaptaciones locales descritas no deben atribuirse al proyecto base. La ruta de composición toma como referencia un ejemplo MIT de [universal-modder](https://github.com/rehan-remade/universal-modder); sus adaptaciones y la ruta nativa se distinguen en el registro privado. Se conservan las licencias y los avisos de procedencia correspondientes.

Esta memoria publica descripción funcional, alcance y figuras seleccionadas. La instrumentación operativa y los registros completos se conservan en un repositorio privado separado. Los archivos comerciales de los juegos permanecen exclusivamente en la instalación local. La publicación no distribuye código del modelo, datos anatómicos, ejecutables ni manuales operativos. Las cifras de ensayos locales se presentan como resultados documentados; su reproducción independiente requiere los artefactos de ejecución correspondientes.
