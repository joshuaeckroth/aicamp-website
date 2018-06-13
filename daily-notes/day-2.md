---
title: Day 2 Notes
layout: note
---

# Day 2 Notes

## Robot code

```
/*
  SparkFun Inventor’s Kit
  Circuit 5C - Autonomous Robot

  This robot will drive around on its own and react to obstacles by backing up and turning to a new direction.
  This sketch was adapted from one of the activities in the SparkFun Guide to Arduino.
  Check out the rest of the book at
  https://www.sparkfun.com/products/14326

  This sketch was written by SparkFun Electronics, with lots of help from the Arduino community.
  This code is completely free for any use.

  View circuit diagram and instructions at: https://learn.sparkfun.com/tutorials/sparkfun-inventors-kit-experiment-guide---v40
  Download drawings and code at: https://github.com/sparkfun/SIK-Guide-Code
*/



//the right motor will be controlled by the motor A pins on the motor driver
const int AIN1 = 13;           //control pin 1 on the motor driver for the right motor
const int AIN2 = 12;            //control pin 2 on the motor driver for the right motor
const int PWMA = 11;            //speed control pin on the motor driver for the right motor

//the left motor will be controlled by the motor B pins on the motor driver
const int PWMB = 10;           //speed control pin on the motor driver for the left motor
const int BIN2 = 9;           //control pin 2 on the motor driver for the left motor
const int BIN1 = 8;           //control pin 1 on the motor driver for the left motor


//distance variables
const int trigPin = 3;
const int echoPin = 4;

int switchPin = 7;             //switch to turn the robot on and off

float distance = 0;            //variable to store the distance measured by the distance sensor

//robot behaviour variables
int backupTime = 300;           //amount of time that the robot will back up when it senses an object
int turnTime = 200;             //amount that the robot will turn once it has backed up

/********************************************************************************/
void setup()
{
  pinMode(trigPin, OUTPUT);       //this pin will send ultrasonic pulses out from the distance sensor
  pinMode(echoPin, INPUT);        //this pin will sense when the pulses reflect back to the distance sensor

  pinMode(switchPin, INPUT_PULLUP);   //set this as a pullup to sense whether the switch is flipped


  //set the motor contro pins as outputs
  pinMode(AIN1, OUTPUT);
  pinMode(AIN2, OUTPUT);
  pinMode(PWMA, OUTPUT);

  pinMode(BIN1, OUTPUT);
  pinMode(BIN2, OUTPUT);
  pinMode(PWMB, OUTPUT);

  Serial.begin(9600);                       //begin serial communication with the computer
  Serial.print("To infinity and beyond!");  //test the serial connection
}

/********************************************************************************/
void loop()
{
  int timeMoving = 0;
  while (timeMoving < 18)
  {
    if (digitalRead(switchPin) == LOW) {
      distance = getAvgDistance();
      while (distance < 0) {
        distance = getAvgDistance();
      }
      Serial.println(distance);
      if (distance > 10.0) {
        rightMotor(255);
        leftMotor(255);
        delay(1000);
        rightMotor(0);
        leftMotor(0);
        timeMoving++;
      }
    }
  }
  rightMotor(255);
  leftMotor(-255);
  delay(280);
  rightMotor(0);
  leftMotor(0);
  timeMoving = 0;
  while (timeMoving < 7)
  {
    if (digitalRead(switchPin) == LOW) {
      distance = getAvgDistance();
      while (distance < 0) {
        distance = getAvgDistance();
      }
      Serial.println(distance);
      if (distance > 10.0) {
        rightMotor(255);
        leftMotor(255);
        delay(1000);
        rightMotor(0);
        leftMotor(0);
        timeMoving++;
      }
    }
  }
  rightMotor(-255);
  leftMotor(255);
  delay(280);
  rightMotor(0);
  leftMotor(0);
  if (distance > 10.0) {
    rightMotor(255);
    leftMotor(255);
    delay(1000);
    rightMotor(0);
    leftMotor(0);
    timeMoving++;
  }
  delay(1000000000);
}


float getAvgDistance()
{
  float sumDistances = 0.0;
  int countDistances = 0;
  float d;
  for (int i = 0; i < 20; i++)
  {
    d = getDistance();
    if (d > 0.3)
    {
      sumDistances += d;
      countDistances++;
    }
  }
  if (countDistances == 0)
  {
    return -1.0;
  }
  return sumDistances / countDistances;
}

/********************************************************************************/
void rightMotor(int motorSpeed)                       //function for driving the right motor
{
  if (motorSpeed > 0)                                 //if the motor should drive forward (positive speed)
  {
    digitalWrite(AIN1, HIGH);                         //set pin 1 to high
    digitalWrite(AIN2, LOW);                          //set pin 2 to low
  }
  else if (motorSpeed < 0)                            //if the motor should drive backwar (negative speed)
  {
    digitalWrite(AIN1, LOW);                          //set pin 1 to low
    digitalWrite(AIN2, HIGH);                         //set pin 2 to high
  }
  else                                                //if the motor should stop
  {
    digitalWrite(AIN1, LOW);                          //set pin 1 to low
    digitalWrite(AIN2, LOW);                          //set pin 2 to low
  }
  analogWrite(PWMA, abs(motorSpeed));                 //now that the motor direction is set, drive it at the entered speed
}

/********************************************************************************/
void leftMotor(int motorSpeed)                        //function for driving the left motor
{
  if (motorSpeed > 0)                                 //if the motor should drive forward (positive speed)
  {
    digitalWrite(BIN1, HIGH);                         //set pin 1 to high
    digitalWrite(BIN2, LOW);                          //set pin 2 to low
  }
  else if (motorSpeed < 0)                            //if the motor should drive backwar (negative speed)
  {
    digitalWrite(BIN1, LOW);                          //set pin 1 to low
    digitalWrite(BIN2, HIGH);                         //set pin 2 to high
  }
  else                                                //if the motor should stop
  {
    digitalWrite(BIN1, LOW);                          //set pin 1 to low
    digitalWrite(BIN2, LOW);                          //set pin 2 to low
  }
  analogWrite(PWMB, abs(motorSpeed));                 //now that the motor direction is set, drive it at the entered speed
}

/********************************************************************************/
//RETURNS THE DISTANCE MEASURED BY THE HC-SR04 DISTANCE SENSOR
float getDistance()
{
  float echoTime;                   //variable to store the time it takes for a ping to bounce off an object
  float calcualtedDistance;         //variable to store the distance calculated from the echo time

  //send out an ultrasonic pulse that's 10ms long
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  echoTime = pulseIn(echoPin, HIGH);      //use the pulsein command to see how long it takes for the
  //pulse to bounce back to the sensor

  calcualtedDistance = echoTime / 148.0;  //calculate the distance of the object that reflected the pulse (half the bounce time multiplied by the speed of sound)

  return calcualtedDistance;              //send back the distance that was calculated
}
```

## Robot code for light following

```
/*
  SparkFun Inventor’s Kit
  Circuit 5C - Autonomous Robot

  This robot will drive around on its own and react to obstacles by backing up and turning to a new direction.
  This sketch was adapted from one of the activities in the SparkFun Guide to Arduino.
  Check out the rest of the book at
  https://www.sparkfun.com/products/14326

  This sketch was written by SparkFun Electronics, with lots of help from the Arduino community.
  This code is completely free for any use.

  View circuit diagram and instructions at: https://learn.sparkfun.com/tutorials/sparkfun-inventors-kit-experiment-guide---v40
  Download drawings and code at: https://github.com/sparkfun/SIK-Guide-Code
*/



//the right motor will be controlled by the motor A pins on the motor driver
const int AIN1 = 13;           //control pin 1 on the motor driver for the right motor
const int AIN2 = 12;            //control pin 2 on the motor driver for the right motor
const int PWMA = 11;            //speed control pin on the motor driver for the right motor

//the left motor will be controlled by the motor B pins on the motor driver
const int PWMB = 10;           //speed control pin on the motor driver for the left motor
const int BIN2 = 9;           //control pin 2 on the motor driver for the left motor
const int BIN1 = 8;           //control pin 1 on the motor driver for the left motor

int photoresistor = 0;

int angle = 0;
int delta = 0;
int k;
const int MOTOR_SLOW_LEFT = 255;
const int MOTOR_SLOW_RIGHT = 255;
const int MOTOR_ANGLE_DELAY = 80;

//distance variables
const int trigPin = 3;
const int echoPin = 4;

int switchPin = 7;             //switch to turn the robot on and off

float distance = 0;            //variable to store the distance measured by the distance sensor

//robot behaviour variables
int backupTime = 300;           //amount of time that the robot will back up when it senses an object
int turnTime = 200;             //amount that the robot will turn once it has backed up

/********************************************************************************/
void setup()
{
  pinMode(trigPin, OUTPUT);       //this pin will send ultrasonic pulses out from the distance sensor
  pinMode(echoPin, INPUT);        //this pin will sense when the pulses reflect back to the distance sensor

  pinMode(switchPin, INPUT_PULLUP);   //set this as a pullup to sense whether the switch is flipped


  //set the motor contro pins as outputs
  pinMode(AIN1, OUTPUT);
  pinMode(AIN2, OUTPUT);
  pinMode(PWMA, OUTPUT);

  pinMode(BIN1, OUTPUT);
  pinMode(BIN2, OUTPUT);
  pinMode(PWMB, OUTPUT);

  Serial.begin(9600);                       //begin serial communication with the computer
  Serial.print("To infinity and beyond!");  //test the serial connection
}

/********************************************************************************/
void loop()
{
  if (digitalRead(switchPin) == LOW) {
    angle = 0;
    int max_light = 0;
    int max_angle = 0;
    for (int i = 0; i < 12; i++)
    {
      // make a turn
      turn_to(60*i);
      // sense light
      photoresistor = analogRead(A0);
      if (photoresistor > max_light) {
        max_light = photoresistor;
        max_angle = 60*i;
      }
    }
    if (max_light > 0)
    {
      turn_to(max_angle);
      // move forward
      rightMotor(255);
      leftMotor(255);
      delay(1000);
      rightMotor(0);
      leftMotor(0);
      Serial.print("Light: ");
      Serial.println(photoresistor);
    }
  }
  else
  {
    rightMotor(0);
    leftMotor(0);
  }
}

void turn_to(int new_angle)
{
  // 1 delta = 90/4 degrees = 22.5 degrees
  delta = new_angle - angle;
  if (delta > 0) // turn right
  {
    for (k = 0; k < (delta*4)/90; k++)
    {
      motors(MOTOR_SLOW_LEFT, -MOTOR_SLOW_RIGHT);
      delay(MOTOR_ANGLE_DELAY);
      motors(0, 0);
      delay(100);
    }
  }
  else if (delta < 0) // turn left
  {
    for (k = 0; k < -(delta*4)/90; k++)
    {
      motors(-MOTOR_SLOW_LEFT, MOTOR_SLOW_RIGHT);
      delay(MOTOR_ANGLE_DELAY);
      motors(0, 0);
      delay(100);
    }
  }
  angle = new_angle;
}



float getAvgDistance()
{
  float sumDistances = 0.0;
  int countDistances = 0;
  float d;
  for (int i = 0; i < 20; i++)
  {
    d = getDistance();
    if (d > 0.3)
    {
      sumDistances += d;
      countDistances++;
    }
  }
  if (countDistances == 0)
  {
    return -1.0;
  }
  return sumDistances / countDistances;
}

/********************************************************************************/
void rightMotor(int motorSpeed)                       //function for driving the right motor
{
  if (motorSpeed > 0)                                 //if the motor should drive forward (positive speed)
  {
    digitalWrite(AIN1, HIGH);                         //set pin 1 to high
    digitalWrite(AIN2, LOW);                          //set pin 2 to low
  }
  else if (motorSpeed < 0)                            //if the motor should drive backwar (negative speed)
  {
    digitalWrite(AIN1, LOW);                          //set pin 1 to low
    digitalWrite(AIN2, HIGH);                         //set pin 2 to high
  }
  else                                                //if the motor should stop
  {
    digitalWrite(AIN1, LOW);                          //set pin 1 to low
    digitalWrite(AIN2, LOW);                          //set pin 2 to low
  }
  analogWrite(PWMA, abs(motorSpeed));                 //now that the motor direction is set, drive it at the entered speed
}

/********************************************************************************/
void leftMotor(int motorSpeed)                        //function for driving the left motor
{
  if (motorSpeed > 0)                                 //if the motor should drive forward (positive speed)
  {
    digitalWrite(BIN1, HIGH);                         //set pin 1 to high
    digitalWrite(BIN2, LOW);                          //set pin 2 to low
  }
  else if (motorSpeed < 0)                            //if the motor should drive backwar (negative speed)
  {
    digitalWrite(BIN1, LOW);                          //set pin 1 to low
    digitalWrite(BIN2, HIGH);                         //set pin 2 to high
  }
  else                                                //if the motor should stop
  {
    digitalWrite(BIN1, LOW);                          //set pin 1 to low
    digitalWrite(BIN2, LOW);                          //set pin 2 to low
  }
  analogWrite(PWMB, abs(motorSpeed));                 //now that the motor direction is set, drive it at the entered speed
}

/********************************************************************************/
//RETURNS THE DISTANCE MEASURED BY THE HC-SR04 DISTANCE SENSOR
float getDistance()
{
  float echoTime;                   //variable to store the time it takes for a ping to bounce off an object
  float calcualtedDistance;         //variable to store the distance calculated from the echo time

  //send out an ultrasonic pulse that's 10ms long
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  echoTime = pulseIn(echoPin, HIGH);      //use the pulsein command to see how long it takes for the
  //pulse to bounce back to the sensor

  calcualtedDistance = echoTime / 148.0;  //calculate the distance of the object that reflected the pulse (half the bounce time multiplied by the speed of sound)

  return calcualtedDistance;              //send back the distance that was calculated
}

void motors(int leftMotorSpeed, int rightMotorSpeed)
{
  if (digitalRead(switchPin) == LOW) { //if the on switch is flipped
    if (rightMotorSpeed > 0)                                 //if the motor should drive forward (positive speed)
    {
      digitalWrite(AIN1, HIGH);                         //set pin 1 to high
      digitalWrite(AIN2, LOW);                          //set pin 2 to low
    }
    else if (rightMotorSpeed < 0)                            //if the motor should drive backwar (negative speed)
    {
      digitalWrite(AIN1, LOW);                          //set pin 1 to low
      digitalWrite(AIN2, HIGH);                         //set pin 2 to high
    }
    else                                                //if the motor should stop
    {
      digitalWrite(AIN1, LOW);                          //set pin 1 to low
      digitalWrite(AIN2, LOW);                          //set pin 2 to low
    }

    if (leftMotorSpeed > 0)                                 //if the motor should drive forward (positive speed)
    {
      digitalWrite(BIN1, HIGH);                         //set pin 1 to high
      digitalWrite(BIN2, LOW);                          //set pin 2 to low
    }
    else if (leftMotorSpeed < 0)                            //if the motor should drive backwar (negative speed)
    {
      digitalWrite(BIN1, LOW);                          //set pin 1 to low
      digitalWrite(BIN2, HIGH);                         //set pin 2 to high
    }
    else                                                //if the motor should stop
    {
      digitalWrite(BIN1, LOW);                          //set pin 1 to low
      digitalWrite(BIN2, LOW);                          //set pin 2 to low
    }

    analogWrite(PWMB, abs(leftMotorSpeed));                 //now that the motor direction is set, drive it at the entered speed
    analogWrite(PWMA, abs(rightMotorSpeed));                 //now that the motor direction is set, drive it at the entered speed
  }
  else
  {
    digitalWrite(AIN1, LOW);
    digitalWrite(AIN2, LOW);
    digitalWrite(BIN1, LOW);
    digitalWrite(BIN2, LOW);
  }
}

```








