# Tarea 1: TAREA: Crea una imagen, p.e. 800x800, con la textura del tablero de ajedrez

```python
#Crea una imagen con tres planos
color_img = np.zeros((800,800,3), dtype = np.uint8)
img_size = 800
square_size = img_size // 8
#Dimensiones
print(color_img.shape)
#Visualiza sin especificar el mapa de color gris
plt.imshow(color_img) 
plt.show()
```

# Tarea 2: TAREA: Crear una imagen estilo Mondrian (un ejemplo https://www3.gobiernodecanarias.org/medusa/ecoescuela/sa/2017/04/17/descubriendo-a-mondrian/ )

```python
mondrian = np.zeros((800, 800,3), dtype = np.uint8)
mondrian.fill(255)

#Vertical lines
mondrian[0:800, 60:80] = 0
mondrian[0:800, 150:170] = 0
mondrian[0:400, 320:340] = 0
mondrian[400:800, 410:430] = 0
mondrian[0:600, 550:570] = 0
mondrian[0:800, 700:720] = 0

#Horizontal lines
mondrian[160:180, 0:800] = 0
mondrian[390:410, 70:720] = 0
mondrian[590:610, 0:800] = 0

#Colors
mondrian[0:160, 80:150] = [255,255,0]
mondrian[0:160, 170:320] = [0,0,255]
mondrian[0:160, 340:550] = [255,0,0]
mondrian[0:160, 570:700] = [255,255,0]

mondrian[180:390, 340:550] = [0,0,255]
mondrian[180:390, 570:700] = [255,0,0]

mondrian[410:590, 170:410] = [255,0,0]
mondrian[410:590, 570:700] = [255,255,0]

mondrian[610:800, 80:150] = [0,0,255]
mondrian[610:800, 170:410] = [255,255,0]

mondrian[180:590, 720:800] = [255,0,0]

plt.imshow(mondrian)
plt.show()

```


# Tarea 3: TAREA: Resuelve una de las tareas previas (a elegir) con las funciones de dibujo de OpenCV  :)

```python
import numpy as np
import cv2
#Realizamos el ejercicio del tablero del ajedrez pero con funciones de OpenCV
width, height = 800, 800

image = np.zeros((height, width, 3), dtype=np.uint8)
square_size = width // 8

# Definir colores
white = (255, 255, 255)
black = (0, 0, 0)

# Dibujar el tablero de ajedrez
for i in range(8):
    for j in range(8):
        if (i + j) % 2 == 0:
            color = white
        else:
            color = black
        x1, y1 = j * square_size, i * square_size
        x2, y2 = (j + 1) * square_size, (i + 1) * square_size
        cv2.rectangle(image, (x1, y1), (x2, y2), color, -1)

# Mostrar la imagen
cv2.imshow("Tablero de Ajedrez", image)
cv2.waitKey(0)
```

# Tarea 4: TAREA: Modifica de alguna forma los valores de un plano de la imagen
```python
import cv2

# Inicializa la captura de video desde la cámara web (0 indica la cámara predeterminada)
vid = cv2.VideoCapture(0)

while(True):
    # Captura un fotograma de la cámara
    ret, frame = vid.read()
    print(frame)
    if ret:
        # Modifica el plano de color verde (canal G) de la imagen
        frame[:, :, 2] = 0  # Establece todos los valores del canal G a 0

        # Muestra la imagen modificada en pantalla
        cv2.imshow('Vid', frame)

    # Detén el bucle al presionar la tecla Esc (código 27)
    if cv2.waitKey(20) == 27:
        break

# Libera la cámara
vid.release()

# Cierra todas las ventanas de visualización
cv2.destroyAllWindows()
```

# Tarea 5: TAREA: Pintar círculos en las posiciones del píxel más claro y oscuro de la imagen  ¿Si quisieras hacerlo sobre la zona 8x8 más clara/oscura?
```python
vid = cv2.VideoCapture(0)

while(True):
    # Lee un fotograma de la cámara
    ret, frame = vid.read()

    # Convierte el fotograma a escala de grises para encontrar los valores máximos y mínimos 
    color_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    # Encuentra el valor mínimo y máximo de intensidad de píxeles en el fotograma
    min_intensity = np.min(color_frame)
    max_intensity = np.max(color_frame)

    # Encuentra las coordenadas de los píxeles más oscuros y más brillantesç
    print(min_intensity)
    dark_pixel_coords = np.argwhere(color_frame == min_intensity)
    bright_pixel_coords = np.argwhere(color_frame == max_intensity)

    # Dibuja círculos rojos en los píxeles más oscuros y verdes en los más brillantes
    for coord in dark_pixel_coords:
        cv2.circle(frame, (coord[1], coord[0]), 10, (0, 0, 255), -1)
    for coord2 in bright_pixel_coords:
        cv2.circle(frame, (coord2[1], coord2[0]), 10, (0, 255, 0), -1)

    # Muestra el fotograma con círculos en píxeles oscuros y brillantes
    cv2.imshow('Pixeles mas oscuros', frame)

    # Sale del bucle si se presiona la tecla Esc (código 27)
    if cv2.waitKey(20) == 27:
        break

# Libera la cámara
vid.release()

# Cierra todas las ventanas de visualización
cv2.destroyAllWindows()
```

# Tarea 6: TAREA: Haz tu propuesta pop art

```python
import random
import cv2
import numpy as np

vid = cv2.VideoCapture(0)

ncells = 10

while True:
    # Fotograma a fotograma
    ret, frame = vid.read()

    if ret:
        # Dimensiones originales
        h, w, c = frame.shape
        # Redimensiono
        down_frame = cv2.resize(frame, (int(w/ncells), int(h/ncells)))
        # Dimensiones reducidas
        h2, w2, c2 = down_frame.shape

        # Separamos canales
        r = down_frame[:, :, 0]
        g = down_frame[:, :, 1]
        b = down_frame[:, :, 2]

        # Creamos imagen negra
        triangle_up_frame = np.zeros((h2*ncells, w2*ncells, 3), dtype=np.uint8)

        for y in range(0, h2):
            for x in range(0, w2):
                # La suma de los valores RGB define el tamaño del triángulo
                size = int((r[y, x] + g[y, x] + b[y, x]) / (ncells*3*2))
                color = r[y, x] + g[y, x] + b[y, x]
                # Puntos del triángulo
                #x*ncells para obtener la posición en píxeles en la imagen original correspondiente al resize
                
                p1 = (x*ncells, y*ncells - size)
                p2 = (x*ncells - size, y*ncells + size)
                p3 = (x*ncells + size, y*ncells + size)

                # Dibuja el triángulo
                cv2.drawContours(triangle_up_frame, [np.array([p1, p2, p3])], 0, (random.randint(0, 255), 255, 255), -1)

        # Muestra fotograma resultante
        cv2.imshow('Cam', triangle_up_frame)

    # Detenemos pulsado ESC
    if cv2.waitKey(20) == 27:
        break

# Libera el objeto de captura
vid.release()
# Destruye ventanas
cv2.destroyAllWindows()

``` 
