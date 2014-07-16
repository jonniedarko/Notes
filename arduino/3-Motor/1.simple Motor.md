#Simple Motor Control
We are first going to simply control the speed of a DC motor, in one direction, using a power transistor, diode, and external power supply to power the motor and a potentiometer to control the speed. Any suitable NPN power transistor designed for high current loads can replace the TIP120 transistor. However, I would highly recommend you use a power Darlington-type transistor. Make sure you choose a transistor that can handle the voltage and current your motor will draw. It may be necessary to fit a heat sink to the transistor if it is pulling more than about an amp.

The external power supply can be a set of batteries or a “wall wart”-style external DC power supply. The power source must have enough voltage and current to drive the motor. The voltage must not exceed that required by the motor. For my testing purposes I used a DC power supply that provided 5V at 500mA. This was enough for the 5V DC motor I was using. If you use a power supply with voltage higher than the motor can take, you may damage it permanently. 

####Parts Required
|Parts| |
|------|-------|
|DC Motor	| ![dc motor](https://cloud.githubusercontent.com/assets/3673943/3596515/c950444e-0cc5-11e4-9b3f-4078063025fd.jpg)|
10kΩ Potentiometer | ![10 k potentiometer](https://cloud.githubusercontent.com/assets/3673943/3596514/c94ffe80-0cc5-11e4-9cd8-4a0549e28668.jpg)|
|TIP120 Transistor* | ![tip120 transistor](https://cloud.githubusercontent.com/assets/3673943/3596519/c96e7086-0cc5-11e4-8010-5cfcce863740.jpg)|
|1N4001 Diode*	| ![diode](https://cloud.githubusercontent.com/assets/3673943/3596517/c9506122-0cc5-11e4-850b-4136e4aa6fb0.jpg)|
|Jack Plug	| ![jackplug](https://cloud.githubusercontent.com/assets/3673943/3596516/c9505a10-0cc5-11e4-8eda-337e63f699a0.jpg)|
|External Power Supply | ![pwr](https://cloud.githubusercontent.com/assets/3673943/3596518/c950cf5e-0cc5-11e4-84fb-c74637c1e311.jpg)|
|1K Resistor | ![1k resistor](https://cloud.githubusercontent.com/assets/3673943/3596531/02eed1de-0cc6-11e4-873b-61559826ea88.jpg)|

####Connect It Up
First, make sure your Arduino is powered off by unplugging it from the USB cable. Now, take the required parts and connect them up as in the circuit below. It is essential for this project that you check and double check all of your connections are as they should be before supplying power to the circuit, as failure to do so may result in damage to your components or even your Arduino. The diode, in particular, is essential to protect the Arduino from back EMF, which we will explain later.

![circuit - simple motor](https://cloud.githubusercontent.com/assets/3673943/3596552/762b1c16-0cc6-11e4-864b-441429d5502a.jpg)

####Enter the Code
Open up your Arduino IDE and type in the following code:

```c
// Project 15 - Simple Motor Control

int potPin = 0;           // Analog in 0 connected to the potentiometer
int transistorPin = 9;    // PWM Pin 9 connected to the base of the transistor
int potValue = 0;         // value returned from the potentiometer

void setup() {
  // set  the transistor pin as output:
  pinMode(transistorPin, OUTPUT);
}

void loop() {
  // read the potentiometer, convert it to 0 - 255:
  potValue = analogRead(potPin) / 4;
  // use that to control the transistor:
  analogWrite(transistorPin, potValue);
}
```

Before uploading your code, disconnect the external power supply to the motor and ensure the potentiometer is turned fully counterclockwise. Now upload the code to the Arduino. Once the code is uploaded, connect the external power supply. You can now turn the potentiometer to control the speed of the motor.

####Code Overview
First we declare three variables that will hold the value for the analog pin connected to the potentiometer, the PWM pin connected to the base of the transistor, and one to hold the value read back from the potentiometer from analog pin 0.
```c
int potPin = 0;           // Analog in 0 connected to the potentiometer
int transistorPin = 9;    // PWM Pin 9 connected to the base of the transistor
int potValue = 0;         // value returned from the potentiometer
In the setup( ) function we set the pin mode of the transistor pin to output.
void setup() {
  // set  the transistor pin as output:
  pinMode(transistorPin, OUTPUT);
}
```

In the main loop potValue is set to the value read in from analog pin 0 (the potPin) and then divided by 4.
```c
potValue = analogRead(potPin) / 4;
```

We need to divide the value read in by 4 as the analog value will range from 0 for 0 volts to 1,023 for 5 volts. The value we need to write out to the transistor pin can only range from 0 to 255, so we divide the value of analog pin 0 (max 1023) by 4 to give the maximum value of 255 for setting the digital pin 9 (Using analogWrite so we are using PWM).
The code then writes out to the transistor pin the value of the pot.
```c
analogWrite(transistorPin, potValue);
```
In other words, when you rotate the potentiometer, different values ranging from 0 to 1,023 are read in and these are converted to the range 0 to 255, and then that value is written out (via PWM) to digital pin 11, which changes the speed of the DC motor. Turn the pot all the way to the left and the motor goes off; turn it to the right and it speeds up until it reaches maximum speed.
The code is very simple and we have learned nothing new. Let us now take a look at the hardware used in this project and see how it all works.


####Hardware Overview
The circuit is essentially split into two sections. Section 1 is our potentiometer, which is connected to 5V and ground with the center pin going into analog pin 0. As the potentiometer knob is rotated, the resistance divider changes to allow voltages from 0 to 5V to come out of the center pin, whose value is read using analog pin 0.
The second section is what controls the power to the motor. The digital pins on the Arduino give out a maximum of 40mA (milliamps). However, the datasheet says the highest output current that is guaranteed is only 20mA. You should never be operating at absolute maximum. A DC motor may require around 500mA to operate at full speed, and this is obviously too much for the Arduino. If we were to try to drive the motor directly from a pin on the Arduino serious and permanent damage could occur.

Therefore, we need to find a way to supply it with a higher current. We therefore take power from an external power supply, which will give us enough current to power the motor. We could use the 5V output from the Arduino, which can provide up to 800mA when connected to an external power supply, and less when powered by USB. However, Arduino boards are expensive and it is all too easy to damage them when connecting them up to high current, electrically noisy loads such as DC motors. We will therefore play it safe and use an external power supply. Also, your motor may require 9V or 12V or higher voltages, and this is beyond anything the Arduino can supply.
However, this project controls the speed of the motor, so we need a way to control the power to speed up or slow down the motor. This is where the TIP-120 transistor comes in.

#####Transistors
A transistor is a power amplifier that can also be used as a digital switch. In our circuit we use it as a switch. The electronic symbol for the type of transistor we are using in this project looks like this:

![npn transistor](https://cloud.githubusercontent.com/assets/3673943/3596614/4db8c796-0cc7-11e4-9c45-b4c892cb4329.jpg)

The transistor has three legs: one is the base, one is the collector, and the third, the emitter. These are marked as C, B and E on the diagram. In our circuit we have up to 5 volts going via a resistor to the base via digital pin 9. The collector is connected to one terminal on the motor. The emitter is connected to ground. Whenever we apply a voltage to the base, via pin 9, this makes the transistor turn on, allowing current to flow through it between the emitter and collector, thus powering the motor that is connected in series with this circuit. By applying a small current at the base we can control a larger current between the emitter and collector.
Transistors are the key components in just about any piece of modern electronic equipment. Many people consider the transistor to be the greatest invention of the twentieth century. For bipolar transistors, current flowing through the base lead allows a (roughly) proportional, but much larger amount of current to flow from the collector to the emitter. The ratio between the base current and the corresponding collector current is known as the gain of the transistor. The gain of the power Darlington transistor we used in this project is about 1,000, meaning a small (1mA) current at the base can control an amp of current flow between the emitter and collector. As we are pulsing our signal, the transistor is turned on and off many times per second and it is this pulsed current that controls the speed of the motor.

#####Motors
A motor is an electromagnet and it has a magnetic field while power is supplied to it. When the power is removed, the magnetic field collapses and this collapsing field can produce a reverse voltage to go back up its wiring. This could seriously damage your Arduino, and that is why the diode has been placed the way it is on the circuit. The white stripe on the diode normally goes to ground. Power will flow from the positive side to the negative side. As we have it the wrong way around, no power will flow down it at all. However, if the motor were to produce a “back EMF” (electromotive force) and send current back down the wire, the diode would act as a valve and prevent it from doing so. The diode in our circuit is therefore put in place to protect your Arduino.
In a given circuit at a given time, a diode may be forward-biased (anode more positive than cathode) or reverse-biased (cathode more positive than anode). Simple diodes conduct well when forward-biased, and conduct very little when reverse-biased. The diode behaves differently in one situation than in the other, but its proper orientation depends on what we want it to accomplish in a given circuit. In this case, we want to provide a safe path (i.e., not through the Arduino or transistor) for the electricity generated by the collapsing field in the motor. The voltage across the motor from the collapsing field is the opposite polarity from the voltage that created the field. Placing a diode with the cathode (white stripe) to the power supply side of the motor and the anode (other lead) to the transistor side of the motor will accomplish this. When we have the motor turned on, the diode is reverse-biased, so it passes a negligible leakage current. When the transistor turns off, the induced voltage from the motor forward biases the diode, which gives the induced current a safe discharge path.
If you were to connect a DC motor directly to a multimeter with no other components connected to it, and then spin the shaft of the motor, you will see that the motor generates a current. This is exactly how wind turbines work. When we have the motor powered up and spinning, and we then remove the power, the motor will keep on spinning under its own inertia until it comes to a stop. In that short time while the motor is spinning without power applied, it is generating a current. This is called “back EMF,”as I mentioned above. The diode acts as a valve and ensures that the electricity does not flow back to the circuit and damage other components.

#####Diodes
Diodes are one-way valves. In exactly the same way that a non-return valve in a water pipe will allow water to flow in one direction, but not in the other, the diode allows a current to flow in one direction, but not the other. We have already come across diodes when we used LEDs. An LED is a light emitting diode and has a polarity. You connect the positive terminal of the power, usually to the long lead on the LED to make the LED turn on. If you turn the LED around, the LED will not only fail to illuminate but will also prevent electricity from flowing across its terminals. The 1N4001 diode has a white band around it next to the cathode (negative) lead. Other diodes may mark the cathode differently, such as a black band on glass-body or metal-case diodes. Imagine the band as a barrier. Electricity flows through the diode from the terminal that has no barrier. When you reverse the voltage and try to make it flow through the side that has the band, the current will be stopped.
Diodes are essential in protecting circuits from a reverse voltage, which occurs if you connect a power supply the wrong way around or if a voltage is reversed, such as the back EMF in our circuit. Therefore, always try to use them in your circuits whenever there is a danger of the power being reversed, either by user error or via phenomena such as back EMF
