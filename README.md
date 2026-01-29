# Grafos-y-lenguajes-formales-

import numpy as np import matplotlib.pyplot as plt import matplotlib.animation as animation

------------------------------
Parámetro de tamaño definido por el usuario
try: tamaño = int(input("Ingrese el tamaño del autómata (ej: 50): ")) if tamaño <= 0: raise ValueError except ValueError: print("Valor inválido, se usará tamaño 50 por defecto.") tamaño = 50

------------------------------
Patrones iniciales
modos = { "aleatorio": lambda: np.random.choice([0, 1, 2, 3], size=(tamaño, tamaño)), "rayas": lambda: np.array([[i % 4 for i in range(tamaño)] for _ in range(tamaño)]), "bloques": lambda: np.tile([[0, 1], [2, 3]], (tamaño // 2, tamaño // 2)) }

------------------------------
Paletas de color
paletas = [ {0: (1, 1, 1), 1: (1, 0.5, 0), 2: (0, 1, 0), 3: (0, 0, 1)}, # Blanco, naranja, verde, azul {0: (0.8, 0.1, 0.1), 1: (0.9, 0.7, 0.2), 2: (0.2, 0.9, 0.7), 3: (0.6, 0.3, 0.9)} # Alternativa ]

------------------------------
Estado inicial
bosque = modos"aleatorio" paleta_actual = 0

------------------------------
Función de actualización por frame
def actualizar(frame): global bosque nueva = bosque.copy() for i in range(tamaño): for j in range(tamaño): vecinos = [ bosque[(i-1)%tamaño, j], bosque[(i+1)%tamaño, j], bosque[i, (j-1)%tamaño], bosque[i, (j+1)%tamaño] ] nueva[i, j] = np.random.choice(vecinos) bosque = nueva img.set_data(np.array([[paletas[paleta_actual][val] for val in row] for row in bosque])) return [img]

------------------------------
Cambiar patrón inicial
def cambiar_patron(patron): global bosque bosque = modospatron print(f"Patrón cambiado a: {patron}")

------------------------------
Cambiar paleta de colores
def cambiar_paleta(): global paleta_actual paleta_actual = (paleta_actual + 1) % len(paletas) print(f"Cambiando paleta a {paleta_actual}")

------------------------------
Crear interfaz con matplotlib
fig, ax = plt.subplots()

Imagen inicial
img = ax.imshow( np.array([[paletas[paleta_actual][val] for val in row] for row in bosque]), interpolation='nearest' )

Título superior de la figura
fig.suptitle("Autómata Celular Estético", fontsize=14, y=0.98)

Subtítulo con instrucciones
ax.set_title("Teclas: [1] aleatorio, [2] rayas, [3] bloques – 'r' cambiar paleta", fontsize=9)

Ocultar ejes
ax.axis('off')

------------------------------
Teclas de interacción
def presionar_tecla(event): if event.key == 'r': cambiar_paleta() elif event.key == '1': cambiar_patron("aleatorio") elif event.key == '2': cambiar_patron("rayas") elif event.key == '3': cambiar_patron("bloques")

Conectar evento
fig.canvas.mpl_connect('key_press_event', presionar_tecla)

------------------------------
Animación
ani = animation.FuncAnimation(fig, actualizar, frames=1000, interval=200, blit=True) plt.show()
