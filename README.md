# mah
#include <Servo.h>                       //inlcui a biblioteca do motor servo
#include <Ultrasonic.h>                  //inclui a biblioteca do sensor ultrasonico
#define echoPin 13                       //define a entrada de sinal do sensor como pino 13 do arduino 
#define triggerPin 12                    //define a saída de sinal do sensor como pinto 12 do arduino
 
Servo myservo ;                           //chama a função do servo motor
Ultrasonic ultrasonic(12,13);             //chama a função do sensor ultrasonico 
 
void setup()
{
  Serial.begin(9600);                      //taxa de velocidade de comunicação 
  pinMode(triggerPin, OUTPUT);             //define o pino tringer como saida
  pinMode(echoPin, INPUT);                 //define o pino echo como entrada
  myservo.attach(10) ;                     //manda sinal para o servo motor
}
 
void loop()                               //veirifica sinal dentro do sensor
{
 
  digitalWrite(triggerPin, LOW);          //verifica se tem sinal no retorno do sensor a cada 2 micro segundos 
  delayMicroseconds(2);
  digitalWrite(triggerPin, HIGH);         //se tiver sinal por um periodo a cima de 10 micro segundos o motor é acionado
  delayMicroseconds(10);
  digitalWrite(triggerPin, LOW);          
  long duration = pulseIn(echoPin, HIGH);   //caso receba o sinal do sensor 
  int cm = microsecondsToCentimeters(duration);   
  Serial.println(cm);                       //manda pulso para para o sensor para que seja cauculo a distancia
  
  if(cm <= 30 && cm >=5)                    //valida a distancia caso seja cauculoda proximidade de 5 a 50 cm
  {
       abreTampa();                        //chama funcão para abrir tampa
       fechaTampa();                       //chama função fecha tampa
 
  }
}
 
void abreTampa()                          //executa o giro do motor de 0º a 180º com delay de 3 segundos
{
    myservo.write(180);
    delay(2000);
}
 
void fechaTampa()                        // executa o giro do motor de 180º a 0º com delay de 1 segundo 
{
  myservo.write(0);
  delay(1000);
}
 
float microsecondsToCentimeters(long microseconds)           // converte tempo do
{
  float seconds = (float) microseconds / 1000000.0;
  float distance = seconds * 340;
  distance = distance / 2;
  distance = distance * 100;
  return distance;
}
