## 1 Feb 2021
Flow es un framework que permite usar la librería para *Reinforment Learning*
RLlib en conjunto con diversas herramientas para facilitar crear entornos de
simulación usando como simuladores de tráfico a SUMO y Aimsun. Entre los dos, el
mejor soportado es el primero. El *RL* se puede usar tanto para conducción
autónoma, como para controlar los semáforos. Para ello, se puede utilizar como
backend a Tensorflow o Pythorch, aunque se promete que la mayoría de las
funciones son agnósticas a los frameworks de *Machine Learning*.

### Ventajas
Leí la documentación del framework Flow, y posee las siguientes herramientas que
directamente me pueden ser útiles:

+ Incluye redes de tráfico pensadas para hacer pruebas, como uno que es una
  matriz de semáforos, otro que simula un cuello de botella y otra que es una
  mini-ciudad.

+ Las redes de tráfico reciben parámetros vía python para configurarlas, en
  lugar de tener que manipualr directamente los archivos xml.

+ Los vehículos se pueden controlar usando la manera nativa de SUMO, o usando
  controladores y enrutadores incluidos en el framework, como el 
  *Intelligent Driver Model* o el *Continuos Router* (para mantener al tráfico
  dentro de la simulación y que no salga).

+ Tiene su propia manera de importar redes de tráfico de Open Street Maps.

+ Es posible usar las ventajas del framework sin necesidad de usar las partes de
  *RL*.

+ Guarda los resultados del experimento en un directorio en diversos tipos de
  archivos (algunos son csv) y posteriormente se pueden visualizar con
  *Tensorboard* para comparar resultados entre diversas ejecuciones en una misma
  gráfica.

### Posibles desventajas

+ No poder integrar la generación de tráfico ya desarrollada.

+ No poder integrar el control de semáforos usando redes de Petri.

+ No poder acoplar mi módulo de recolección de datos en una base de datos.

### Acercamiento

Terminaré la simulación de un día completo en una intersección sin Flow,
recopilaré datos necesarios, y posteriormente intentaré replicarlo en Flow para
ahora si saber a ciencia cierta si le puedo acoplar las partes de mi
arquitectura. De no ser así, no es el fin del mundo, lo que haré es hacerlo de
la manera que Flow dicta, y trataré de hallar métricas que pueda recopilar en
ambas maneras que me permitan hacer un versus justo entre ambos acercamientos.

