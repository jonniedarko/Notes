#Interactive LED Chase Effect
Leave your circuit board as it was in Project 5. We are going to add a potentiometer to this circuit which will allow you to change the speed of the lights while the code is running.

###Parts Required
4.7KΩ Rotary Potentiometer	![rotary potentiometer](https://cloud.githubusercontent.com/assets/3673943/3585162/4b97ab48-0c23-11e4-85bf-788a73e6f093.jpg)

###Connect It Up
First, make sure your Arduino is powered off by unplugging it from the USB cable. Now add a potentiometer to the circuit so it is connected as in image with the left leg going to the 5V on the Arduino, the middle leg going to Analog Pin 2, and the right leg going to ground.

![circuit - interactive chase](https://cloud.githubusercontent.com/assets/3673943/3585176/93d33684-0c23-11e4-9215-565e01b93e94.jpg)

###Enter the Code
Open up your Arduino IDE and type in the following code
```c
// Create array for LED pins
byte ledPin[] = {4, 5, 6, 7, 8, 9, 10, 11, 12, 13};   
// delay between changes
int ledDelay;                                         
int direction = 1;
int currentLED = 0;
unsigned long changeTime;
// select the input pin for the potentiometer
int potPin = 2;                                       

void setup() {
        // set all pins to output
        for (int x=0; x<10; x++) {                    
                pinMode(ledPin[x], OUTPUT);
        }
        changeTime = millis();
}

void loop() {
        // read the value from the pot
        ledDelay = analogRead(potPin);
        // if it has been ledDelay ms since last change
        if ((millis() - changeTime) > ledDelay) {      
                changeLED();
                changeTime = millis();
        }
}

void changeLED() {
        for (int x=0; x<10; x++) {                    // turn off all LED's
                digitalWrite(ledPin[x], LOW);
        }
        digitalWrite(ledPin[currentLED], HIGH);       // turn on the current LED
        currentLED += direction;                      // increment by the direction value
                                                      // change direction if we reach the end
         if (currentLED == 9) {direction = -1;}
        if (currentLED == 0) {direction = 1;}
}
```

This time when you verify and upload your code, you should now see the lit LED appear to bounce back and forth between each end of the string of lights as before. But, by turning the knob of the potentiometer, you will change the value of ledDelay and speed up or slow down the effect.
Let’s take a look at how this works and find out what a potentiometer is.

####Code Overview
The code for this project is almost identical to the previous project’s code. We have simply added a potentiometer to our hardware, and the code has additions to enable us to read the values from the potentiometer and use them to adjust the speed of the LED chase effect.
We first declare a variable for the potentiometer pin number

```c
int potPin = 2;
```

as our potentiometer is connected to analog pin 2. To read the value from an analog pin, we use the analogRead command. The Arduino has 6 analog input/outputs with a 10-bit analog-to-digital convertor (we will discuss bits later on). This means the analog pin can read in voltages between 0 to 5 volts in integer values between 0 (0 volts) and 1,023 (5 volts). This gives a resolution of 5 volts / 1,024 units or 0.0049 volts (4.9mV) per unit.

We need to set our delay using the potentiometer, so we will simply use the direct values read in from the pin to adjust the delay between 0 and 1,023 milliseconds (or just over 1 second). We do this by directly reading the value of the potentiometer pin into ledDelay. Notice that we do not need to set an analog pin to be an input or output as we do with a digital pin.


```c
ledDelay = analogRead(potPin);
```

This is done during our main loop, and therefore, it is constantly being read and adjusted. By turning the knob, you can adjust the delay value between 0 and 1,023 milliseconds (or just over a second) and therefore have full control over the speed of the effect.
Okay, let’s find out what a potentiometer is and how it works.

####Hardware Overview
The only additional piece of hardware used in this project was the 4K7 (4700Ω) potentiometer.

You have already come across a resistor and know how it works. The potentiometer is simply a fixed resistor with an adjustable wiper that can be used to divide the total resistance into two parts that add up to the total (fixed) value. In this project, we use a 4K7 (0r 4.7K Ω, K = 1,000) or 4,700Ω potentiometer which means its range is from 0 to 4,700 Ohms.


The potentiometer has three legs. By connecting up just two legs, the potentiometer becomes a variable resistor. By connecting all three legs and applying a voltage across it, the potentiometer becomes a voltage divider. This is how we have used it in our circuit. One side is connected to ground, the other to 5V, and the center pin to our analog pin. By adjusting the knob, a voltage between 0 and 5V will be available from the center pin; we can read the value of that voltage on Analog Pin 2 and use its value to change the delay rate of the light effect.

The potentiometer can be very useful in providing a means of adjusting a value from 0 to a set amount, e.g., the volume of a radio or the brightness of a lamp.

####Exercise
Now try out the following two exercises. You have all the necessary knowledge to adjust the code to enable you to do the following:
 - Exercise 1: Get the LEDs at BOTH ends of the strip to start as on, then to both move toward each other, then appear to bounce off each other, and, finally, move back to the end.
 - Exercise 2: Make a bouncing ball effect by turning the LEDs so that they are vertical. Then make an LED start at the bottom, then “bounce” up to the top LED, then back to the bottom; then next time, have it “bounce” only up to the 9th LED, then back down, then up to the 8th, etc., to simulate a bouncing ball losing momentum after each bounce.
