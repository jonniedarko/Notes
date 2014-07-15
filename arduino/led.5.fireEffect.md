#LED Fire Effect
Here We will use LEDs and a flickering random light effect, using PWM again, to recreate the effect of a flickering flame. If you were to place these LEDs inside a model house on a model railway layout, for example, you could create a special effect of the house being on fire, or you could place it into a fake fireplace in your house to give a fire effect. This is a simple example of how LEDs can be used to create special effects for movies, stage plays, model dioramas, etc.

####Parts Required
This time we are going to use three LEDs, one red, one green, and one blue.


Red Diffused 5mm LED	
2 x Yellow Diffused 5mm LED	
3 x Current-Limiting Resistor	


####Connect It Up
Power down your Arduino, then connect up your three LEDs as in Figure 3-7. This is essentially the same circuit as in project 8, but using one red and two yellow LEDs instead of red, green, and blue. Again, the effect is best seen when the light is diffused using a cylinder of paper, or when bounced off a white card or mirror onto the surface you wish the effect to be projected against.
![circuit - fire effect](https://cloud.githubusercontent.com/assets/3673943/3586415/2476127c-0c30-11e4-8226-bbefbeeb3304.jpg)

####Enter the Code
Open up your Arduino IDE and type in the code 
```c
// Project 9 - LED Fire Effect
int ledPin1 = 9;
int ledPin2 = 10;
int ledPin3 = 11;

void setup()
{
        pinMode(ledPin1, OUTPUT);
        pinMode(ledPin2, OUTPUT);
        pinMode(ledPin3, OUTPUT);
}

void loop()
{
        analogWrite(ledPin1, random(120)+135);
        analogWrite(ledPin2, random(120)+135);
        analogWrite(ledPin3, random(120)+135);
        delay(random(100));
}
```

Now press the Verify/Compile button at the top of the IDE to make sure there are no errors in your code. If this is successful, you can now click the Upload button to upload the code to your Arduino.
If you have done everything correctly, you should now see the LEDs flickering in a random manner to simulate a flame or fire effect.
Now let’s take a look at the code and find out how it works.

####Code Overview
So let’s take a look at the code for this project. First we declare and initialize some integer variables that will hold the values for the digital pins we are going to connect our LEDs to.

int ledPin1 = 9;
int ledPin2 = 10;
int ledPin3 = 11;

We then set them up to be outputs.

pinMode(ledPin1, OUTPUT);
pinMode(ledPin2, OUTPUT);
pinMode(ledPin3, OUTPUT);

The main program loop then sends out a random value between 0 and 120, and then adds 135 to it to get full LED brightness, to the PWM pins 9, 10 and 11.

analogWrite(ledPin1, random(120)+135);
analogWrite(ledPin2, random(120)+135);
analogWrite(ledPin3, random(120)+135);

Finally, we have a random delay between zero and 100ms.

delay(random(100));

The main loop then starts again, causing the flicker light effect you can see. Bounce the light off a white card or a mirror onto your wall and you will see a very realistic flame effect.
As the hardware is simple, we will jump right into Project 10.

####EXERCISE
Now try out the following two exercises:
 - Exercise 1: Using a blue and/or white LED or two, see if you can recreate the effect of the flashes of light from an arc welder.
 - Exercise 2: Using blue and red LEDs, recreate the effect of the lights on an emergency vehicle.
