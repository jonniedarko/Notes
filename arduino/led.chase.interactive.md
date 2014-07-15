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
