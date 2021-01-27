---
chapter: Desarrollo
graphicspath: ./../imagenes/
bibliography: [D:/Files/Documents/source/Tesis/mendeley/tesis_tls_cimat.bib] 
csl: D:/Files/Documents/source/Tesis/apa7-spanish.csl 
lang: es 
---

# Arquitectura
\begin{figure}[H]
    \centering
\includegraphics[width=\textwidth]{arquitectura.png}
    \caption{Arquitectura de un agente de control.}
    \label{fig:arq1}
\end{figure}

## Tráfico
Los vehículos que se mueven en los carriles de la intersección y cruzan de un
carril a otro en nodos llamados intersecciones.  Sobre las intersecciónes puede
haber ubicados semáforos.

## Propiedades de la intersección
Son las propiedades intrínsecas de la intersección que no suelen cambiar con el
paso del tiempo:

+ Número de calles.

+ Carriles en cada calle.

+ Extensión aproximada de cada carril.

+ Estructura de la intersección: todas las posibles conexiones entre carriles.

+ Derechos y prioridades de paso.

+ Disposición de las luces del semáforo (cuántas hay y a que intersección
  apuntan).

+ Existencia de cruces y puentes peatonales, y si tienen algún botón que permita
  detener el tráfico.

## Observador de eventos
Su única tarea es ser el sentido de la vista del agente de control. Tiene
funciones para observar eventos de interés que suceden con regularidad en el
mundo real en un momento dado y sus propiedades, tales como:

+ El paso de los vehículos, con propiedades como:
  + Ruta que toma (pueden seguir derecho o girar en algún sentido).
  + Velocidad promedio aproximada.
  + Cuanto tiempo permanece detenido.
  + Aceleración aproximada.
+ Cambios de luces (en que carril y  a que color cambió).

Y también notifica eventos emergentes y sus propiedades, como:

+ Aparición de un vehículos de prioridad (de que tipo y en que carril).

+ Un accidente (que carril obstruye).

Todo lo observado lo comunica  al *Registrador de eventos* indicando el tiempo
exacto en el que sucedió (*timestamp*).

## Registrador de eventos
Es el encargado de guardar un caché de los datos crudos enviados por el
*Observador de eventos* e interpretarlos, para cada hora generar reportes con
datos derivados que se almacenan en la base de datos. El reporte generado agrupa
los datos por ciclo de semáforo, siendo éstos los siguientes:

- Flujo por carril, es decir, el número de vehículos que pasan por cada carril cada determinado tiempo.

- Tiempo de espera promedio por carril.

- Densidad de cada carril.

- Cola más larga en la señal roja.

- Número de vehículos que pasan en señal verde.

- Tiempo ocioso de la señal verde.

- Cola más larga próxima fase.

- Tiempo de cambio de la cola más larga en señal roja.

También recibe datos al exterior a través de la *Interfaz de comunicación*, que
se almacenan selectivamente en la base de datos.

## Interfaz de comunicación
Permite enviar y recibir reportes de otros agentes de control, así como
notificaciones de eventos externos que pueden alterar el tráfico, como:

- Alertas de desastre natural.

- Eventos públicos y días festivos.

- Manifestaciones.

## Comparador
Se encarga de comparar constantemente el comportamiento de los eventos
registrados con el comportamiento predicho. Cuando estos difieren de manera
significativa, el comparador solicita al *Modelo predictivo* que genere una
nueva predicción. También se encarga de solicitar nuevas predicciones cada que
un evento externo lo solicite.

## Modelo predictivo
Se alimenta del histórico de reportes generados por el *Registrador de eventos*
que obtiene de la base de datos para predecir del comportamiento de una
intersección durante la siguiente hora. Los datos que genera son los mismos que
figuran el los reportes creados por el *Registrador de eventos*. También hace
predicciones bajo demanda cuando el *Comparador* lo solicita.

<!-- **(Incluir aquí gráfica que represente el rango de tiempo en el que son válidas las predicciones, la estimación de pérdida de tiempo efectivo por el tiempo de generación, etc.)** -->

Ya que el proceso de hacer una predicción y su posterior optimización es un
proceso intensivo, las peticiones para nuevas predicciones suceden cada hora,
pero de manera escalonada. Por ejemplo: si existen 4 agentes de control, uno
iniciará una petición de predicción a las 2:00 p.m., mientras que otra los hará
a las 2:15 p.m., otro a las 2:30, y así sucesivamente, de tal manera que el
tiempo entre cada hora se se reparta lo más igualitariamente posible. De la mano
con estas peticiones escalonadas, las peticiones se realizarán de manera
asíncrona usando hilos de procesamiento para lograr paralelizar el procesamiento
de varias intersecciones a la vez de ser necesario. De esta manera, se aprovecha
también el sistema de encolamiento de hilos nativo de los sistemas operativos,
que resulta particularmente útil para no atrasar las predicciones programadas
por las realizadas bajo demanda.

 <!-- todo: (Incluir aquí gráfica de encolamiento de hilos) -->

<!-- todo: Predice que cantidad de trafico relativo habrá en determinada hora (a definir si será de manera numérica o con expresiones relativas de lógica difusa, como *poco*, *mucho* o *normal*). -->

## Categorizador de parámetros
Se encarga de categorizar la influencia de los parámetro predichos entre sí
mismos y como finalmente influyen sobre el comportamiento global del tráfico,
para así obtener conclusiones en forma de restricciones y reglas aplicadas para
al temporizado de los ciclos del semáforo. Categoriza intervalos de tiempo que
presentan una serie de condiciones a términos abstractos, como: "hora pico",
"hora sin actividad" o "tráfico esporádico", para así poder usar esas categorías
como condiciones para las reglas y restricciones vistas anteriormente.
Categoriza como ventajoso o perjudicial el incremento o decremento de un
parámetro respecto a parámetros que se busquen incrementar o decrementar (como
el tiempo de espera, la generación de colas, etc). De ser necesario, infiere una
función de como su variación afecta a estos parámetros.

Ejemplo verbal: si se hacen ciclos en verde un un carril **mayores a** 45
segundos durante **horas pico**, se genera una **cola de espera perjudicial a la
fluidez** del carril perpendicular, entonces se infiere una regla que impida que
se rebase ese tiempo de ciclo dadas esas condiciones, y en caso de que se tenga
que se rebase, se tenga la función que indica que tanto perjudica ese aumento
para calcular los tiempos de ciclo que produzcan el mejor costo-beneficio (pues
se tienen que tomar en cuenta el resto de los parámetros).

Este módulo analiza constantemente los reportes almacenados en la base de datos
para realizar el proceso mencionado anteriormente y todo lo inferido se guarda
de manera centralizada como una base de conocimientos central como registros en
la base de datos.

Este módulo se inspiro en la llamada *Colección de hechos* usada por
\textcite{JoelTrejo2006}.

> La colección de hechos alberga los datos correspondientes a la aplicación de
> determinada estrategia de control cuando se presentan ciertas condiciones en el
> tráfico observado. La colección de hechos puede desempeñar el papel de memoria
> auxiliar en la cual se registran los razonamientos llevados a cabo.
(p. 47)

## Motor de priorización
Se encarga de asignar un valor numérico a cada carril, que indica su prioridad.
Entre mayor el número, mayor la prioridad. Este número de genera usando como
base la demanda predicha, aunque también es posible usar restricciones y reglas
definidas por el *Categorizador de parámetros*.

## Generador de intervalos
Usa los valores generados por el *Motor de priorización* para generar una
proporción de la duración de cada fase del semáforo en la duración total del
ciclo. Posteriormente usa como base las reglas y restricciones generados por el
*Categorizador de parámetros* para generar varias propuestas de la duración
total del ciclo de fases de luces. Se obtiene como salida varios juegos de
propuestas de duraciones de fases que pueden haber tomado en cuenta diferentes
reglas.

## Constructor de redes de Petri
A partir de las propuestas de intervalos generadas por el *Generador de
intervalos*, se encarga de construir una red de Petri temporizada para cada una
de ellas.
<!-- Como se pueden configurar las redes de petri para lograr mejores ciclos de semáforos? Hay alguna técnica de acomodo? Que ventaja tiene vs usar solo tiempos? (creo que la respuesta es la coordinacion cuando son muchos carriles). -->

## Optimizador
Recibe múltiples propuestas de redes de Petri y a través de algoritmos
genéticos, cruza los genes de las diferentes propuestas (que tomaron en cuenta
diferente reglas para construirse) buscando maximizar los parámetros
considerados como ventajosos (como el flujo) y minimizar los considerados como
perjudiciales (como las cosas y los tiempos de espera). La información de cuáles
son estos parámetros se obtiene del *Categorizador de parámetros*. Al final da
como salida una red de Petri optimizada.

## Controlador
Tiene las funciones para necesarias para leer la red de Petri temporizada que
recibe y controlar los cambios de luces según lo indique la red.

## Luces de semáforo
Son la manera en la que se plasman los intervalos de luces generados y son lo
que físicamente indica a los conductores cuando deben circular o detenerse.

# Creación de simulación

## Red de tráfico

Para probar y desarrollar a detalle la arquitectura se usará el simulador de
tráfico urbano SUMO, que incluye prácticamente todas las herramientas
necesarias.

Se creó la red de tráfico usando un script llamado
[osmWebWizard](https://sumo.dlr.de/docs/Tutorials/OSMWebWizard.html), que
permite seleccionar un área geográfica real desde OpenStreetMaps y convertirla
en el tipo de archivo que utiliza el simulador.

Para motivos de prueba, se usó una intersección de Mérida conocida, para poder
simular flujos a partir de lugares familiares, y en dado caso de ser necesario
recolectar datos reales. La intersección en cuestión está ubicada en el sur de
la ciudad, donde Circuito Colonias se cruza con la calle 50.

Para corroborar que los datos generados por el script sean certeros, se comparó
la generación de la intersección con imágenes reales de Google Maps, y hubo que
ajustar el ancho del carril que corresponde a la calle 50 de uno a dos carriles
(realmente tiene como 4, pero siempre hay coches estacionados a ambos costados,
al final se pueden aprovechar solo 2). Desconozco si las calles adyacentes
tienen el ancho correcto, pero considero que solo ésta afectan al objetivo de la
simulación.

Los datos recuperados por osmWebWizard, a excepción al numero de carriles de la
calle 50, fueron bastante certeros, y la intersección de interés tiene
correctamente los derechos de paso del semáforo.

Una vez montada la simulación, fue necesario prepararla para manipularse
programáticamente, por lo que el semáforo a controlar se renombró a
*semaforo_circuito_colonias* y se modificó el comportamiento de los semáforos
para que sean estáticos y solo cambien cuando se les indique manualmente usando
código.


SUMO no maneja las señales de trafico por calle, si no por conexión entre
carriles (para ilustrar los derechos de paso y giros permitidos). En el netedit
es posible agruparlos, pero decidí dejarlos sin agrupar para tener absoluto
control de manera programática de cada conexión, y así me será posible agrupar
las conexiones de la manera más lógica que se asemeje a la vida real. Descubrí
el orden de como maneja SUMO los indices de cada enlace: en orden de las
manecillas del reloj, en teoría empezando por arriba, pero en este caso empieza
por la derecha. Toma en cuenta para numerar los carriles que tienen tráfico
entrando, los que solo reciben tráfico no se numeran (tiene sentido que sea
así).

## Modelado de la arquitectura

Con la red de tráfico lista, el objetivo es programar la arquitectura
previamente propuesta en Python, y para ello se utilizará una interfaz para
manipular la simulación en tiempo real llamada TraCI. Dicha interfaz ya viene
incluído en la instalación por defecto de SUMO.

<!-- Previamente ya la había utilizado y ya tengo mi manera de consumirlo, y me guié de la [guía oficial](https://sumo.dlr.de/docs/TraCI/Interfacing_TraCI_from_Python.html). -->

Los primeros módulos de la arquitectura a programar son el *Observador de
eventos* y el *Registrador de eventos*, y para ello se modelaron las propiedades
de una intersección en clases que se relacionan entre si.

La más básica es Edge, que es lo equivalente a una calle y contiene sus
propiedades asociadas:

-   conection_uses_from: relaciona el Edge con una Conection.

-   conection_uses_to: relaciona el Edge con una Conection.

-   name: nombre o apodo para la calle.

-   num_lanes: numero de carriles.

-   is_traffic_input: indica si el trafico entra por esta calle.

-   associated_detector_name: nombre del detector de trafico asociado a esta
    calle.

-   street_name: nombre real de la calle.

-   aprox_length: largo aproximado de la calle.

-   aprox_total_width: ancho aproximado de la calle completa que incluye a todos
    los carriles.

Las calles están conectadas entre si, y esta relación se representa a través de
la clase Conection, que indica una conexión simple entre Edges (calles):

-   intersection: relaciona que una Intersection puede tener varias Conections.

-   from_edge: desde que calle viene el trafico.

-   to_edge: hacia que calle viene el trafico.

-   validate: si se deben validar que los parámetros recibidos representen una
    conexión válida. Por defecto es False, para evitar realizar esta operación
    cuando no sea necesario para ahorrar procesamiento.

Las calles se agrupan en intersecciones que tienen conexiones entre ellas y
posiblemente un semáforo. Esto se representa en la clase Intersection:

-   name: nombre o apodo para la intersección.

-   associated_traffic_light_name: el nombre del semáforo asociado a la
    intersección.

-   conections: lista de todas las conexiones entre calles.

Siguiendo la arquitectura, los datos de cada intersección y del estado de la
simulación deben almacenarse en algún lugar para posteriormente realizar un
reporte que se guardará para su análisis. Se escogió medio de almacenamiento
temporal una base de datos en sqlite, y se utilizó la librería Pony ORM para
convertir el modelado en clases a tablas de una base de datos.

Ahora lo que sigue es modelar el EventObserver


<!-- Actualmente se está trabajando en la implementación del framework Flow, que
permite manipular conectar fácilmente SUMO con librerías de Reinforcement
Learning, así como incluye métodos para generar tráfico de manera más realista. -->

<!-- # Notas
+ Tomar inspiracion de los docs de [Edges value retrieval](https://sumo.dlr.de/docs/TraCI/Edge_Value_Retrieval.html#extended_retrieval_messages) para los calculos derivados de parametros para los reportes
  + Tambien de vehiculos
+  -->