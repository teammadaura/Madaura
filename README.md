# Madaura
El teammadaura es un grupo  de jóvenes que esta fabricando un vehículo para la competencia de futuros ingenieros en la WRO 2025. El equipo esta conformado por dos jóvenes de 8vo grado y un joven de 9no grado
1.¿Qué hace el proyecto?

 Es un vehiculo que cumple las funciones de avanzar hacia adelante, detenerse, avanzar hacia atras, el vehiculo puede girar a la izquierda, y tambien puede girar a la derecha. A futuro tenemos planeado la implementacion de modulo de camara junto a la tarjeta de desarrollo ESP32. La camara la usaremos para detectar los pilares de color verde y rojo, y asi poder esquibarlos.

Le añadimos 3 sensores de sonido a los costados y al frente del carro con piezas fabricadas con PVC, sustituimos el chasis del carro de plastico a uno de aluminio porque el chasis anterior se estaba doblando a medida que el carro se usaba para pruebas, con el nuevo chasis el carro se hizo ligeramente mas largo y le implementamos al carro un pulsador que actue como boton de arranque.


2.¿Por qué el proyecto es útil?

Es util ya que nos permite participar en la categoria futuros ingenieros en la competencia WRO 2025, tambien nos permite desarrollar nuestras habilidades como ingenieros y programadores.

¿Cómo comenzaron los usuarios con el proyecto?
Comenzamos armando las partes principales del carro como las que pueden ser los motores, ruedas, direccionales, plataformas utilizando el kit de robotica que nos proporciono nuestra institucion llamado olibot. Al terminar de ensamblar empezamos una busqueda de componentes que nos puedan ayudar a nuestro carro como los que pueden ser el puente h l298n, sensores de ultrasonido, arduino y un sensor de color TCS3200 que actualmente no estamos usando por las constantes fallas en su deteccion de color. 

¿Dónde pueden recibir ayuda los usuarios con el proyecto?

Podemos recibir ayuda de los docentes y personal del colegio U.E.P San Agustin, o de los representantes de los estudiantes del club de robotica

¿Quién mantiene y contribuye con el proyecto?

En el proyecto contribuyen los docentes y estudiantes de U.E.P San Agustin, junto con la ayuda de ciertos representantes profesionales en el area de robotica como por ejemplo, el Padre de Samuel Tua

Aqui abajo se encuentra el código actualizado👇:

#include <Servo.h>
int trigfron=11, echofron=12;
int trigder=10, echoder=8;
int trigizq=6, echoizq=7;
int IN1=2, IN2=4, ENA=3;
Servo servo;
long duracionfrontal;
float distanciafrontal; 
long duracionderecha;
float distanciaderecha;
long duracionizquierda;
float distanciaizquierda;

void setup() {
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(trigfron, OUTPUT);
  pinMode(echofron, INPUT);
  pinMode(trigder, OUTPUT);
  pinMode(echoder, INPUT);
  pinMode(trigizq, OUTPUT);
  pinMode(echoizq, INPUT);
  servo.attach(9);
  servo.write(78);
  Serial.begin(9600);

}
   //Codigo de testeo para calibrar ultrasonido frontal, que detenga el carro y retroceda si detecta un objeto a una distancia menor o igual a 20cm
void loop() 
{
  analogWrite(ENA, 200); //Velocidad del motor 
    distanciafrontal = leerUltrasonido(trigfron, echofron);
    distanciaderecha = leerUltrasonido(trigder, echoder);
    distanciaizquierda = leerUltrasonido(trigizq, echoizq);
   
    Serial.print("Distancia Front:"), Serial.print(distanciafrontal);
    Serial.print("  Distancia Der:"), Serial.print(distanciaderecha);        //Impresion de las distancias en el monitor serial
    Serial.print("  Distancia Izq:"), Serial.println(distanciaizquierda);
    if(distanciafrontal <= 20)
    {
     digitalWrite(IN1, LOW);
     digitalWrite(IN2, HIGH);                     //CONSTANTE ERROR: MOTOR DC REACCIONA SIN QUE SE CUMPLA LA CONDICIONAL, POSEE RETARDOS Y COMPORTAMIENTOS NO ESPERADOS
                          
    }else{
     digitalWrite(IN1, HIGH);
     digitalWrite(IN2, LOW);
    }
  
  }
}
//Funcion del servomotor
void direccion(int dir)
{
  switch(dir)
  {
    case 1: //Girar servo hacia la derecha
    servo.write(113);
    break;
    case 0: //Servo ubicado en en centro
    servo.write(78);
    break;
    case -1: /Girar servo hacia la izquierda
    servo.write(15);
    break;
  }
}
//Funcion de direccionales del motor dc
void dirmotor(int sentido)
{
  switch(sentido)
  {
    case 1: //Motor hacia adelante
    digitalWrite(IN1, HIGH);  
    digitalWrite(IN2, LOW);
    break;
    
    case 0: //Motor hacia atras
    digitalWrite(IN1, LOW);
    digitalWrite(IN2, HIGH);                                                        
    break;
                                                                                                            
    default: //Motor apagado
    digitalWrite(IN1, LOW);
    digitalWrite(IN2, LOW);
    break;
  }
}
//Funcion de lectura de ultrasonido
float leerUltrasonido(int trigPin, int echoPin) {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  long duration_us = pulseIn(echoPin, HIGH, 25000);
  float distanceCm = duration_us * 0.0343 / 2.0;
  return distanceCm;
}
