---
title: Diseño e implementación de arquitectura para control inteligente de tráfico urbano
bibliography: [D:/Files/Documents/source/Tesis/mendeley/tesis_tls_cimat.bib]
csl: D:/Files/Documents/source/Tesis/apa7-spanish.csl
lang: es
geometry: margin=20mm
---

**Alumno:** Br. Giovanny González Baltazar

**Asesor interno:** Dr. Mauricio Gabriel Orozco del Castillo

**Asesor externo:** Dr. Joel Antonio Trejo Sánchez




<!-- todo: ### Notas
+ Tomar inspiracion de los docs de [Edges value retrieval](https://sumo.dlr.de/docs/TraCI/Edge_Value_Retrieval.html#extended_retrieval_messages) para los calculos derivados de parametros para los reportes
  + Tambien de vehiculos -->



<!-- 
# Definiciones
+ Calle
+ Una intersección consiste de un conjunto de calles relacionadas entre sí y en el área de cruce. \textcite{JoelTrejo2006}JoelTrejo2006 p.2
+ Un Semáforo es el elemento de la intersección responsable de desplegar las estrategias de control establecidas en la intersección. \textcite{JoelTrejo2006}JoelTrejo2006 p. 3
+ El flujo se define para el caso de control de tráfico urbano, como el número de vehículos que pasan por algún punto \textcite{JoelTrejo2006}JoelTrejo2006 p.3
+ La densidad se define como el número de vehículos en un tramo de la calle dividida entre la longitud de la calle \textcite{JoelTrejo2006}JoelTrejo2006 p.4
+ Ciclo de semáforo
+ Estado: El estado del sistema de tráfico urbano índica la situación en la que se encuentra el sistema en un instante de tiempo dado \textcite{JoelTrejo2006}JoelTrejo2006 p.3
+ Tráfico

+ TraCI:
+ Edge(arista/calle): A single-directed street connection between two points (junctions/nodes). An edge contains at least one lane.
+ Lane (carril):
Sacar definiciones de [aqui](https://sumo.dlr.de/docs/Other/Glossary.html). Investigar como citarlo. -->

# Referencias