# Obstacle-avoidance-robot
This robot uses 5 ultrasonic sensors and servo motor as a steering and can go autonomously by avoiding collisions. The arduino code is as follows:
#include<Servo.h>
Servo myservo;
const int pingPin1 = 2; // Trigger Pin of Ultrasonic Sensor1
const int echoPin1 = 3; // Echo Pin of Ultrasonic Sensor1
const int pingPin2 = 4; // Trigger Pin of Ultrasonic Sensor2
const int echoPin2 = 5; // Echo Pin of Ultrasonic Sensor2
const int pingPin3 = 6; // Trigger Pin of Ultrasonic Sensor3
const int echoPin3 = 7; // Echo Pin of Ultrasonic Sensor3
const int pingPin4 = 8; // Trigger Pin of Ultrasonic Sensor4
const int echoPin4 = 9; // Echo Pin of Ultrasonic Sensor4
const int pingPin5 = 12; // Trigger Pin of Ultrasonic Sensor5
const int echoPin5 = 13; // Echo Pin of Ultrasonic Sensor5
long dur1,D1,dur2,D2,dur3,D3,dur4,D4,dur5,D5;

int en1=10,en2=11;
int in1=A0;
int in2=A1;
int in3=A2;
int in4=A3;
int T=20;
int f=150,b=200;
void setup()
{
myservo.attach(3);  
pinMode(pingPin1, OUTPUT);
pinMode(pingPin2, OUTPUT);
pinMode(pingPin3, OUTPUT);
pinMode(pingPin4, OUTPUT);
pinMode(pingPin5, OUTPUT);
pinMode(echoPin1, INPUT);
pinMode(echoPin2, INPUT);
pinMode(echoPin3, INPUT);
pinMode(echoPin4, INPUT);
pinMode(echoPin5, INPUT);
pinMode(en1, OUTPUT);
pinMode(en2, OUTPUT);
pinMode(in1, OUTPUT);
pinMode(in2, OUTPUT);
pinMode(in3, OUTPUT);
pinMode(in4, OUTPUT);
Serial.begin(9600); // Starting Serial Terminal
}
void forward(){
 myservo.write(52);
  delay(200);
  analogWrite(en1,f);
  analogWrite(en2,b); 
  digitalWrite(in1,LOW);
  digitalWrite(in2,HIGH);
  digitalWrite(in3,HIGH);
  digitalWrite(in4,LOW);
  
  } 
void backward(){
  myservo.write(52);
  delay(200);
  analogWrite(en1,f);
  analogWrite(en2,b); 
  digitalWrite(in1,HIGH);
  digitalWrite(in2,LOW);
  digitalWrite(in3,LOW);
  digitalWrite(in4,HIGH);
  
  }
void fright(){
  myservo.write(2);              
  delay(200);                      
  analogWrite(en1,f);
  analogWrite(en2,b); 
  digitalWrite(in1,LOW);
  digitalWrite(in2,HIGH);
  digitalWrite(in3,HIGH);
  digitalWrite(in4,LOW);}
void fleft(){
  myservo.write(104);              
  delay(200);                             
  analogWrite(en1,f);
  analogWrite(en2,b); 
  digitalWrite(in1,LOW);
  digitalWrite(in2,HIGH);
  digitalWrite(in3,HIGH);
  digitalWrite(in4,LOW);}
void bright(){
  myservo.write(2);              
  delay(200);                      
  analogWrite(en1,f);
  analogWrite(en2,b); 
  digitalWrite(in1,HIGH);
  digitalWrite(in2,LOW);
  digitalWrite(in3,LOW);
  digitalWrite(in4,HIGH);
 }    
void bleft(){
  myservo.write(104);              
  delay(200);                            
  analogWrite(en1,f);
  analogWrite(en2,b); 
  digitalWrite(in1,HIGH);
  digitalWrite(in2,LOW);
  digitalWrite(in3,LOW);
  digitalWrite(in4,HIGH);}
void stopp(){
  myservo.write(52);
  delay(200);
  analogWrite(en1,f);
  analogWrite(en2,b); 
  digitalWrite(in1,LOW);
  digitalWrite(in2,LOW);
  digitalWrite(in3,LOW);
  digitalWrite(in4,LOW);
  }  
void loop()
{
  d1();
  d2();
  d3();
  d4();
  d5();
freeroam();
}
void d1(){
digitalWrite(pingPin1, LOW);
delayMicroseconds(2);
digitalWrite(pingPin1, HIGH);
delayMicroseconds(10);
digitalWrite(pingPin1, LOW);
dur1 = pulseIn(echoPin1, HIGH);
D1 = microsecondsToCentimeters(dur1);
Serial.print("d1:");
Serial.print(D1);
Serial.print(" \t");
delay(50);}
void d2(){
digitalWrite(pingPin2, LOW);
delayMicroseconds(2);
digitalWrite(pingPin2, HIGH);
delayMicroseconds(10);
digitalWrite(pingPin2, LOW);
dur2 = pulseIn(echoPin2, HIGH);
D2 = microsecondsToCentimeters(dur2);
Serial.print("d2:");
Serial.print(D2);
Serial.print(" \t");
delay(50);}
void d3(){
digitalWrite(pingPin3, LOW);
delayMicroseconds(2);
digitalWrite(pingPin3, HIGH);
delayMicroseconds(10);
digitalWrite(pingPin3, LOW);
dur3 = pulseIn(echoPin3, HIGH);
D3 = microsecondsToCentimeters(dur3);
Serial.print("d3:");
Serial.print(D3);
Serial.println();
delay(50);}
void d4(){
digitalWrite(pingPin4, LOW);
delayMicroseconds(2);
digitalWrite(pingPin4, HIGH);
delayMicroseconds(10);
digitalWrite(pingPin4, LOW);
dur4 = pulseIn(echoPin4, HIGH);
D4 = microsecondsToCentimeters(dur4);
Serial.print("d4:");
Serial.print(D4);
Serial.println();
delay(50);}
void d5(){
digitalWrite(pingPin5, LOW);
delayMicroseconds(2);
digitalWrite(pingPin5, HIGH);
delayMicroseconds(10);
digitalWrite(pingPin5, LOW);
dur5 = pulseIn(echoPin5, HIGH);
D5 = microsecondsToCentimeters(dur5);
Serial.print("d5:");
Serial.print(D5);
Serial.println();
delay(50);}
long microsecondsToCentimeters(long microseconds)
{
return microseconds / 29 / 2;
}
void freeroam()
{
     if(D2 <= T && D1>=T && D3 > T) /*IF LEFT DETECTED*/
     {
        fright();
     }
     if(D3 <= T && D1 >= T && D2 > T) /*IF RIGHT DETECTED*/
     {
      fleft();
     }
      if(D1 > T && D2 > T && D2 > T) /*IF SAFE TO MOVE*/
     {
      forward();
     }
     if(D2 > T && D1 <= T && D3 > T) /*IF ONLY CENTER DETECTED*/
    {
      stopp();
       delay(10000);
      /*WAIT FOR 10S FOR THE PATH TO BE CLEARED*/
      if (D3 < D2) /* CHECK IF MORE SPACE ON LEFT, IF YES THEN MOVE LEFT*/
          {
            fleft();
          }
        else
        if(D2 < D3) /*CHECK IF MORE SPACE ON RIGHT, IF YES THEN MOVE RIGHT*/
          {
            fright();
          }
        else
        {
          stopp();
        }
        }
        if(D2 <= T && D3 <= T && D1 > T) /*IF LEFT AND RIGHT DETECTED*/
          {
            stopp();
            delay(10000);
           /* WAIT FOR 10S FOR THE PATH TO BE CLEARED*/
            if(D4>D5) /*CHECK IF MORE SPACE ON BACK LEFT, IF YES THEN MOVE BACKLEFT*/
            {
              bleft();
            }
            else if(D5>D4) /*CHECK IF MORE SPACE ON BACK RIGHT, IF YES THEN  MOVE BRIGHT*/
            {
              bright();
            }
            else
            {
              stopp();
            }
          }
        if (D1 <= T && D3 <= T && D2 >T) /*IF FRONT AND RIGHT DETECTED MOVE FORWARD LEFT*/
          {
       fleft();
          }
        if (D1 <= T && D2 <= T && D3 > T) /*IF FRONT AND LEFT DETECTED MOVE FORWARD RIGHT*/
          {
          fright();
          }
            if(D2 <= T && D1 <= T && D3 <= T) /*IF FULLY BLOCKED IN FRONT*/
          {
          stopp();
            delay(10000); /*WAIT FOR 10S FOR THE PATH TO BE CLEARED*/
            if(D4>D5) /*IF MORE SPACE ON BACK LEFT MOVE BACKLEFT*/
            {
              bleft();
            }
            else if(D5>D4) /*IF MORE SPACE ON BACK RIGHT MOVE BACKRIGHT*/
            {
              bright();
            }
            else
            {
              stopp();
            }
          }
          if(D1 <= T && D2 <= T && D3 <= T && D4 <= T && D5 > T) /*IF FULLY BLOCKED IN FRONT AND IN BACKLEFT THEN MOVE BACK RIGHT*/
          {
            bright();
          }
          if(D1 <= T && D2 <= T && D3 <= T && D5 <= T && D4 > T) /*IF FULLY BLOCKED IN FRONT AND IN BACKRIGHT THEN MOVE BACKLEFT*/
          {
            bleft();
          }
          if(D1 <= T && D2 <= T && D3 <= T && D4 <= T && D5 <= T) /*IF COMPLETELY BLOCKED FROM ALL SIDES STOP*/
          {
            stopp();
          }
}
