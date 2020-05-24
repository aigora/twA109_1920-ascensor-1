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

void sube()// variamos los valores de giro para
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

## VERSION DEFINITIVA
1: Conexión al C.

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "SerialPort.h"
#include "SerialPort.C"
#define MAX_DATA_LENGTH 255
// Funciones prototipo
void autoConnect(SerialPort *arduino,char*);
int main(void)
{
    
   SerialPort *arduino;
    // Puerto serie en el que esta¡ Arduino
    
   char* portName = "\\\\.\\COM3";
    
   // Buffer para datos procedentes de Arduino
   
   char incomingData[MAX_DATA_LENGTH];
    // Crear estructura de datos del puerto serie
    
   arduino = (SerialPort *)malloc(sizeof(SerialPort));
    // Apertura del puerto serie
    
   Crear_Conexion(arduino,portName);
    autoConnect(arduino,incomingData);
    return 0;
}
void autoConnect(SerialPort *arduino,char *incomingData)
{
    char sendData = 0;
    int readResult;
    // Espera la conexión con Arduino
    
   while (!isConnected(arduino))
    {
        Sleep(100);
        Crear_Conexion(arduino,arduino->portName);
    }
    //Comprueba si arduino esta conectado
    
   if (isConnected(arduino))
    {
        printf ("Conectado con Arduino en el puerto %s\n",arduino->portName);
    }
    // Bucle de la aplicacion
    
   printf ("0 - OFF, 1 - ON, 9 - SALIR\n");
    scanf("%c",&sendData);
    while (isConnected(arduino) && sendData!='9')
    
   {
        sendData = getch();
        writeSerialPort(arduino,&sendData, sizeof(char));
        readResult=readSerialPort(arduino,incomingData,MAX_DATA_LENGTH);
        if (readResult!=0)
            printf ("%s",incomingData);
        sleep(10);
    }
    
   if (!isConnected(arduino))
        printf ("Se ha perdido la conexión con Arduino\n");
}


2.Programa Arduino.

#include <LiquidCrystal.h>
#include <Servo.h>

Servo myservo;
const int pinBuzzer = 12;
int RS = 4;
int E = 2;
int D4 = 1;
int D5 = 10;
int D6 = 13;
int D7 = 8;
int V0 = 3;
LiquidCrystal lcd (RS, E, D4, D5, D6, D7);

const int  inPin1=5;
const int  inPin2=7;//botones de subida y bajada

const int  ROJOPin=9;
const int  VERDEPin=10;
//de momento no const int  AZULPin=8;
int pa=LOW;



//DECLARACION DE FUNCIONES

void baja ()// variamos los valores de giro para baje la plataforma

{
  tone(pinBuzzer, 650);
  digitalWrite(8,HIGH);
digitalWrite(9,LOW);
digitalWrite(10,LOW);

 for(int i =0;i<95;i=i+2)
 {
  myservo.write(i);
  delay(100);
 }
 delay(1000);
 digitalWrite(8,LOW);
 noTone(pinBuzzer); 
}

void sube()// variamos los valores de giro para suba

{
   tone(pinBuzzer, 650);
digitalWrite(8,HIGH);
digitalWrite(9,LOW);
digitalWrite(10,LOW);

 for(int i=90;i>=0;i=i-2)
 {
  myservo.write(i);
  delay(100);
 }
 delay(1000);
 digitalWrite(8,LOW);
 noTone(pinBuzzer);
}

void para () //funcion para que el motor no se mueva;la utilizaremos al inicio y mientras no se activen los botones de subida o bajada

{
digitalWrite(inPin1,LOW);//ponemos los pines  6 y 7 en balor bajo
	
digitalWrite(inPin2,LOW);
         
}


void setup() {
  
  analogWrite(V0, 30);
    lcd.begin(16,2);
    lcd.setCursor (0,0);
    lcd.print("trabajo");
    lcd.setCursor (0,1);
    lcd.print("ascensor");
  Serial.begin(9600);
  
  pinMode(pinBuzzer,OUTPUT);
myservo.attach(6);

pinMode(inPin1,INPUT);
pinMode(inPin2,INPUT);
  pinMode(8,OUTPUT); //Rojo
  pinMode(9,OUTPUT); //Azul
  pinMode(10,OUTPUT); //Verde
}

  
void loop() {
 
para();
	pa=LOW;// inicializamos la variable que almacena el valor del pulsador a un valor bajo
	
while(pa==LOW)//mientras el valor de pa siga siendo bajo (no lo hayamos pulsado)seguimos leyendo su estado
	{
		pa = digitalRead(inPin1);//esta funcion lee el estado del pin PA y cuando lo hayamos pulsado cambiará de LOW a HIGH y saltará del bucle
		//SEMÁFORO EN ROJO MIENTRAS NO HAYA ACTIVACION DE BOTONES
		
digitalWrite(9,HIGH);
digitalWrite(8,LOW);
digitalWrite(10,LOW);
		}
pa=LOW;//reinicializamos pa

  sube();
  para();
//lo primero que hacemos es tener el motor parado para inicializar bajos los niveles de MAPin y MCPin

pa=LOW;// inicializamos la variable que almacena el valor del pulsador a un valor bajo

while(pa==LOW)//mientras el valor de pa siga siendo bajo (no lo hayamos pulsado)seguimos leyendo su estado
	{
		pa = digitalRead(inPin2);//esta funcion lee el estado del pin PA y cuando lo hayamos pulsado cambiará de LOW a HIGH y saltará del bucle
		//SEMÁFORO EN ROJO MIENTRAS NO HAYA ACTIVACION DE BOTONES
		
digitalWrite(10,HIGH);
digitalWrite(9,LOW);
digitalWrite(8,LOW);
		}
	pa=LOW;//reinicializamos pa

  baja();
    
  }



