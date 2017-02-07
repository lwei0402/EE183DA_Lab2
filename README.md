# EE183DA_Lab2
Project Knock Knock
This tutorial will show you how to build a musical instrument by using an ESP8266 MCU. The sound is produced by knocking a glass cup. 

-Youtube video of Project Knock Knock:
The servo:
https://youtu.be/X764AlUlYuk
The web server:
https://youtu.be/ScRqmYlt5qY

-Bill of Materials:
ESP8266 MCU
NodeMCU Motor Shield
Micro-USB Cable
Servo Motor
Chopsticks
Tape
Glass Cup
Power Supply or Laptop

-Hardware Setup:

The hardware setup for this system is pretty simple and straight forward:
1-Put MCU on top of the motor shield (Be aware of direction of the MCU)
2-Tape chopstick onto the Servo
3-Connect servo to the motor shield(Pay attention to match the pins: Black:GND, Red:Power and Orange:PWM)
4-Connect MCU to power supply or laptop

-Design Process
	First of all, I decided to make a musical instrument by knocking glass cup with water. After testing the regular servo and continuous servo, I found that regular servo was good for rotate to certain angle. Unlike the continuous servo which can only keep rotating with various speed, the regular servo is better for implementing the knock motion. Since the arm of the servo itself is too short to knock the cup, I taped a chopstick to it make the arm longer.
	For user interfacing, I chose to use a http server hosted by the ESP8266 MCU’s built-in Wifi module. I chose it over other web hosting services because it’s easy to implement and the connection is stable enough for this project.
	For controlling the knock motion, I simply set the servo to rotate to certain angle whenever I wanted to knock the glass. The position will be reset after the motion. I implemented two different songs by changing the delay between each knocks. I also wrote a knock(delay time) function in the code to make controlling the timing easier, instead of writing bunch of servo.write() and delay(). In addition, I programmed the on-board LED to turn on and off as the music is playing. 

-Calibrate the servo
	It will be helpful if you calibrate your servo before writing the code for it. The easiest way is to use the sweep example in the Arduino library(Make sure change the pin attached from 2 to D2 since Arduino has different pin convention). Then figure out the midpoint of the whole swing, and attach the chopstick to that position. 

-Code and explanations:



-Future Modification
Since we only have one regular servo in this project, we can try to add more servos in the future. The sound will have different frequency is the amount of water in the cup is different, therefore we can have music with different tones. Furthermore, if we have more knowledge about HTML, we can definitely make the webpage for controlling fancier.
