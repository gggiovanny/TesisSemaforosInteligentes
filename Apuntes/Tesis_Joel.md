---
bibliography: [D:/Files/Documents/source/Tesis/mendeley/tesis_tls_cimat.bib]
csl: D:/Files/Documents/source/Tesis/mendeley/apa.csl
---
# A usar en la introducción:
> La congestión de tráfico urbano causa considerables costos debido a pérdidas de
> tiempo, incrementa la posibilidad de accidentes, y los problemas de
> contaminación en las principales ciudades por lo que tiene un impacto negativo
> en el ambiente. También es responsable de problemas de salud tales como estrés,
> ruido y complicaciones similares. La solución de aumentar la dimensión de la red
> de tráfico urbano no siempre resulta ser la mejor opción además es muy difícil y
> muy costosa, especialmente en áreas urbanas.
[@Antonio2006a, Introducción]

> Una de las principales respuestas al problema del control de tráfico urbano es
> reducir el tiempo de espera de los usuarios en la red de tráfico. Se puede
> reducir el tiempo de espera de los usuarios en la red de tráfico por medio del
> cambio dinámico de las señales desplegadas en los semáforos, y que este cambio
> se realice de acuerdo a la demanda de tráfico y a la coordinación con
> intersecciones adyacentes.
[@Antonio2006a, Introducción]



# Que cosas me van a servir como estado del arte?

# Cosas que ocupé para la arquitectura
## Definiciones
+ Calle
+ Una intersección consiste de un conjunto de calles relacionadas entre sí y en el área de cruce. @Antonio2006a p.2
+ Un Semáforo es el elemento de la intersección responsable de desplegar las estrategias de control establecidas en la intersección. @Antonio2006a p. 3
+ El flujo se define para el caso de control de tráfico urbano, como el número de vehículos que pasan por algún punto @Antonio2006a p.3
+ La densidad se define como el número de vehículos en un tramo de la calle dividida entre la longitud de la calle @Antonio2006a p.4
+ Ciclo de semáforo
+ Estado: El estado del sistema de tráfico urbano índica la situación en la que se encuentra el sistema en un instante de tiempo dado @Antonio2006a p.3
+ Tráfico


La arquitectura de control tendrá:

### Entrada (propiedades):
- Calle: una extensión de terreno donde los vehiculos ocupan una sección de la misma. Puede tener:
  - Carriles
  - Cola de espera
  - Área aproximada
- Tipos de vehículos especiales y comportamientos especiales asociados.
- Estructura de la intersección:
  - Todas las posibles conexiones entre carriles
  - Derechos y prioridades de paso
  - Layout del semáforo: que luces tiene el semaforo, si tiene paso para peatones, etc.

### Salida (red de petri temporizada)
+ Parámetro de salida del controlador de tiempo.
  - Decisión de cambiar el tiempo asignado a las luces.{more decrease, decrease, no change, increase, more increase}
+ Parámetro de salida del controlador de fases.
  - Cambiar la decisión de la próxima fase. {No change, change}. Hemos

### Estado:
+ Datos recopilados en tiempo real y por ciclo: 
  - número de vehículos en las calles
  - número de accidentes
  - Vehiculos de prioridad (tipo y cantidad de cada uno)
+ Datos recopilados por únicamente por ciclo
  - flujo de los segmentos de calle
  - densidad de los segmentos de calle
+ Parámetros de entrada del controlador de tiempo (por ciclo). 
  - Cola más larga en la señal roja. 
  - Número de vehículos que pasan en señal verde. 
  - Tiempo ocioso de la señal verde 
+ Parámetros de entrada del controlador de fases.
  - Cola más larga en la señal roja. 
  - Cola más larga próxima fase.
  - Tiempo de cambio de la cola más larga en señal roja.

Parametros inspirados en trabajo visto en @Antonio2006a [pp 2-4, 22-23] 


# Cosas sueltas
incluir o eliminar vueltas a la izquierda, incrementar o disminuir el numero de calles que tienen derecho de paso simultáneamente, entre otras. @Antonio2006a pp. 38 y 39

# Referencias