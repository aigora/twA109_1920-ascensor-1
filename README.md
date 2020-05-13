# ASCENSOR

Queremos realizar un ascensor con arduino que suba y baje con la ayuda de un servomotor unido a un sistema mecanico, para trasladar el movimiento de rotación del servomotor a un movimiento lineal de subida o bajada.

## Maria Ortega Monge 54777 y Laura Villajos Mateo 54904
Maria Ortega Monge con usuario de Github mariaortegamonge.
Laura Villajos Mateo con usuario de Github lauravillajosmateo.

## OBJETIVOS

El objetivo principal como se ha mencionado antes es conseguir el movimiento de subida y bajada con un servomotor.
También queremos añadirle una especie de semáforo que indique cuando se está moviendo (azul), cuando esta en el primer piso(verde) y cuando esta en el segundo piso (rojo).
Adicionalmente queremos ponerle un altavoz con sonido que emita este sonido cuando este en movimiento y deje de emitirlo cuando pare.
Y unos botones que serían los encargados de mandarle la señal al servomotor de que suba o baje el ascensor.

## SENSORES Y ACTUADORES

El proyecto dispone de un control en pantalla para iniciar todo el proceso. Además, cuenta con un servomotor, varios LEDs de diferentes colores, un buzzer para emirtir sonidos, dos pulsadores de subida y bajada y una pantalla que incluirá información acerca del trabajo.

## SUBPROGRAMAS (FUNCIONES)
Contamos con cuatro funciones: función subir, función bajar y función para.

El procesamiento de la función subir es: se activa mediante el accionamiento de un pulsador. Esto hace que el servomotor gire un numero determinado de grados mediante un bucle for y además activa el LED correspondiente a ese movimiento y el sonido. El giro del servomotor permite mediante un mecanismo de cuerda y polea que el ascensor suba.

El procesamiento de la función bajar es: al contrario que la función subir, el pulsador hace que el servomotor gire el mismo numero de grados que gira en la funcion subir pero en el sentido contrario y mediante el mismo mecanismo de cuerda y polea consigue bajar el ascensor. También se activa el LED del color correspondiente y el sonido.

El procesamiento de la funcion para es: Cuando termina la función subir o la función bajar se activa esta función para hacer que el servomotor no haga ningún movimiento y el ascensor se quede quieto en el piso que esté, para el sonido y enciende el LED que le corresponde a este "movimiento".


## Primera Versión 

#include <Servo.h>
Servo myservo;
const int pinBuzzer = 12;
int RS = 4, t E = 2, D4 = 1, D5 = 10, D6 = 13,
D7 = 8, VO = 3;
const int  inPin1=5;//
const int  inPin2=7;//
const int  ROJOPin=9;
const int  VERDEPin=10;

void baja ()// variamos los valores de giro para que giren las poleas
{ 
digitalWrite(9,LOW);
digitalWrite(10,LOW);
 for(int i =0;i<95;i=i+2)
 {myservo.write(i);
  delay(100);
 }
 delay(1000); 
}

oid sube()// variamos los valores de giro para
 que giren al lado contrario
{
digitalWrite(9,LOW);
digitalWrite(10,LOW);
 for(int i=90;i>=0;i=i-2)
 {
  myservo.write(i);
  delay(100);
 }
 delay(1000);
}

void para () //la utilizaremos al inicio y cuando esté bajando y 
se pulse el botón subir.
{digitalWrite(inPin1,LOW);//ponemos los pines  6 y 7 en 
balor bajo
digitalWrite(inPin2,LOW);      
}

void loop() {
para();
  sube();
  para();
  baja();
  }





