---
title: Descripción de la arquitectura
geometry: margin=20mm
---

![](Propuesta%20de%20arquitectura.png)

# Tráfico
Los vehículos que se mueven en los carriles de la intersección y cruzan de un carril a otro en nodos llamados intersecciones.  Sobre las intersecciónes puede haber ubicados semáforos.

# Propiedades de la intersección
Son las propiedades intrínsecas de la intersección que no suelen cambiar con el paso del tiempo:
+ Número de calles.
+ Carriles en cada calle.
+ Extensión aproximada de cada carril.
+ Estructura de la intersección: todas las posibles conexiones entre carriles.
+ Derechos y prioridades de paso.
+ Disposición de las luces del semáforo (cuántas hay y a que intersección apuntan).
+ Existencia de cruces y puentes peatonales, y si tienen algún botón que permita detener el tráfico.

# Observador de eventos
Su única tarea es ser el sentido de la vista del agente de control. 
Tiene funciones para observar eventos de interés que suceden con regularidad en el mundo real en un momento dado y sus propiedades, tales como:

+ El paso de los vehículos, con propiedades como:
  + Ruta que toma (pueden seguir derecho o girar en algún sentido).
  + Velocidad promedio aproximada.
  + Cuanto tiempo permanece detenido.
  + Aceleración aproximada.
+ Cambios de luces (en que carril y  a que color cambió).

Y también notifica eventos emergentes y sus propiedades, como:

+ Aparición de un vehículos de prioridad (de que tipo y en que carril).
+ Un accidente (que carril obstruye).

Todo lo observado lo comunica  al *Registrador de eventos* indicando el tiempo exacto en el que sucedió (*timestamp*).

# Registrador de eventos
Es el encargado de guardar un caché de los datos crudos enviados por el *Observador de eventos* e interpretarlos, para cada hora generar reportes con datos derivados que se almacenan en la base de datos. 
El reporte generado agrupa los datos por ciclo de semáforo, siendo éstos los siguientes:

- Flujo por carril, es decir, el número de vehículos que pasan por cada carril cada determinado tiempo.
- Tiempo de espera promedio por carril.
- Densidad de cada carril.
- Cola más larga en la señal roja. 
- Número de vehículos que pasan en señal verde. 
- Tiempo ocioso de la señal verde.
- Cola más larga próxima fase.
- Tiempo de cambio de la cola más larga en señal roja.

También recibe datos al exterior a través de la *Interfaz de comunicación*, que se almacenan selectivamente en la base de datos.

# Interfaz de comunicación
Permite enviar y recibir reportes de otros agentes de control, así como notificaciones de eventos externos que pueden alterar el tráfico, como:

- Alertas de desastre natural.
- Eventos públicos y días festivos.
- Manifestaciones.

# Comparador
Se encarga de comparar constantemente el comportamiento de los eventos registrados con el comportamiento predicho. Cuando estos difieren de manera significativa, el comparador solicita al *Modelo predictivo* que genere una nueva predicción. También se encarga de solicitar nuevas predicciones cada que un evento externo lo solicite.

# Modelo predictivo
Genera una nueva predicción cada cierto tiempo fijo (cada hora de manera escalonada, usando una cola de espera).
Se alimenta con el histórico y con el flujo actual, asi como con la hora, dia, el calendario con días festivos y notificaciones externas de eventos.
Predice que cantidad de trafico relativo habrá en determinada hora (a definir si será de manera numérica o con expresiones relativas de lógica difusa, como *poco*, *mucho* o *normal*).

# Modelo de priorizacion
Le da un valor numérico a cada ruta según la demanda predicha y se encarga de establecer tiempos para cada uno.

# Constructor de redes de Petri
A partir de los tiempos generados por el motor de priorización, construye una propuesta de red de Petri temporizada.

# Optimizador
A través de metaheurísticas (como algoritmos genéticos), optimiza el comportamiento de la red de Petri para minimizar los tiempos de espera y maximizar el flujo.

# Controlador
Tiene las funciones para necesarias para leer la red de Petri que recibe y realizar los cambios de luces.según lo indique la red.

# Luces de semáforo
Indica como se deben alternar los flujos de trafico y son lo que físicamente ven los vehículos.
