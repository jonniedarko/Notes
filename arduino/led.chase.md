#LED Chase Effect
We are now going to use a string of LEDs (10 in total) to make an LED chase effect (see Table 3-1), similar to that used on the car KITT in the Knightrider TV series or the Cylons in Battlestar Galactica; on the way, we’ll introduce the concept of arrays.

####Parts Required
10 x 5mm RED LEDs	![LEDs]((https://cloud.githubusercontent.com/assets/3673943/3584985/ebd67d26-0c20-11e4-8372-295e58ab7e80.jpg)

10 x Current-Limiting Resistors	![Resistors](https://cloud.githubusercontent.com/assets/3673943/3584986/ebd6ee14-0c20-11e4-9095-93822c64d14a.jpg))

####Connect It Up
First, make sure your Arduino is powered off by unplugging it from the USB cable. Now take your breadboard, LEDs, resistors, and wires, and connect everything up as in Figure 3-1. Check your circuit thoroughly before connecting the power back up to the Arduino.

![The circuit](https://cloud.githubusercontent.com/assets/3673943/3584987/ebd7e95e-0c20-11e4-9b8a-93605ef90f9d.jpg)


####Enter the Code
Open up your Arduino IDE and type in the code from Listing 3-1:

```c
// Project 5 - LED Chase Effect
byte ledPin[] = {4, 5, 6, 7, 8, 9, 10, 11, 12, 13};    // Create array for LED pins
int ledDelay = 65;                                     // delay between changes
int direction = 1;
int currentLED = 0;
unsigned long changeTime;

void setup() {
        for (int x=0; x<10; x++) {                     // set all pins to output
                pinMode(ledPin[x], OUTPUT);
        }
        changeTime = millis();
}

void loop() {
        if ((millis() - changeTime) > ledDelay) {      // if it has been ledDelay ms since last change
                changeLED();
                changeTime = millis();
        }
}

void changeLED() {
        for (int x=0; x<10; x++) {                      // turn off all LED's
                 digitalWrite(ledPin[x], LOW);
        }
        digitalWrite(ledPin[currentLED], HIGH);         // turn on the current LED
        currentLED += direction;                        // increment by the direction value
        // change direction if we reach the end
        if (currentLED == 9) {direction = -1;}
        if (currentLED == 0) {direction = 1;}
}
```

Now press the Verify button at the top of the IDE to make sure there are no errors in your code. If this is successful, you can now click the Upload button to upload the code to your Arduino. If you have done everything correctly, you should now see the LEDs appear to move along the line, then bounce back to the start. We have not introduced any different hardware in this project, so there is no need to take a look at that. However, we have introduced a new concept in the code of this project in the form of arrays. Let us take a look at the code for Project 5 and see how it works.

##Code Overview
Our very first line in this sketch is

```c
byte ledPin[] = {4, 5, 6, 7, 8, 9, 10, 11, 12, 13};
```
which declares the ledPin variable to be an array (collection) of elements of data type byte. All the elements share the same name (in this case, ledPin), but can be selected individually by an index number, much like apartments in an apartment building. The [] after the variable name in the declaration tells the compiler this is an array variable rather than a simple (non-indexed) variable. We have then initialized the array with 10 values, which are the digital pin numbers 4 through to 13. To access an element of the array, we follow the array name with the index number of that element between square brackets. The index number between the [] doesn’t have to be a constant—it can be a variable or expression. Arrays are zero indexed, which simply means that the index of the first element is zero, rather than one. So in our 10-element array, the index numbers are 0 to 9. In this case, the third element `ledPin[2]` has the value of 6, and the seventh element `ledPin[6]` has a value of 10.

You have to tell the compiler the size of the array if you do not initialize it with data first in the declaration. In our sketch we did not explicitly choose a size, as the compiler is able to count the values we have assigned to the array to work out that the size is 10 elements. If we had declared the array, but not initialized it with values at the same time, we would need to declare a size, for example we could have done this:

```c
byte ledPin[10];
```

and then loaded data into the elements later on. To retrieve a value from the array, we would do something like this:

```c
x = ledpin[5];
```

In this example, x would now hold a value of 8. To get back to your program, we have started off by declaring and initializing an array with 10 values that are the digital pin numbers used for the outputs to our 10 LEDs.
In our main loop, we check that at least ledDelay milliseconds have passed since the last change of LEDs, and if so, it passes control to our function. The reason we are only going to pass control to the `changeLED()` function in this way, rather than using `delay()` commands, is to allow other code, if needed, to run in the main program loop (as long as that code takes less than ledDelay to run.
The function we created is

```c
void changeLED() {
        // turn off all LED's
        for (int x=0; x<10; x++) {
                digitalWrite(ledPin[x], LOW);
        }
        // turn on the current LED
        digitalWrite(ledPin[currentLED], HIGH);
        // increment by the direction value
        currentLED += direction;
        // change direction if we reach the end
        if (currentLED == 9) {direction = -1;}
        if (currentLED == 0) {direction = 1;}
}
```
and the job of this function is to turn all LEDs off and then turn on the current LED (this is done so fast you will not see it happening), which is stored in the variable currentLED.
This variable then has direction added to it. As direction can only be either a 1 or a −1, then the number will either increase (+1) or decrease by one (currentLED +(−1)).
We then have an if statement to see if we have reached the end of the row of LEDs, and if so, we then reverse the direction variable.
By changing the value of ledDelay, you can make the LED ping back and forth at different speeds. Try different values to see what happens. You have to stop the program and manually change the value of ledDelay, then upload the amended code to see any changes.
However, wouldn’t it be nice to be able to adjust the speed while the program is running? Yes, it would, so let’s do exactly that in the next project by introducing a way to interact with the program and adjust the speed using a potentiometer.
