Overview:
this project which i made consists of a basic prototype of a stick which is designed for visually impared people. I have used an aurdino and ultrasonic sensors and other such basic tools to create this. It basically detects obstacles nearby within 55cm range and has such more features.
Altough this project is very basic i had much more ideas for it but due to time constraint and lack of funds for components i coudnt finish them. We have coded the project working in Aurdino uno and uploaded in our aurdino.
It was a team project for a college organised prodathon done in my 1st semester of my engineering course.

Features:
this stick can detect objects upto 300cm away from its sensor and it gets alert and activates the buzzer, as it comes closer the buzzer sound intensity gets higher, we have added an LED for the other person to know if they r too close(if the person is the obstacle here).
It is an easy to hold light weight stick aswell 

Components used:
HC-SR04 sensor
Aurdino Uno
LED
Resistor (2kohm)
9v battery
Passive Buzzer
Breadboard

![Smart Stick Circuit](https://github.com/user-attachments/assets/ef5d4cf9-78f4-43ed-91a5-9fbed155210d)

Working:
as it is an obstacle detecting stick the range of the ultra sonic sensor is 300cm,if any object comes within the range of 300cm the sensor will find out,but the settings done by us are only producing alarms if the distace is less than 60cm,the intensity starts increasing as the distance
closes by.
Based on how close the obstacle is, the Arduino activates the buzzer and LED as alerts.

🔹 Far obstacle → slow vibration / blinking

🔹 Medium distance → faster alerts

🔹 Very close obstacle → continuous buzzer and LED
The code runs in loop as long as there is power being provided to the aurdino circuit.
Power On → Sensor Reads Distance → Is Obstacle Close?
                         ↓
                 YES → Alert via Buzzer + LED
                         ↓
                  Loop Continues

Code:
cpp
#include <SoftwareSerial.h>

const int trigPin = 9;    
const int echoPin = 10;   
const int buzzerPin = 11; 
const int ledPin = 13;    
const int alertDistance = 60;

const int motorPin = 8;        
SoftwareSerial btSerial(2, 3);

void setup() {
  Serial.begin(9600);
  Serial.println("Smart Stick Booting Up...");
  btSerial.begin(9600);

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(buzzerPin, OUTPUT);
  pinMode(ledPin, OUTPUT);
  pinMode(motorPin, OUTPUT);
}

void loop() {
  checkBluetoothCommands();
  checkSerialCommands();
  checkObstacles();
}

void checkBluetoothCommands() {
  if (btSerial.available() > 0) {
    char command = btSerial.read();
    handleCommand(command);
  }
}

void checkSerialCommands() {
  if (Serial.available() > 0) {
    char command = Serial.read();
    handleCommand(command);
  }
}

void handleCommand(char command) {
  switch (command) {
    case 'L':
      vibratePattern(2);
      break;
    case 'R':
      vibratePattern(3);
      break;
    case 'F':
      vibratePattern(1);
      break;
  }
}

void checkObstacles() {
  long duration;
  double distance;

  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = (duration * 0.0343) / 2.0;

  Serial.print("Distance: ");
  Serial.println(distance);

  if (distance > 0 && distance < alertDistance) {
    digitalWrite(ledPin, HIGH);

    int beepDelay;
    if (distance < 10) beepDelay = 50;
    else if (distance < 20) beepDelay = 100;
    else if (distance < 30) beepDelay = 200;
    else if (distance < 40) beepDelay = 300;
    else beepDelay = 400;

    tone(buzzerPin, 1000);
    delay(beepDelay);
    noTone(buzzerPin);
    delay(beepDelay);

  } else {
    digitalWrite(ledPin, LOW);
    noTone(buzzerPin);
  }
}

void vibratePattern(int times) {
  if (times == 1) {
    digitalWrite(motorPin, HIGH);
    delay(400);
    digitalWrite(motorPin, LOW);
  } else {
    for (int i = 0; i < times; i++) {
      digitalWrite(motorPin, HIGH);
      delay(150);
      digitalWrite(motorPin, LOW);
      delay(100);
    }
  }
  delay(200);
}


Prototype Image:
<img width="667" height="901" alt="image" src="https://github.com/user-attachments/assets/e222b75c-4031-4039-8e45-962b10f1fbcf" />

Future Improvements:
We can add navigation system which will help the user to reach a location by themselves.
Feedback system will help modify the stick for a specific user.
Connection and GPS of the user to the closest relatives and other family members.
AI voice assistant

My contribution:
I have designed the circuit and the working of this prototype.
Wrote the code using AI.
Made the stick physically and connected them.
Spoke about the working of the stick in the presentation.
