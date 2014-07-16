#Piezo Knock Sensor
A piezo disc works when an electric field (voltage) is applied across ceramic material in the disc, causing it to change shape and therefore make a sound (a click). The disc also works in reverse in that when the disc is knocked or squeezed, the force on the material causes an electric charge (voltage) to be generated. We can read that current using the Arduino and we are going to do that now by making a knock sensor

####Parts  Required
Piezo Sounder (or piezo disc)  ![2 way screw terminal](https://cloud.githubusercontent.com/assets/3673943/3595813/725d5000-0cbb-11e4-9b60-9fb0738f5e16.jpg)

2-Way Screw Terminal  ![buzzer](https://cloud.githubusercontent.com/assets/3673943/3595814/725dc3c8-0cbb-11e4-9ec4-db32d3b1e2d8.jpg)

Red Diffused 5mm LED ![redLed](https://cloud.githubusercontent.com/assets/3673943/3584985/ebd67d26-0c20-11e4-8372-295e58ab7e80.jpg)

1MΩ Resistor	![1m resistor](https://cloud.githubusercontent.com/assets/3673943/3596020/80a18782-0cbe-11e4-849b-85c0ebcef049.jpg)

150Ω Current-Limiting Resistor   ![150ohms resistor](https://cloud.githubusercontent.com/assets/3673943/3596021/80bae89e-0cbe-11e4-862d-5b6cf24ef9f6.jpg)

####Connect It Up
First, make sure your Arduino is powered off by unplugging it from the USB cable. Then connect up your parts so that you have the circuit in image below. A piezo disc works better for this project than a piezo sounder. The resistor on the piezo is there to discharge any piezo capacitance built up in the piezo while it is being used.

![circuit - knocker](https://cloud.githubusercontent.com/assets/3673943/3596039/b748875e-0cbe-11e4-9cec-46372847d393.jpg)

####Enter the Code
Open up your Arduino IDE and type in the following code:

```c
//Piezo Knock Sensor

int ledPin = 9; // LED on Digital Pin 9
int piezoPin = 5; // Piezo on Analog Pin 5
int threshold = 120; // The sensor value to reach before activation
int sensorValue = 0;  // A variable to store the value read from the sensor
float ledValue = 0; // The brightness of the LED

void setup() {
        pinMode(ledPin, OUTPUT); // Set the ledPin to an OUTPUT
        // Flash the LED twice to show the program has started
        digitalWrite(ledPin, HIGH); delay(150); digitalWrite(ledPin, LOW); delay(150);
        digitalWrite(ledPin, HIGH); delay(150); digitalWrite(ledPin, LOW); delay(150);
}

void loop() {
        sensorValue = analogRead(piezoPin);  // Read the value from the sensor

        if (sensorValue >= threshold) { // If knock detected set brightness to max
                ledValue = 255;
        }
        analogWrite(ledPin, int(ledValue) ); // Write brightness value to LED
        ledValue = ledValue - 0.05; // Dim the LED slowly
        if (ledValue <= 0) { ledValue = 0;}  // Make sure value does not go below zero
}
```
After you have uploaded your code, the LED will flash quickly twice to show you that the program has started. You can now knock the sensor (place it flat on a surface first) or squeeze it between your fingers. Every time the Arduino detects a knock or squeeze, the LED will light up and then gently fade back down to off. The threshold value in the code was set for the piezo disc I used when building the project. You may need to set this to a higher or lower value depending on the type and size of piezo you have used for your project. Lower is more sensitive and higher is less

####Code Overview
We haven’t learnt any new code commands in this project, but we will go over how it works anyway.
First, we set up the necessary variables for our program. These are self-explanatory.
```c
int ledPin = 9; // LED on Digital Pin 9
int piezoPin = 5; // Piezo on Analog Pin 5
int threshold = 120; // The sensor value to reach before activation
int sensorValue = 0;  // A variable to store the value read from the sensor
float ledValue = 0; // The brightness of the LED
```
In the setup function, the ledPin is set to an output and the LED is flashed quickly twice as a visual indication that the program has started working.
```c
void setup() {
        pinMode(ledPin, OUTPUT);
        digitalWrite(ledPin, HIGH); delay(150); digitalWrite(ledPin, LOW); delay(150);
        digitalWrite(ledPin, HIGH); delay(150); digitalWrite(ledPin, LOW); delay(150);
}
```
In our main loop, we first read the analog value from pin 5, which the piezo is attached to
```c
sensorValue = analogRead(piezoPin);
```
Then the code checks if that value is greater than or equal to (>=) the threshold we have set, i.e., to determine that it really is a knock or squeeze (the piezo is very sensitive as you can see if you set the threshold to a very low value). If it is, then it sets ledValue to 255, which is the maximum voltage out of PWM pin 9.
```c
if (sensorValue >= threshold) {
        ledValue = 255;
}
```
We then write that value to PWM pin 9. As ledValue is a float, we “cast” it to an integer, as the analogWrite function can only accept an integer and not a floating value. (Casting simply means “to convert to a different data type.”)
```c
analogWrite(ledPin, int(ledValue) );
```
We then reduce the value of ledValue, which is a float, by 0.05.
```c
ledValue = ledValue - 0.05;
```
We want the LED to dim gently, and hence, we use a float to store the brightness value of the LED instead of an integer. That way, we can reduce its value by a small amount (in our case 0.05), meaning it will take a little while as the main loop repeats for the value of ledValue to reach zero, when the LED will be at its dimmest and off. If you want the LED to dim slower or faster, then increase or decrease this value.

Finally, we don’t want ledValue to go below zero as PWM pin 9 can only output a value from 0 to 255, so we check if it is smaller, or equal to zero, and if it is smaller, we change it back to zero.
```c
if (ledValue <= 0) { ledValue = 0;}
```
The main loop then repeats, dimming the LED slightly each time until the LED goes off, or another knock is detected, and the brightness is set back to maximum.














