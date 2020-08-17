//Reverse Car Parking System
int sensor=2;
int buzzer=3;
void setup()
{
Serial.begin(9600);
pinMode(trigpin,OUTPUT);
pinMode(echopin,INPUT);
pinMode(buzzer,OUTPUT);
}
void loop()
{
int a=digitalRead(sensor);
if (a==1)
{
duration=pulseln(echopin,HIGH);

distance=duration*0.034/2;

Serial.println(“object is near”);

digitalWrite(buzzer,HIGH);
}
else
{
Serial.println(“ no object”);
digitalWrite(buzzer,LOW);
}
}   

//Car Speed Detector with LCD Display
#include<reg51.h>
sfr LCD=0x80;
sbit RS=P2^6;
sbit EN=P2^7;
unsigned int a=0,i=0,v;
void tm();
void delay(unsigned char time)
{
unsigned int a,b;
for(a=0;a<time;a++)
for(b=0;b<1275;b++);
}
void lcdcmd(unsigned char value)
{
LCD=value;
RS=0;
EN=1;
delay(10);
EN=0;
}
void lcddata(unsigned char value)
{
LCD=value;
RS=1;
EN=1;
delay(10);
EN=0;
}
void lcd_init()
{
lcdcmd(0x38);

delay(20);
lcdcmd(0x0c);
delay(20);
lcdcmd(0x01);
delay(20);
lcdcmd(0x06);
delay(20);
}
void ISR_ex0(void) interrupt 0
{
while(1)
{
tm();
a++;
}
}
void ISR_ex1(void) interrupt 2
{
unsigned char m,n,temp,o;
v=100/a;
m=v/100;
temp=v%100;
n=temp/10;
o=temp%10;
lcdcmd(0xc6) ;
lcddata(m+48);
lacdata(n+48);
lcddata(o+48);
a=0;
lcddata(‘m’);
lcddata(‘m’);
lcddata(‘/’);
lcddata(‘s’);

lcddata(‘e’);
lcddata(‘c’);
lcddata(‘ ’);
lcddata(‘ ’);
}
void main()
{
unsigned char w[]=”WELCOMESPEED=WAITING…”;
IT0=1;
EX0=1;
IT1=1;
EX1=1;
EA=1;
IP=0X04;
lcd_init();
lcdcmd(0x84);
for(i=0;i<7:i++)
lcddata(w[i]);
lcdcmd(0xc0);
for(i=7;i<13;i++)
lcddata(w[i]);
for(i=13;i<24;i++)
lcddata(w[i]);
while(1);
}
void tm()
{
int y=0;
for(y=0;y<15:y++)
{
TMOD=0x01;
TL0=0xFD;
TH0=0x4B

TR0=1;
while(TF0==0);
TR0=0;
TF0=0;
}
}

//Density Based Traffic Control System
#include<TimerOne.h>
int signal1[] = {23, 25, 27};
int signal2[] = {46, 48, 50};
int signal3[] = {13, 12, 11};
int signal4[] = {10, 9, 8};

int redDelay = 5000;
int yellowDelay = 2000;

volatile int triggerpin1 = 31;    
volatile int echopin1 = 29;       
volatile int triggerpin2 = 44;     
volatile int echopin2 = 42;        
volatile int triggerpin3 = 7;    
volatile int echopin3 = 6;       
volatile int triggerpin4 = 5;    
volatile int echopin4 = 4;       
volatile long time;                    
volatile int S1, S2, S3, S4;          

int t = 5; 
void setup()
{
  
Serial.begin(115200);
Timer1.initialize(100000);  
Timer1.attachInterrupt(softInterr);
for(int i=0; i<3; i++)

{
  pinMode(signal1[i], OUTPUT);

 pinMode(signal2[i], OUTPUT);
  pinMode(signal3[i], OUTPUT);
  pinMode(signal4[i], OUTPUT);
}
  
  pinMode(triggerpin1, OUTPUT);  
  pinMode(echopin1, INPUT);      
  pinMode(triggerpin2, OUTPUT);  
  pinMode(echopin2, INPUT);
  pinMode(triggerpin3, OUTPUT);  

  pinMode(echopin3, INPUT);
  pinMode(triggerpin4, OUTPUT);  
  pinMode(echopin4, INPUT); 
 }

void loop()
{  

if(S1<t)
{
signal1Function();
}  

if(S2<t)
{
 signal2Function();
 } 
 
if(S3<t)
{
signal3Function();
} 

if(S4<t)
{
signal4Function();
}
}

void softInterr()
{

  digitalWrite(triggerpin1, LOW);  
  delayMicroseconds(2);
  digitalWrite(triggerpin1, HIGH); 
  delayMicroseconds(10);
  digitalWrite(triggerpin1, LOW);
  time = pulseIn(echopin1, HIGH); 
  S1= time*0.034/2;
 
  digitalWrite(triggerpin2, LOW);  
  delayMicroseconds(2);
  digitalWrite(triggerpin2, HIGH); 
  delayMicroseconds(10);
  digitalWrite(triggerpin2, LOW);

time = pulseIn(echopin2, HIGH); 
S2= time*0.034/2; 

digitalWrite(triggerpin3, LOW);  
delayMicroseconds(2);
digitalWrite(triggerpin3, HIGH); 
delayMicroseconds(10);
  
digitalWrite(triggerpin3, LOW);
time = pulseIn(echopin3, HIGH); 
S3= time*0.034/2;

digitalWrite(triggerpin4, LOW);  
delayMicroseconds(2);
digitalWrite(triggerpin4, HIGH); 
delayMicroseconds(10);
digitalWrite(triggerpin4, LOW);
time = pulseIn(echopin4, HIGH); 
S4= time*0.034/2;

Serial.print("S1: ");
Serial.print(S1);
Serial.print("  S2: ");
Serial.print(S2);
Serial.print("  S3: ");
Serial.print(S3);
Serial.print("  S4: ");
Serial.println(S4);
}

void signal1Function()
{
 Serial.println("1");
 low();
 digitalWrite(signal1[0], LOW);
 digitalWrite(signal1[2], HIGH);
 delay(redDelay);
 if(S2<t || S3<t || S4<t)
 
{  
 digitalWrite(signal1[2], LOW);
 digitalWrite(signal1[1], HIGH);
 delay(yellowDelay);
}

}

void signal2Function()
{
 Serial.println("2");
 low();
 digitalWrite(signal2[0], LOW);
 digitalWrite(signal2[2], HIGH);
 delay(redDelay);
  if(S1<t || S3<t || S4<t)
  {
  digitalWrite(signal2[2], LOW);    
  digitalWrite(signal2[1], HIGH);
  delay(yellowDelay);   
  }
 }
void signal3Function()
{
Serial.println("3");
low();
digitalWrite(signal3[0], LOW);
digitalWrite(signal3[2], HIGH);
delay(redDelay);

if(S1<t || S2<t || S4<t)
{
 digitalWrite(signal3[2], LOW);
 digitalWrite(signal3[1], HIGH);
 delay(yellowDelay);
}  
}

void signal4Function()
{
Serial.println("4");
low();
digitalWrite(signal4[0], LOW);
digitalWrite(signal4[2], HIGH);
delay(redDelay);
if(S1<t || S2<t || S3<t)
{
digitalWrite(signal4[2], LOW);
digitalWrite(signal4[1], HIGH);
delay(yellowDelay);
}
}
void low()
{
for(int i=1; i<3; i++)
{
digitalWrite(signal1[i], LOW);
digitalWrite(signal2[i], LOW);
digitalWrite(signal3[i], LOW);
digitalWrite(signal4[i], LOW);
}
for(int i=0; i<1; i++)
{
digitalWrite(signal1[i], HIGH);    
digitalWrite(signal2[i], HIGH);
digitalWrite(signal3[i], HIGH);
digitalWrite(signal4[i], HIGH);
}
}

//Temperature Based Fan Speed Control and Monitoring Using Arduino
#include<LiquidCrystal.h>
LiquidCrystal led(7,6,5,4,3,2);
int tempPin= A1;
int led= 8;
int temp;
int tempMin=30
int tempMax=60
int fanSpeed;
int fanLCD;
void setup()
{
PinMode(fan,OUTPUT);
PinMode(led,OUTPUT);
PinMode(tempPin,INPUT);
lcd.begin(16,2);
Serial.begin(9600);
}
void loop()
temp=readtemp();
Serial.print(temp);
 if ( temp< tempMin)
{
fanSpeed=0;
analogWrite(fan,fanSpeed);
fanLCD=0;
digitalWrite(fan,LOW);
}
if  ( temp >= tempMin ) && ( temp <= tempMax )
{
fanSpeed=temp;
map(temp tempMin, tempMax,, 0 , 100);
map(temp tempMin, tempMax,, 32, 255);
fanSpeed= fanSpeed*1.5;
analogWrite(fan,fanSpeed);
}
if ( temp > tempMax)
{
digitalWrite(led,HIGH);
}
else
{
digitalWrite(led,HIGH);
}
lcd.print(“ TEMP: ”)
lcd.print(temp);
lcd.print( “ C ”);                                                            
lcd,SetCursor(0,1);
lcd.print(“ FANS: ” );
lcd.print(fanLCD);
lcd.print(“ % ” );
delay(200);
lcd.clear();
{
int readTemp()
temp=analogRead(tempPin);
return temp*0.4882;
}
