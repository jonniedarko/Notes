#Simple Motor Control
We are first going to simply control the speed of a DC motor, in one direction, using a power transistor, diode, and external power supply to power the motor and a potentiometer to control the speed. Any suitable NPN power transistor designed for high current loads can replace the TIP120 transistor. However, I would highly recommend you use a power Darlington-type transistor. Make sure you choose a transistor that can handle the voltage and current your motor will draw. It may be necessary to fit a heat sink to the transistor if it is pulling more than about an amp.

The external power supply can be a set of batteries or a “wall wart”-style external DC power supply. The power source must have enough voltage and current to drive the motor. The voltage must not exceed that required by the motor. For my testing purposes I used a DC power supply that provided 5V at 500mA. This was enough for the 5V DC motor I was using. If you use a power supply with voltage higher than the motor can take, you may damage it permanently. 

####Parts Required
Parts|
------|-------
DC Motor	| ![dc motor](https://cloud.githubusercontent.com/assets/3673943/3596515/c950444e-0cc5-11e4-9b3f-4078063025fd.jpg)
10kΩ Potentiometer | ![10 k potentiometer](https://cloud.githubusercontent.com/assets/3673943/3596514/c94ffe80-0cc5-11e4-9cd8-4a0549e28668.jpg)
TIP120 Transistor* | ![tip120 transistor](https://cloud.githubusercontent.com/assets/3673943/3596519/c96e7086-0cc5-11e4-8010-5cfcce863740.jpg)
1N4001 Diode*	| ![diode](https://cloud.githubusercontent.com/assets/3673943/3596517/c9506122-0cc5-11e4-850b-4136e4aa6fb0.jpg)
Jack Plug	| ![jackplug](https://cloud.githubusercontent.com/assets/3673943/3596516/c9505a10-0cc5-11e4-8eda-337e63f699a0.jpg)
External Power Supply | ![pwr](https://cloud.githubusercontent.com/assets/3673943/3596518/c950cf5e-0cc5-11e4-84fb-c74637c1e311.jpg)
1K Resistor | ![1k resistor](https://cloud.githubusercontent.com/assets/3673943/3596531/02eed1de-0cc6-11e4-873b-61559826ea88.jpg)
