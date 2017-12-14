# Resuelve Laberintos

![Portada Resuelve Laberintos](https://github.com/arduinoguate/ArduChallenge/blob/master/Publicidad/PortadaEventoResuelveLaberinto_Facebook.png)

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Arduino Guatemala](https://img.shields.io/badge/Arduino-Guatemala-blue.svg)](https://www.facebook.com/ArduinoGuatemala)

## Descripción

El Resuelve Laberinos es un robot autónomo, el cual usando un microcontrolador y varios sensores, puede llegar a resolver un laberinto.

Usualmente los Robot Resuelve Laberintos, trabajan de 2 maneras: 

* Por medio de sensores infrarrojos (su trabajo es detectar una línea Blanca o Negra en el Suelo y guiar su recorrido)  
* Por medio de sensores ultrasónicos (estos detectan obstáculos durante un trayecto, permitiendo reconocer hacia donde deben guiar su     recorrido)

![Resuelve Laberintos](https://github.com/arduinoguate/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/robot.png)

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

![Laberinto](https://github.com/arduinoguate/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/laberinto.png)
![Resolución Laberinto](https://github.com/arduinoguate/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/resolucion%20laberinto.png)

Como se puede observar dentro del laberinto, vemos el recorrido que el robot va a realizar, tomando en cuenta siempre el algoritmo **Left Hand on the Wall**. El reconocimiento de la trayectoria se almacena en un Array, el cual reconoce las letras LRBS como instrucciones que debe realizar. En este caso se tiene lo siguiente: (de izquierda a derecha y hacia abajo)

**Recorrido = LBLLBSR**

***

### Analizando la Optimización del Recorrido

Como se pudo observar en la primera parte, el recorrido queda guardado de la siguiente manera **LBLLBSR**, pero ¿cómo hace el robot para acortar el camino y resolverlo de una manera directa?, veamos a continuación la siguiente serie de imágenes de lo que debe hacer el robot para acortar el recorrido:

![Optimización Laberinto](https://github.com/arduinoguate/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/optimizacion%20laberinto.png)

Observando las imágenes anteriores nos damos cuenta que tiene una trayectoria reducida, esto lo hace de la siguiente manera:

**Recorrido Final = SRR**

Pero, como lo hace bien, necesitamos que nuestro camino LBLLBSR lo convierta a un recorrido final SRR. Para comenzar, debemos observar cuales fueron los fallos de nuestro robot. 

Una “B” indica que el robot realiza un giro de 180°, lo cual significa que allí estuvo mal. Para Optimizarlo, necesitamos sustituir la “B” por un camino correcto, que veremos a continuación.

***

### Optimizando el Recorrido

![Optimizando Laberinto](https://github.com/arduinoguate/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/optimizando%20laberinto.png)

Veamos los primeros 3 movimientos del recorrido LBLLBSR, estos movimientos son LBL, como se observa en la imágen anterior. En vez de que el robot gire hacia la izquierda, de un giro de 180° y vuelva a girar hacia la izquierda, necesitamos que vaya hacia adelante. Así que podemos decir que LBL = S.
Esto es lo que el robot utiliza para optimizar el recorrido. 

A continuación una serie de ejemplos, pero no son todos:

-	LBR = B
-	LBS = R
-	RBL = B
-	SBL = R
-	SBS = B
-	LBL = S

Estas optimizaciones no deben ser programadas, en el primer recorrido del robot en el laberinto. Pero si es necesario que el código los utilice cuando se está recortando el recorrido.

A continuación se optimizará el recorrido anterior con todo lo que ya sabemos:

-	Recorrido = LBLLBSR
-	LBL = S, **optimizado** lo que significa que el nuevo recorrido debería de ser: SLBSR
-	Como ya sabemos LBS se puede reducir a R, por lo que la siguiente optimización quedaría SRR
-	Como se puede observar, ya encontramos el recorrido optimizado que necesitábamos.

El robot optimiza el recorrido durante el recorrido, el recorrido es almacenado en un Array y en todo momento va guardando nuevos movimientos. 

Siempre hace la comprobación de que el movimiento previo pudo haber sido una B, si fuera así, entonces optimiza automáticamente el recorrido. 

Se necesita saber al menos 3 movimientos para que el algoritmo funcione y optimice el recorrido: El movimiento ANTES Y DESPUES de un GIRO DE 180°(obviamente contando el giro de 180° como uno de los 3 movimientos).

***

### Ejemplo de Resolución de un Laberinto

![Ejemplo Laberinto](https://github.com/arduinoguate/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/ejemplo%20laberinto.png
)

Analizaremos otro Ejemplo:

- Usando siempre el algoritmo **Left Hand on the Wall**, este es el camino que debería de tomar
LLLBLLLRBLLBSRSRS

- Utilizando el proceso de optimización del recorrido:
LL(LBL =S)LL(RBL= B)(LBS = R)RSRS

- El nuevo recorrido quedaría: 
LLSLLBRRSRS

- Volvemos a acortar el recorrido hasta que todas las B sean eliminadas:
LLSL(LBR = B) RSRS

- Continuamos acortando:
LLS(LBR= B)SRS

- El nuevo recorrido quedaría:
LLSBSRS

- Continuamos acortando:
LL(SBS = B)RS

- El nuevo recorrido quedaría:
LLBRS

- Continuamos acortando:
L(LBR = B)S

- El nuevo recorrido quedaría:
LBS

- Y acortando el recorrido final quedaría:
LBS = R

Por lo que para resolverlo, el robot únicamente tiene que girar a la Derecha para llegar a la meta. **R**

## Diseño Estructura

![Diagrama Diseño](https://github.com/arduinoguate/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/diagrama%20dise%C3%B1o.png)
![Diseño Final](https://github.com/arduinoguate/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/dise%C3%B1o%20final.png)

## Motores

![Motores Vista Perfil](https://github.com/arduinoguate/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/motores%20perfil.png)
![Motores Vista Superior](https://github.com/arduinoguate/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/motores%20superior.png)

## Sensores

Utilizamos el Sensor QTR 8A POLOLU. Recortándolo a solo 6 sensores, como se muestra en las imágenes.

![Sensor Vista](https://github.com/arduinoguate/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/sensor%20vista.png)
![Sensor Pines](https://github.com/arduinoguate/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/pines%20sensor.png)

## Montando Componentes al Case

Debe de tener un aproximado de 5mm de altura del suelo

![Montaje 1](https://github.com/arduinoguate/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/montaje1.png)
![Montaje 2](https://github.com/arduinoguate/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/montaje2.png)

## Circuito

![Circuito](https://github.com/arduinoguate/ArduChallenge/blob/master/Resuelve%20Laberintos/Multimedia/circuito.png)

## Código

Como es costumbre de todo programador, la mayoría de palabras e instrucciones, están en inglés, pero de igual manera es totalmente entendible, cualquier duda del mismo, preguntar en la Comunidad Arduino Jutiapa.

```arduino

#define leftCenterSensor   3      // Definimos el Pin para el Sensor Central Izquierdo conectado al pin Analogico 3
#define leftNearSensor     4      // Definimos el Pin para el Sensor Cercano Izquierdo conectado al pin Analogico 4
#define leftFarSensor      5      // Definimos el Pin para el Sensor Lejano Izquierdo conectado al pin Analogico 5
#define rightCenterSensor  2      // Definimos el Pin para el Sensor Central Derecho conectado al pin Analogico 2
#define rightNearSensor    1      // Definimos el Pin para el Sensor Cercano Derecho conectado al pin Analogico 1
#define rightFarSensor     0      // Definimos el Pin para el Sensor Lejano Derecho conectado al pin Analogico 0

int leftCenterReading;            // Se declara una variable para utilizar en la lectura del Sensor Central Izquierdo
int leftNearReading;              // Se declara una variable para utilizar en la lectura del Sensor Cercano Izquierdo
int leftFarReading;               // Se declara una variable para utilizar en la lectura del Sensor Lejano Izquierdo
int rightCenterReading;           // Se declara una variable para utilizar en la lectura del Sensor Central Izquierdo
int rightNearReading;             // Se declara una variable para utilizar en la lectura del Sensor Cercano Derecho
int rightFarReading;              // Se declara una variable para utilizar en la lectura del Sensor Lejano Derecho

int replaystage;                  // Guarda el estado en Memoria, para Repetir la Ruta mas Corta la cual reconocera en el primer recorrido

#define leapTime 230              // Tiempo de espera para pensar y para tomar decisiones

#define leftMotor1  9             // Se define un Pin Digital PWM 9 para el Motor Izquierdo 1
#define leftMotor2  10            // Se define un Pin Digital PWM 10 para el Motor Izquierdo 2

#define rightMotor1 5              // Se define un Pin Digital PWM 5 para el Motor Derecho 1
#define rightMotor2 6              // Se define un Pin Digital PWM 9 para el Motor Derecho 2

#define led 13                    // Led Indicador encendido y de cuando termina una carrera

char path[30] = {};               // Se crea una variable de Tipo CHAR para reconocer los patrones de recorrido (los cuales son LETRAS) y que despues sera comparado para buscar la ruta mas corta
int pathLength;                   // Se crea una Variable para almacenar todo el recorrido
int readLength;                   // Se crea uan Variable para Leer el recorrido almacenado en la variable anterior

void setup() {

  // Aqui Iniciamos nuestro codigo, declarando los pines como de entrada y de salida tanto del sensor QTR como de los motores

  pinMode(leftCenterSensor, INPUT);
  pinMode(leftNearSensor, INPUT);
  pinMode(leftFarSensor, INPUT);
  pinMode(rightCenterSensor, INPUT);
  pinMode(rightNearSensor, INPUT);
  pinMode(rightFarSensor, INPUT);

  pinMode(leftMotor1, OUTPUT);
  pinMode(leftMotor2, OUTPUT);
  pinMode(rightMotor1, OUTPUT);
  pinMode(rightMotor2, OUTPUT);
  pinMode(led, OUTPUT);
  //Serial.begin(115200);
  digitalWrite(led, HIGH);
  delay(1000);
}

void loop() {

  // Iniciamos nuestro codigo loop el cual es bastante corto, Inicia ejecutando la funcion de los sensores para tomar lectura todo el tiempo del estado de los sensores

  readSensors();

  // Luego comienza con la primera sentencia, donde dice lo siguiente, Si los sensores lejanos izquierdo y derecho en lectura son menores a 200 y los sensores centrales izquierdo y derecho en lecturo son mayores a 200
  // comprobando que cualquiera de los 2 sean verdaderos, entonces el robot ejecutara la funcion creada mas abajo llamada Straight(), lo cual significa Avanzar hacia Adelante

  if (leftFarReading < 200 && rightFarReading < 200 &&
      (leftCenterReading > 200 || rightCenterReading > 200) ) {
    straight();
  }
  else {                                                    // Si en el recorrido encuentra una lectura de los sensores diferente, entonces ejecutara la funcion  leftHandWall creada mas abajo la cual sirve para decidir hacia donde girar y guardar estados
    leftHandWall();
  }

}

void readSensors() {                                    // Funcion creada para que se mantenga siempre leyendo datos en los sensores

  leftCenterReading  = analogRead(leftCenterSensor);
  leftNearReading    = analogRead(leftNearSensor);
  leftFarReading     = analogRead(leftFarSensor);
  rightCenterReading = analogRead(rightCenterSensor);
  rightNearReading   = analogRead(rightNearSensor);
  rightFarReading    = analogRead(rightFarSensor);

}

void leftHandWall() {                                 // Funcion creada para tomar deciciones

  if ( leftFarReading > 200 && rightFarReading > 200) { // Si la lectura de los sensores lejanos Izquierdo y Derecho son mayores a 200, entoces que avance un pequeño tiempo hacia adelante y que vuelva a leer
    digitalWrite(leftMotor1, HIGH);
    digitalWrite(leftMotor2, LOW);
    digitalWrite(rightMotor1, HIGH);
    digitalWrite(rightMotor2, LOW);
    delay(leapTime);
    readSensors();

    if (leftFarReading > 200 || rightFarReading > 200) { //Si la lectura sigue siendo 200 en los sensores lejanos izquirdo y derecho entonces que ejecute la funcion DONE la cual significa que a terminado
      done();
    }
    if (leftFarReading < 200 && rightFarReading < 200) { // Si detecta una lectura menos a 200 en los sensores izquierdo y derecho entonces que gire a la izquierda
      turnLeft();
    }
  }

  if (leftFarReading > 200) {                 // Si puedes girar a la izquierda, entonces que gire a la izquierda
    digitalWrite(leftMotor1, HIGH);
    digitalWrite(leftMotor2, LOW);
    digitalWrite(rightMotor1, HIGH);
    digitalWrite(rightMotor2, LOW);
    delay(leapTime);
    readSensors();

    if (leftFarReading < 200 && rightFarReading < 200) {
      turnLeft();
    }
    else {
      done();
    }
  }

  if (rightFarReading > 200) {            // Si no puede girar a la izquierda entonces que gire a la derecha
    digitalWrite(leftMotor1, HIGH);
    digitalWrite(leftMotor2, LOW);
    digitalWrite(rightMotor1, HIGH);
    digitalWrite(rightMotor2, LOW);
    delay(30);
    readSensors();

    if (leftFarReading > 200) {
      delay(leapTime - 30);
      readSensors();

      if (rightFarReading > 200 && leftFarReading > 200) { //Si la lectura sigue siendo 200 en los sensores lejanos izquirdo y derecho entonces que ejecute la funcion DONE la cual significa que a terminado
        done();
      }
      else {
        turnLeft();
        return;
      }
    }
    delay(leapTime - 30);
    readSensors();
    if (leftFarReading < 200 && leftCenterReading < 200 &&
        rightCenterReading < 200 && rightFarReading < 200) {
      turnRight();
      return;
    }
    // despues de ejecutar todas las comprobaciones, va a guardar una letra en la variabla char PATH para que este pueda decidir despues el camino mas corto

    path[pathLength] = 'S';
    // Serial.println("s");
    pathLength++;
    //Serial.print("Path length: ");
    //Serial.println(pathLength);
    if (path[pathLength - 2] == 'B') {
      //Serial.println("shortening path");
      shortPath();
    }
    straight();
  }
  readSensors();                                                                // si entre todas las comprobaciones para girar a la izquierda o derecha, detecta un camino sin salida, entonces va a girar sobre si mismo 180 grados
  if (leftFarReading < 200 && leftCenterReading < 200 && rightCenterReading < 200
      && rightFarReading < 200 && leftNearReading < 200 && rightNearReading < 200) {
    turnAround();
  }
}

void done() {                                 //Funcion creada para Finalizar el recorrido, cuando sea necesario y ejecutar el replay para que calcule el circuito mas corto
  digitalWrite(leftMotor1, LOW);
  digitalWrite(leftMotor2, LOW);
  digitalWrite(rightMotor1, LOW);
  digitalWrite(rightMotor2, LOW);
  replaystage = 1;
  path[pathLength] = 'D';
  pathLength++;
  while (analogRead(leftFarSensor) > 200) {  // Mientras este en la meta final parpadeara el led, al levantarlo y dara un tiempo para regresarlo a punto de inicio
    digitalWrite(led, LOW);
    delay(150);
    digitalWrite(led, HIGH);
    delay(150);
  }
  delay(5000);                             // esperara 5 segundo para que nos de tiempo de poner el carro al principio del recorrido y este ejecute el camino mas corto
  replay();
}

void turnLeft() {                     // Funcion creada para Girar a la Izquirda y que guarde el estado PATH en este caso la letra L
  while (analogRead(rightCenterSensor) > 200 || analogRead(leftCenterSensor) > 200) {
    digitalWrite(leftMotor1, LOW);
    digitalWrite(leftMotor2, HIGH);
    digitalWrite(rightMotor1, HIGH);
    digitalWrite(rightMotor2, LOW);
    delay(2);
    digitalWrite(leftMotor1, LOW);
    digitalWrite(leftMotor2, LOW);
    digitalWrite(rightMotor1, LOW);
    digitalWrite(rightMotor2, LOW);
    delay(1);
  }
  while (analogRead(rightCenterSensor) < 200) {
    digitalWrite(leftMotor1, LOW);
    digitalWrite(leftMotor2, HIGH);
    digitalWrite(rightMotor1, HIGH);
    digitalWrite(rightMotor2, LOW);
    delay(2);
    digitalWrite(leftMotor1, LOW);
    digitalWrite(leftMotor2, LOW);
    digitalWrite(rightMotor1, LOW);
    digitalWrite(rightMotor2, LOW);
    delay(1);
  }
 if (replaystage == 0) {
    path[pathLength] = 'L';
    //Serial.println("l");
    pathLength++;
    //Serial.print("Path length: ");
    //Serial.println(pathLength);
    if (path[pathLength - 2] == 'B') {
      //Serial.println("shortening path");
      shortPath();
    }
  }
}

void turnRight() {                            // Funcion creada para Girar a la Derecha y que guarde el estado PATH en este caso la letra R
  while (analogRead(rightCenterSensor) > 200) {
    digitalWrite(leftMotor1, HIGH);
    digitalWrite(leftMotor2, LOW);
    digitalWrite(rightMotor1, LOW);
    digitalWrite(rightMotor2, HIGH);
    delay(2);
    digitalWrite(leftMotor1, LOW);
    digitalWrite(leftMotor2, LOW);
    digitalWrite(rightMotor1, LOW);
    digitalWrite(rightMotor2, LOW);
    delay(1);
  }
  while (analogRead(rightCenterSensor) < 200) {
    digitalWrite(leftMotor1, HIGH);
    digitalWrite(leftMotor2, LOW);
    digitalWrite(rightMotor1, LOW);
    digitalWrite(rightMotor2, HIGH);
    delay(2);
    digitalWrite(leftMotor1, LOW);
    digitalWrite(leftMotor2, LOW);
    digitalWrite(rightMotor1, LOW);
    digitalWrite(rightMotor2, LOW);
    delay(1);
  }
  while (analogRead(leftCenterSensor) < 200) {
    digitalWrite(leftMotor1, HIGH);
    digitalWrite(leftMotor2, LOW);
    digitalWrite(rightMotor1, LOW);
    digitalWrite(rightMotor2, HIGH);
    delay(2);
    digitalWrite(leftMotor1, LOW);
    digitalWrite(leftMotor2, LOW);
    digitalWrite(rightMotor1, LOW);
    digitalWrite(rightMotor2, LOW);
    delay(1);
  }
  if (replaystage == 0) {
    path[pathLength] = 'R';
    Serial.println("r");
    pathLength++;
    Serial.print("Path length: ");
    Serial.println(pathLength);
    if (path[pathLength - 2] == 'B') {
      Serial.println("shortening path");
      shortPath();
    }
  }
}

void straight() {                 // Funcion creada para ir hacia Adelante y que guarde el estado PATH en este caso la letra S
  if ( analogRead(leftCenterSensor) < 200) {
    digitalWrite(leftMotor1, HIGH);
    digitalWrite(leftMotor2, LOW);
    digitalWrite(rightMotor1, HIGH);
    digitalWrite(rightMotor2, LOW);
    delay(1);
    digitalWrite(leftMotor1, HIGH);
    digitalWrite(leftMotor2, LOW);
    digitalWrite(rightMotor1, LOW);
    digitalWrite(rightMotor2, LOW);
    delay(5);
    return;
  }
  if (analogRead(rightCenterSensor) < 200) {
    digitalWrite(leftMotor1, HIGH);
    digitalWrite(leftMotor2, LOW);
    digitalWrite(rightMotor1, HIGH);
    digitalWrite(rightMotor2, LOW);
    delay(1);
    digitalWrite(leftMotor1, LOW);
    digitalWrite(leftMotor2, LOW);
    digitalWrite(rightMotor1, HIGH);
    digitalWrite(rightMotor2, LOW);
    delay(5);
    return;
  }
  digitalWrite(leftMotor1, HIGH);
  digitalWrite(leftMotor2, LOW);
  digitalWrite(rightMotor1, HIGH);
  digitalWrite(rightMotor2, LOW);
  delay(4);
  digitalWrite(leftMotor1, LOW);
  digitalWrite(leftMotor2, LOW);
  digitalWrite(rightMotor1, LOW);
  digitalWrite(rightMotor2, LOW);
  delay(1);

}

void turnAround() {                     // Funcion creada para Girar 180 grados y que guarde el estado PATH en este caso la letra B
  digitalWrite(leftMotor1, HIGH);
  digitalWrite(leftMotor2, LOW);
  digitalWrite(rightMotor1, HIGH);
  digitalWrite(rightMotor2, LOW);
  delay(150);
  while (analogRead(leftCenterSensor) < 200) {
    digitalWrite(leftMotor1, LOW);
    digitalWrite(leftMotor2, HIGH);
    digitalWrite(rightMotor1, HIGH);
    digitalWrite(rightMotor2, LOW);
    delay(2);
    digitalWrite(leftMotor1, LOW);
    digitalWrite(leftMotor2, LOW);
    digitalWrite(rightMotor1, LOW);
    digitalWrite(rightMotor2, LOW);
    delay(1);
  }
  path[pathLength] = 'B';
  pathLength++;
  straight();
  //Serial.println("b");
  //Serial.print("Path length: ");
  //Serial.println(pathLength);
}

void shortPath() {                                                // Funcion creada para buscar el camino mas corto para poder recordar el recorrido, utilizando un seguimiento directo hacia la salida
  int shortDone = 0;
  if (path[pathLength - 3] == 'L' && path[pathLength - 1] == 'R') { // Girar 180 grados
    pathLength -= 3;
    path[pathLength] = 'B';
    //Serial.println("test1");
    shortDone = 1;
  }
  if (path[pathLength - 3] == 'L' && path[pathLength - 1] == 'S' && shortDone == 0) { // Girar hacia la derecha
    pathLength -= 3;
    path[pathLength] = 'R';
    //Serial.println("test2");
    shortDone = 1;
  }
  if (path[pathLength - 3] == 'R' && path[pathLength - 1] == 'L' && shortDone == 0) { // Girar 180 grados
    pathLength -= 3;
    path[pathLength] = 'B';
    //Serial.println("test3");
    shortDone = 1;
  }
 if (path[pathLength - 3] == 'S' && path[pathLength - 1] == 'L' && shortDone == 0) { // Girar hacia la Izquierda
    pathLength -= 3;
    path[pathLength] = 'R';
    //Serial.println("test4");
    shortDone = 1;
  }
  if (path[pathLength - 3] == 'S' && path[pathLength - 1] == 'S' && shortDone == 0) { // Girar 180 grados
    pathLength -= 3;
    path[pathLength] = 'B';
    //Serial.println("test5");
    shortDone = 1;
  }
  if (path[pathLength - 3] == 'L' && path[pathLength - 1] == 'L' && shortDone == 0) { // Seguir avanzando
    pathLength -= 3;
    path[pathLength] = 'S';
    //Serial.println("test6");
    shortDone = 1;
  }
  path[pathLength + 1] = 'D';     // Terminado
  path[pathLength + 2] = 'D';
  pathLength++;
  //Serial.print("Path length: ");
  //Serial.println(pathLength);
  //printPath();
}

void printPath() {                              // por si tuvieramos conectado a un puerto serial, nos imprimira los valores de recorrido
  Serial.println("+++++++++++++++++");
  int x;
  while (x <= pathLength) {
    Serial.println(path[x]);
    x++;
  }
  Serial.println("+++++++++++++++++");
}

void replay() {                                       // Funcion que reconoce el recorrido mas corto a la hora de repetir el circuito
  readSensors();
  if (leftFarReading < 200 && rightFarReading < 200) {
    straight();
  }
  else {
    if (path[readLength] == 'D') {
      digitalWrite(leftMotor1, HIGH);
      digitalWrite(leftMotor2, LOW);
      digitalWrite(rightMotor1, HIGH);
      digitalWrite(rightMotor2, LOW);
      delay(100);
      digitalWrite(leftMotor1, LOW);
      digitalWrite(leftMotor2, LOW);
      digitalWrite(rightMotor1, LOW);
      digitalWrite(rightMotor2, LOW);
      endMotion();
    }
    if (path[readLength] == 'L') {
      digitalWrite(leftMotor1, HIGH);
      digitalWrite(leftMotor2, LOW);
      digitalWrite(rightMotor1, HIGH);
      digitalWrite(rightMotor2, LOW);
      delay(leapTime);
      turnLeft();
    }
    if (path[readLength] == 'R') {
      digitalWrite(leftMotor1, HIGH);
      digitalWrite(leftMotor2, LOW);
      digitalWrite(rightMotor1, HIGH);
      digitalWrite(rightMotor2, LOW);
      delay(leapTime);
      turnRight();
    }
    if (path[readLength] == 'S') {
      digitalWrite(leftMotor1, HIGH);
      digitalWrite(leftMotor2, LOW);
      digitalWrite(rightMotor1, HIGH);
      digitalWrite(rightMotor2, LOW);
      delay(leapTime);
      straight();
    }
    readLength++;
  }
  replay();
}

void endMotion() {   // Fucnion creada para apagar todo y que de una secuencia de luces la cual siginifica que ya podes repetir el proceso
  digitalWrite(led, LOW);
  delay(500);
  digitalWrite(led, HIGH);
  delay(200);
  digitalWrite(led, LOW);
  delay(200);
  digitalWrite(led, HIGH);
  delay(500);
  endMotion();
}

```

## Agradecimientos

- Luis Olivet (creación del tutorial) 
- Samuel Palma (revisión y adaptación del tutorial)
- Alumnos de Ingeniería en Sistemas de la Universidad Mariano Gálvez de Guatemala de la Clase de Inteligencia Artificial (creación, transcripción y comentarios del código)

## Licencia

ArduChallenge es un proyecto de código abierto con [Licencia MIT](https://opensource.org/licenses/MIT).
