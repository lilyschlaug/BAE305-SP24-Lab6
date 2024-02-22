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
2. Use a ruler to gain understanding of the resolution of the sensing system. Move the object by a millimeter and check the serial monitor for accuracy.     

Part 2 - Move It    
1. Write a program to control the movement of both motors. Commands should be for 3 different speeds. A code was downloaded and edited to fit our desired outcome. 

# Test Results
Part 1 - Learn to Listen   
- sensing system has resolution of 100th centimeters.
- It is not very precise based on the based on the procedure with a ruler. 
- The minimum time of flight is 4.08 mm and minimum distance we can detect is 2.04 mm. 

# Discussion
Part 2 - Move It    
What is the minimum speed number for the motors to move forward?   
- Minimum to move : 35 

Did you make any design decisions that had an impact on the results? How did they impact the results? What do the results mean?

# Conclusion

#Refrences 

