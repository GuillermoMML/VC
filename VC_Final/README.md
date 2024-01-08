Hemos implementado un detector de vehículos y un sistema de seguimiento de velocidad.

El código se divide en dos partes principales: la primera parte contiene la clase `Tracker`, que realiza el seguimiento de objetos en un video, y la segunda parte incluye la interfaz de usuario utilizando la biblioteca `tkinter` y la detección de vehículos basada en la clasificación de fondo.

# Motivación/Argumento del Trabajo

En relación con la motivación para realizar este trabajo, lo elegimos porque nos parecía divertido explorar la capacidad de grabar los coches en una determinada autopista y averiguar a qué velocidad aproximadamente se desplazan.

# Propuesta de Ampliación

Esto podría extrapolarse a otros tipos de vehículos, como los ciclomotores e incluso vehículos voladores, con ciertos ajustes, lo que le otorga una gran potencialidad. Esta iniciativa podría ser de utilidad en campos como el de la policía, mejorando así su desempeño laboral.

### Parte 1: Tracker

El propósito principal de `Tracker` es realizar un seguimiento de los objetos en un video mediante el almacenamiento de sus posiciones centrales y asignando identificadores únicos a cada objeto detectado donde se hace uso de los siguientes atributos:

- `center_points`: un diccionario que almacena las posiciones centrales de los objetos con sus respectivos identificadores.
- `id_count`: un contador para asignar identificadores únicos a los nuevos objetos detectados.

Y los siguientes métodos:

- `__init__(self)`: inicializa la clase con el diccionario vacío de puntos centrales y el contador de identificadores en cero.
- `update(self, objects_rect)`: actualiza la información de seguimiento con las nuevas posiciones de los objetos detectados y devuelve una lista de cajas delimitadoras con identificadores únicos.

### Parte 2: Interfaz Gráfica y Detección de Velocidades

En la segunda parte, tratamos la Interfaz Gráfica y Detección de Velocidades, combinando ambas utilizando la clase `Tracker` y haciendo uso de los siguientes atributos:

- `max_speed`: la velocidad máxima permitida.
- `current_id`: el identificador del coche actual en la zona 3.
- `total_cars`: el total de coches con velocidad superior a la máxima permitida.
- `carI`, `carO`: los diccionarios para almacenar información sobre la entrada y salida de vehículos.

Los métodos principales son los siguientes:

- `__init__(self, video_source, total_cars, default_max_speed)`: inicializa la aplicación con la fuente de video, el número total de coches y la velocidad máxima permitida.
- `update(self)`: actualiza continuamente la interfaz gráfica con los fotogramas del video procesados.
- `validar_entero(self)`: valida y establece la nueva velocidad máxima introducida por el usuario.
- `process_frame(self, frame)`: realiza el procesamiento del fotograma, incluyendo la detección de vehículos y el seguimiento de velocidad.
- `convert_frame_to_photo(self, frame)`: convierte el fotograma procesado a un formato compatible con la interfaz gráfica.

#### Funcionamiento:

Se hace uso de la clasificación de fondo (`cv2.createBackgroundSubtractorMOG2`) para detectar vehículos en el video, se define una región de interés (ROI) mediante polígonos para separar áreas específicas, se aplican técnicas de procesamiento de imágenes, como umbralización y dilatación, para mejorar la detección, haciendo uso de la clase `Tracker` para seguir y asignar identificadores a los vehículos detectados y se controla la velocidad de los vehículos en una región específica y se actualiza el recuento total de coches que superan la velocidad máxima permitida.
