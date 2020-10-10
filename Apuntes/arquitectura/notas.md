+ Registrar de donde vienen los vehículos y hacia donde van.
+ A partir de eso suponer flujos armando un mapa de calor.
+ Optimizar tiempos para los flujos mas usados
+ Ver si de manera indirecta se puede distribuir el tráfico.
+ Asignarle un número de prioridad a cada flujo y alterlos para simular como lo hace el CPU.

# Modelo predictivo
+ Se debe llevar un historico del flujo de tráfico
+ Se alimenta con el historico y con el flujo actual, asi como con la hora, dia, el calendario con dias festivos y notificaciones push de eventos (como manifestaciones o cosas asi).
+ Predice en X hora que cantidad de trafico relativo habrá (poco, mucho, normal). Tambien guarda la cantidad numérica.
+ Dependiendo la cantidad de trafico esperado es que se le va a asignar la prioridad.
+ Esto alimenta al motor de paralelizacion de trafico.

# Redes de petri
+ Hay dos tipos:
  + Las que modelan el sistema (no se usaran)
  + Las que temporizan el trafico.