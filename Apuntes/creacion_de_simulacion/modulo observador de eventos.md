Ahora voy a proceder a controlar la simulación previamente construida usando una interfaz para controlar SUMO
llamada TraCI.
TraCI viene incluída en la instalación por defecto de SUMO.
Previamente ya la había utilizado y ya tengo mi manera de consumirlo, y me guié de la [guía oficial](https://sumo.dlr.de/docs/TraCI/Interfacing_TraCI_from_Python.html).

Se modelaron las propiedades de una intersección en clases que se relacionan entre si.

La más básica es Edge, que es lo equivalente a una calle y contiene sus propiedades asociadas:

+ is_traffic_input: indica si el trafico entra por esta calle.
+ associated_detector_name: nombre del detector de trafico asociado a esta calle.
+ num_lanes: numero de carriles.
+ aprox_length: largo aproximado de la calle.
+ aprox_total_width: ancho aproximado de la calle completa que incluye a todos los carriles.
+ aprox_area: aprox_total_width # area calculada.
+ from_direction: desde que direccion viene el la calle.
+ to_direction: a que direccion va la calle.

Las calles están conectadas entre si, y esta relación se representa a través de la clase Conection, que indica una conexión simple entre Edges (calles):

+ from_edge: desde que calle viene el trafico.
+ to_edge: hacia que calle viene el trafico.
+ validate: si se deben validar que los parámetros recibidos representen una conexión válida. Por defecto es False, para evitar realizar esta operación cuando no sea necesario para ahorrar procesamiento

Las calles se agrupan en intersecciones que tienen conexiones entre ellas y posiblemente un semáforo. Esto se representa en la clase Intersection:

+ edges_list: lista de todas las calles en la intersección. Útil si se quiere recorrer todas una por una.
+ conections_list: lista de todas las conexiones entre calles.
+ associated_traffic_light_name: el nombre del semáforo asociado a la interseccion.

Ahora lo que sigue es modelar el EventObserver

# Notas
+ Tomar inspiracion de los docs de [Edges value retrieval](https://sumo.dlr.de/docs/TraCI/Edge_Value_Retrieval.html#extended_retrieval_messages) para los calculos derivados de parametros para los reportes
  + Tambien de vehiculos
+ 