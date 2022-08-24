# FireFighterIoT
Implementation of Esp8266 to create a FireFighter IoT

## Materials

- 1 x Esp8266
- 1 x module L298N
- 4 x Motor DC
- 1 x voltage module DC
- 1 x 9v baterry (12v recommended)
- 1 x Relay module
- a lot of wires

## Diagram

![diagram](/fig/diagram.png)

## Require

This project needs a controller of blynk to drive the car and shoot the weapon like this:

![diagram](/fig/blynk_diagram.jpeg)

And you have to configure these datastreams in your blynk template:

![diagram](/fig/blynk_datastreams.png)

## Code

``` c++

// Include the library files
#define BLYNK_PRINT Serial
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>

// Define the motor pins
#define ENA 16
#define IN1 5
#define IN2 4
#define IN3 0
#define IN4 2
#define ENB 14

//Define weapon pins
#define activator 12 //relay pin

// Variables for the Blynk widget values
int x = 50;
int y = 50;
int Speed;
int Button = 0;

char auth[] = "uPsPkoyPVYvwfBz9SB4avUpVa7mfpn7e"; //Enter your Blynk auth token
char ssid[] = "INFINITUM64DC"; //Enter your WIFI name
char pass[] = "UQs3CTyPyg"; //Enter your WIFI passowrd


void setup() {
  Serial.begin(9600);
  //Set the motor pins as output pins
  pinMode(ENA, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(ENB, OUTPUT);

  //Set weapon pins as output
  pinMode(activator, OUTPUT);

  // Initialize the Blynk library
  Blynk.begin(auth, ssid, pass, "blynk.cloud", 80);
}

// Get the joystick values
BLYNK_WRITE(V0) {
  x = param[0].asInt();
}
// Get the joystick values
BLYNK_WRITE(V1) {
  y = param[0].asInt();
}
//Get the slider values
BLYNK_WRITE(V2) {
  Speed = param.asInt();
}

//get the button value
BLYNK_WRITE(V3){
    Button = param.asInt();
}
// Check these values using the IF condition
void smartcar() {
  if (y > 70) {
    carForward();
    Serial.println("carForward");
  } else if (y < 30) {
    carBackward();
    Serial.println("carBackward");
  } else if (x < 30) {
    carLeft();
    Serial.println("carLeft");
  } else if (x > 70) {
    carRight();
    Serial.println("carRight");
  } else if (x < 70 && x > 30 && y < 70 && y > 30) {
    carStop();
    Serial.println("carstop");
  }
}

void weapon(){
  if(Button == 1){
    digitalWrite(activator,HIGH);
  }else if(Button == 0){
    digitalWrite(activator,LOW);
  }
}


void loop() {
  Blynk.run();// Run the blynk function
  smartcar();// Call the main function
  weapon();//call the weapon function
}

/**************Motor movement functions*****************/
void carForward() {
  analogWrite(ENA, Speed);
  analogWrite(ENB, Speed);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}
void carBackward() {
  analogWrite(ENA, Speed);
  analogWrite(ENB, Speed);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}
void carLeft() {
  analogWrite(ENA, Speed);
  analogWrite(ENB, Speed);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}
void carRight() {
  analogWrite(ENA, Speed);
  analogWrite(ENB, Speed);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}
void carStop() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}
```

## Evidence

![diagram](/fig/evidence0.jpeg)
![diagram](/fig/evidence1.jpeg)
![diagram](/fig/evidence2.jpeg)

## Credits

Created By Marco Alejandro Hernandez Castellanos