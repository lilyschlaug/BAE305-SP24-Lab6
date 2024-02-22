# Lab 6 -  Moving with Purpose: Sensing and Mobility for Basic Robot Applications
# By: Lily Schlaug and Noah Lane
# Summary
This lab involved controlling the movement of robot wheels by developing a program to communicate instructions. An H-bridge was used to operate the motor. This setup allows us to interface the motor and control both direction and speed of motor rotation with minimal connections. This lab also introduced an ultrasonic sensor. This is a common sensor to use for object detection and measuring distances. The ultrasonic sensor was used to create a collision prevention measure, where the motor shuts off if an object gets within a certain distance.  

# Materials
1 x Computer running Arduino IDE   
SparkFun Inventor’s kit:   
1 x RedBoard   
1 x Ultrasonic sensor   
2 x Motors   
1 x Motor Driver   

# Assembly Procedures
Part 1 Learn to listen   
1. Connect the ultrasonic sensor to the breadboard
2. Connect VCC to 5V, trig to digital pin 11, echo to digital pin 12, and GND to ground   
![image](https://github.com/npla225/BAE305-SP24-Lab6/assets/156371043/67e6dd8f-52ae-47c7-a226-8e5408f8299f)

**Figure 1. Schematic for part 1 ultrasonic sensor testing**
   
Part 2 Move it   
1. Connect the motors, H bridge motor control circuit and the switch to the breadboard
2. Wire the circuit as shown below in Figure 2
![image](https://github.com/npla225/BAE305-SP24-Lab6/assets/156371043/069c2648-5467-40fa-9448-06f6235e1100)

**Figure 2. Schematic for motor control circuit**
3. Integrate the circuit from Part 1 with the motor control circuit
![image](https://github.com/npla225/BAE305-SP24-Lab6/assets/156371043/500cadee-dd8f-41e2-91d6-46d39ec3cbfb)

**Figure 3. Schematic for integrated motor control and ultrasonic circuit**

# Test Equipment
No test equipment required. 

# Test Procedures
Part 1 - Learn to Listen    
1. Download code to read distance of an object to the sensor via serial communications.   
2. Use a ruler to gain understanding of the resolution of the sensing system. Move the object by a millimeter and check the serial monitor for accuracy.       
```/*
 * HC-SR04 example sketch
 *
 * https://create.arduino.cc/projecthub/Isaac100/getting-started-with-the-hc-sr04-ultrasonic-sensor-036380
 *
 * by Isaac100
 */

const int trigPin = 11;
const int echoPin = 12;

float duration, distance;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  Serial.begin(9600);
}

void loop() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = (duration*.0343)/2;
  Serial.print("Distance: ");
  Serial.println(distance);
  delay(100);
}
``` 

Part 2 - Move It    
1. Write a program to control the movement of both motors. Commands should be for 3 different speeds. A code was downloaded and edited to fit our desired outcome.    
2. Using the code below, test the program to make the motors turn left, right or go backwards at the entered speed. 
3. Program the code to make the motors stop when an object gets within 10 cm of the sensor. 

```/*
  SparkFun Inventor’s Kit
  Circuit 5B - Remote Control Robot

  Control a two wheeled robot by sending direction commands through the serial monitor.
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

int switchPin = 7;             //switch to turn the robot on and off

const int driveTime = 20;      //this is the number of milliseconds that it takes the robot to drive 1 inch
                               //it is set so that if you tell the robot to drive forward 25 units, the robot drives about 25 inches

//const int turnTime = 8;        //this is the number of milliseconds that it takes to turn the robot 1 degree
                               //it is set so that if you tell the robot to turn right 90 units, the robot turns about 90 degrees

                               //Note: these numbers will vary a little bit based on how you mount your motors, the friction of the
                               //surface that your driving on, and fluctuations in the power to the motors.
                               //You can change the driveTime and turnTime to make them more accurate

String botDirection;           //the direction that the robot will drive in (this change which direction the two motors spin in)
String distance;               //the distance to travel in each direction
String speed; 
int speedInt;
float duration, Distance;
const int trigPin = 6;
const int echoPin = 7;

/********************************************************************************/
void setup()
{
  pinMode(switchPin, INPUT_PULLUP);   //set this as a pullup to sense whether the switch is flipped

  //set the motor control pins as outputs
  pinMode(AIN1, OUTPUT);
  pinMode(AIN2, OUTPUT);
  pinMode(PWMA, OUTPUT);

  pinMode(BIN1, OUTPUT);
  pinMode(BIN2, OUTPUT);
  pinMode(PWMB, OUTPUT);

  Serial.begin(9600);           //begin serial communication with the computer

  //prompt the user to enter a command
  Serial.println("Enter a direction followed by a distance.");
  Serial.println("f = forward, b = backward, r = turn right, l = turn left");
  Serial.println("Example command: f 50");

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
}

/********************************************************************************/
void loop()
{
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  Distance = (duration*.0343)/2;
  Serial.print("Distance: ");
  Serial.println(Distance);
  delay(100);

  if (digitalRead(7) == LOW)
  {   
    if (Distance > 10)
    {                                                  //if the switch is in the ON position
      if (Serial.available() > 0)                         //if the user has sent a command to the RedBoard
      {
        botDirection = Serial.readStringUntil(' ');       //read the characters in the command until you reach the first space
        speed = Serial.readStringUntil(' ');           //read the characters in the command until you reach the second space

        //print the command that was just received in the serial monitor
        Serial.print(botDirection);
        Serial.print(" ");
        Serial.println(distance.toInt());

        if (botDirection == "f")                         //if the entered direction is forward
        {
          speedInt = speed.toInt();
          rightMotor(speedInt);                                //drive the right wheel forward
          leftMotor(speedInt);                                 //drive the left wheel forward


          //delay(10);            //drive the motors long enough travel the entered distance
          //rightMotor(0);                                  //turn the right motor off
          //leftMotor(0);                                   //turn the left motor off
        }
        else if (botDirection == "b")                    //if the entered direction is backward
        {
          speedInt = speed.toInt();
          rightMotor(-speedInt);                               //drive the right wheel forward
          leftMotor(-speedInt);                                //drive the left wheel forward
          //delay(driveTime * distance.toInt());            //drive the motors long enough travel the entered distance
          //rightMotor(0);                                  //turn the right motor off
          //leftMotor(0);                                   //turn the left motor off
        }
        else if (botDirection == "r")                     //if the entered direction is right
        {
          rightMotor(-speedInt);                               //drive the right wheel forward
          leftMotor(speedInt);                                 //drive the left wheel forward
          //delay(turnTime * distance.toInt());             //drive the motors long enough turn the entered distance
          //rightMotor(0);                                  //turn the right motor off
          //leftMotor(0);                                   //turn the left motor off
        }
        else if (botDirection == "l")                   //if the entered direction is left
        {
          rightMotor(speedInt);                                //drive the right wheel forward
          leftMotor(-speedInt);                                //drive the left wheel forward
          //delay(turnTime * distance.toInt());             //drive the motors long enough turn the entered distance
          //rightMotor(0);                                  //turn the right motor off
          //leftMotor(0);                                   //turn the left motor off
        }
        else if (botDirection == "s")
        {
          rightMotor(0);                                  //turn the right motor off
          leftMotor(0);
        }
      }                                   //turn the left motor off
    }
    
    else
    {
      rightMotor(0);                                  //turn the right motor off
      leftMotor(0);                                   //turn the left motor off
    }
  }
  else
  {
    rightMotor(0);                                  //turn the right motor off
    leftMotor(0);                                   //turn the left motor off
  }
}
/********************************************************************************/
void rightMotor(int motorSpeed)                       //function for driving the right motor
{
  if (motorSpeed > 0)                                 //if the motor should drive forward (positive speed)
  {
    digitalWrite(AIN1, HIGH);                         //set pin 1 to high
    digitalWrite(AIN2, LOW);                          //set pin 2 to low
  }
  else if (motorSpeed < 0)                            //if the motor should drive backward (negative speed)
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
  else if (motorSpeed < 0)                            //if the motor should drive backward (negative speed)
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
```

# Test Results
Part 1 - Learn to Listen   
- sensing system has resolution of 100th centimeters.
- It is not very precise based on the based on the procedure with a ruler. 
- The minimum time of flight is 4.08 mm and minimum distance we can detect is 2.04 mm. 

# Discussion
Part 2 - Move It    
What is the minimum speed number for the motors to move forward?   
- Minimum to move : 35 

# Conclusion
The purpose of this lab was to provide an introduction to the ultrasonic sensor. This was done working initially with just the ultrasonic sensor and collecting distance readings collected from the sensor and displayed in the command window. The next requirement of the lab was to learn how to use the H bridge circuit for motor control and the programming format for how to operate both motors at the same time. Three motor settings were then established in the program by modifying the original code from spark fun. The section of the lab required the integration of the ultrasonic sensor into the motor control code and use it to stop the motors after an object comes into a specific range from the ultrasonic sensor. The distance value for the ultrasonic sensor had to be measured beforehand and stored before it could be inserted into the program. 
 
# Refrences 
Figures 1-3

JOEL_E_B. “Sparkfun Inventor’s Kit Experiment Guide - v4.0.” SparkFun Inventor’s Kit Experiment Guide - v4.0 - SparkFun Learn, learn.sparkfun.com/tutorials/sparkfun-inventors-kit-experiment-guide---v40/circuit-5c-autonomous-robot. Accessed 22 Feb. 2024. 
