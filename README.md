#include <ESP32Servo.h>
// 180 horizontal MAX
Servo horizontal; // horizontal servo
int servoh = 90; // 90; // stand horizontal servo
int servohLimitHigh = 180;
int servohLimitLow = 0;
//65 degrees MAX
Servo vertical;// vertical servo
int servov = 45;  // 90; // stand vertical servo
int servovLimitHigh = 180;
int servovLimitLow = 0;
//Panel voltage "sensor"
const int voltagePin = 35;
  float analogValue = 0;
  float voltage = 0;

void setup() {
  //put your setup code here, to run:
  horizontal.attach(18);
  vertical.attach(19);  
  horizontal.write(90);
  vertical.write(45);

  Serial.begin(9600);
  delay(5000);
}

void loop () {
//main code

int dtime =10; int tol = 50;

int lt = analogRead(12); //top left
int rt = analogRead(13); //top right
int ld = analogRead(14); //down left
int rd = analogRead(15); //down right


int avt = ( lt+ rt) /2; // average value top
int avd = ( ld+ rd) /2; // average value down
int avl = ( lt+ ld) /2; // average value left
int avr = ( rt+ rd) /2; // average value right

int dvert = avt - avd; //check the diffirence of up and down
int dhoriz = avl - avr; //check the diffirence of right and left

    analogValue = analogRead(voltagePin);
    voltage = map(analogValue, 0, 4095, 0.0, 2500.0); 
    voltage /=100;
    
    Serial.print("voltage ");
    Serial.println(voltage);

if (-1*tol > dvert || dvert > tol)
{
  if(avt<avd)
  {
  servov=servov + 1;
  if (servov > servovLimitHigh)
  {
    servov = servovLimitHigh;
  }
  vertical.write(servov);
  delay(20);
  }
else if(avt>avd)
 {
  servov=servov - 1;
  if (servov < servovLimitLow)
    {
servov = servovLimitLow;
    }

vertical.write(servov);
  delay(20);
 }
else if (avt==avd)
    {
  //nothing
    }
}


if (-1*tol > dhoriz || dhoriz > tol)
{
  if(avl>avr)
  {
    servoh=servoh - 1;
    if (servoh > servohLimitHigh)
    {
      servoh = servohLimitHigh;
    }
    horizontal.write(servoh);
    delay(20);
  }
  else if (avl<avr)
  {
    servoh=servoh + 1;
      if (servoh < servohLimitLow)
      {
        servoh = servohLimitLow;
      }
      horizontal.write(servoh);
      delay(20);
      }
      else if(avl==avr)
      {
        //nothing
      }
}
delay(100);
}
