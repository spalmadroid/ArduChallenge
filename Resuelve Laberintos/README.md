# Resuelve Laberintos

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Arduino Guatemala](https://img.shields.io/badge/Arduino-Guatemala-blue.svg)](https://www.facebook.com/ArduinoGuatemala)

## Descripción

El Resuelve Laberinos es un robot autónomo, el cual usando un microcontrolador y varios sensores, puede llegar a resolver un laberinto.

Usualmente los Robot Resuelve Laberintos, trabajan de 2 maneras: 

* Por medio de sensores infrarrojos (su trabajo es detectar una línea Blanca o Negra en el Suelo y guiar su recorrido)  
* Por medio de sensores ultrasónicos (estos detectan obstáculos durante un trayecto, permitiendo reconocer hacia donde deben guiar su     recorrido)

![Resuelve Laberintos](https://github.com/spalmadroid/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/robot.png)

## Materiales

-	Placa Arduino Uno R3
-	Batería de 9V alcalina
-	Borner para batería de 9V
-	2 Micro motores DC de 5V con llanta
-	Shield con protoboard para Arduino Uno
-	Regulador de voltaje 5V IC 7805
-	Circuito integrado L293D
-	Cables para protoboard tipo DUPOND (Hembra/Macho)
-	Cables para protoboard pequeños
-	Tornillos milimétricos de 3mm con tuerca
-	Diodo Led para indicador de estado
-	Módulo QTR 8A POLOLU.
-	Chasis a medida impreso en 3D (o construir el propio con cualquier material)

**NOTA:**
Los materiales utilizados en este tutorial pueden variar. Cualquier cambio realizado, tanto en tamaño, motores utilizados, tipos de placas o materiales a utilizar, pueden modificar el código proporcionado.

## Funcionamiento

### ¿Cuáles son los pasos que debe ejecutar el Robot Resuelve Laberintos?

Básicamente existen 2: 
* El primero es realizar un recorrido por el laberinto y encontrar la meta. 
* El segundo es optimizar el camino, para que el robot pueda realizar el recorrido más corto del laberinto sin equivocarse.

***

### ¿Cómo es que el Robot Resuelve Laberintos encuentra la meta del laberinto?

Se utiliza la técnica llamada **"Left Hand on the Wall"** (Mano izquierda en el muro). Imaginemos que nosotros estamos dentro de un laberinto y que mantenemos nuestra mano siempre tocando la pared izquierda todo el tiempo. Haciendo esto, nosotros vamos a lograr recorrerlo sin necesidad de repetir un camino fallido y convertir la salida del laberinto en un ciclo interminable, ni regresando a donde empezamos.

 Este es el Algoritmo utilizando la técnica **Left Hand on the Wall**, simplificado en simples condiciones:

-	Si se puede girar hacia la izquierda, entonces se gira hacia la izquierda.
-	Si se puede avanzar hacia adelante, entonces se avanza hacia adelante.
-	Si no se puede girar hacia la izquierda, pero se puede girar hacia la derecha, entonces se gira hacia la derecha.
-	Si no se puede girar hacia la izquierda, y tampoco se puede girar hacia la derecha, significa que se ha llegado a un camino muerto, por lo tanto, se debe dar un giro de 180° y se sigue intentando.

El robot debe realizar esas decisiones dentro del algoritmo, cuando se encuentra con una intersección. 
Una intersección dentro del laberinto, es cualquier punto en donde tenga la oportunidad de girar. Si el robot tiene la oportunidad de cruzar, pero este sigue avanzando, esto se considera como que siguió hacia adelante, cada movimiento en una intersección, o cuando gira, será almacenado en la memoria del ROBOT (EEPROM del ARDUINO).

-	L = Girar hacia la izquierda (Turn Left).
-	R = Girar hacia la derecha (Turn Right).
-	S = Avanzar hacia delante (Going Straight).
-	B = Realizar un Giro de 180° (Turning Around).

Ahora se aplicará lo aprendido hasta el momento en un simple laberinto, así se podrá observar el comportamiento de nuestro robot, con las siguiente imágenes, aplicando el algoritmo **Left Hand on the Wall**. El círculo rojo seria el robot:

![Laberinto](https://github.com/spalmadroid/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/laberinto.png)
![Resolución Laberinto](https://github.com/spalmadroid/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/resolucion%20laberinto.png)

***

## Licencia

ArduChallenge es un proyecto de código abierto con [Licencia MIT](https://opensource.org/licenses/MIT).
