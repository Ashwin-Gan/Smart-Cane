# Smart-Cane
//This project includes designing the prototype using ultrasonic sensor to track the distance of obstacles. If the obstacle is very close (less than 50 cm), the buzzer will beep, warning the person. The RED LED light will also glow. The intensity of the vibration of the cane will be high as the person approaches the obstacle closer. The microcontroller used is ESP8266 and the coding is done in Arduino IDE. Initially, simulation is made using the Arduino Uno board in Tinkercad.
//Code
//Defining constants
#define SOUND_VELOCITY 0.0343

//Defining pins
#define TRIG_Pin D5
#define ECHO_Pin D6
#define GLED_Pin D1
#define RLED_Pin D2
#define BUZZ_Pin D4

//Defining variables
float TIME, DIST_CM, VIB;

void setup()
{
  
  //Setting pin modes as I/P and O/P
  Serial.begin(115200);
  pinMode(TRIG_Pin,OUTPUT);
  pinMode(ECHO_Pin,INPUT);
  pinMode(BUZZ_Pin,OUTPUT);
  pinMode(GLED_Pin,OUTPUT);
  pinMode(RLED_Pin,OUTPUT);

}

void loop()
{
  
  //Clearing the trigger pin
  digitalWrite(TRIG_Pin,LOW);
  delayMicroseconds(2);
  
  /*Activating trigger pin for 10ms
    to send sonic burst            */
  digitalWrite(TRIG_Pin,HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_Pin,LOW);
  
  /*Reading echo pin and assigning 
    sound wave travel time(s)     */
  TIME = pulseIn(ECHO_Pin,HIGH);
  
  //Calculating and displaying distance in cm
  DIST_CM = TIME * SOUND_VELOCITY/2;
  Serial.print("DISTANCE(cm) = ");
  Serial.println(DIST_CM);
  delay(100);
  
  /*Checking distance and triggering 
  buzzer and LED accordingly        */
  if(DIST_CM < 50)
  {
    tone(BUZZ_Pin,1000); //Buzzer ON
    digitalWrite(RLED_Pin,HIGH); //Red LED ON
    digitalWrite(GLED_Pin,LOW); //Green LED OFF
  }
  else
  {
    digitalWrite(GLED_Pin,HIGH); //Green LED
    digitalWrite(RLED_Pin,LOW); //Red LED OFF
    noTone(BUZZ_Pin);
  }
  
    //Mapping vibration with distance 
  if(DIST_CM >=3 && DIST_CM <=100)
    VIB = map(DIST_CM,3,100,100,0);
  else
  {
    if(DIST_CM < 3)
      VIB = 100;
    else if(DIST_CM > 100)
      VIB = 0;
  }
  
  //Printing vibration percentage
  Serial.print("Vibration = ");
  Serial.print(VIB);
  Serial.println('%');
}
