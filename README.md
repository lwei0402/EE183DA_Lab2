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

-Code and explanation:
//include libraries that we are going to use
\#include <ESP8266WiFi.h>
\#include <Servo.h> 
 
Servo myservo; 
//knock function to control delay of the knock
void knock(int d)
{
  myservo.write(120);
  delay(d);
  myservo.write(80);
  delay(d);
}
WiFiServer server(80); //Initialize the server on Port 80
const short int LED_PIN = D4;//GPIO16

void setup() 
{
  WiFi.mode(WIFI_AP); //Our ESP8266-12E is an AccessPoint
  WiFi.softAP("Likai", "lwei0402"); // Set your own said and password
  server.begin(); // Start the HTTP Server
         //Looking under the hood
         Serial.begin(115200);
         IPAddress HTTPS_ServerIP= WiFi.softAPIP(); // Obtain the IP of the Server
         Serial.print("Server IP is: "); // Print the IP to the monitor window
         Serial.println(HTTPS_ServerIP);
         pinMode(LED_PIN, OUTPUT); //GPIO16 is an OUTPUT pin;
         digitalWrite(LED_PIN, LOW); //LED Initial state is ON
         myservo.attach(D2); // Set up Servo Pin
}

void loop() 
{  
         WiFiClient client = server.available();
         if (!client) {
               return; }
         //Looking under the hood
         Serial.println("Somebody has connected :)");
         //Read what the browser has sent into a String class
  
      //and print the request to the monitor
         String request = client.readStringUntil('\r');
         //Looking under the hood
         Serial.println(request);
         // Handle the Request
         if (request.indexOf("/OFF") != -1)
         {
	//Play song1 if button 1 has been pressed 
           digitalWrite(LED_PIN, HIGH);
             knock(200);
             knock(200);
             knock(200);
             knock(200);
             knock(600);
             knock(200);
             digitalWrite(LED_PIN, LOW);
         }          
         else if (request.indexOf("/ON") != -1){
	//Play song2 if button 2 has been pressed
            digitalWrite(LED_PIN, HIGH);
            knock(400);
            knock(400);
            knock(200);
            knock(200);
            knock(600);
           digitalWrite(LED_PIN, LOW);
         }
         // Prepare the HTML document to respond and add buttons:
         String s = "HTTP/1.1 200 OK\r\n";
         s += "Content-Type: text/html\r\n\r\n";
         s += "<!DOCTYPE HTML>\r\n<html>\r\n";
         s += "<br><input type=\"button\" name=\"b1\" value=\"Song 1\"";
         s += " onclick=\"location.href='/ON'\">";
         s += "<br><br><br>";
         s += "<br><input type=\"button\" name=\"b1\" value=\"Song 2\"";
         s += " onclick=\"location.href='/OFF'\">";
         s += "</html>\n";
         //Serve the HTML document to the browser.
         client.flush(); //clear previous info in the stream
         client.print(s); // Send the response to the client
         delay(1);
         Serial.println("Client disonnected"); //Looking under the hood
}

-Future Modification
Since we only have one regular servo in this project, we can try to add more servos in the future. The sound will have different frequency is the amount of water in the cup is different, therefore we can have music with different tones. Furthermore, if we have more knowledge about HTML, we can definitely make the webpage for controlling fancier.
