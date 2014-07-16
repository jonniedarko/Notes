#Shift Register 8-Bit Binary Counter
we are going to use additional ICs (integrated circuits) in the form of shift registers, to enable us to drive LEDs to display a binary counter (we will explain what binary is shortly). In this project, we will drive eight LEDs independently using just three output pins from the Arduino.

|parts                       |                                  |
|----------------------------|----------------------------------|
| 1 x 74HC595 Shift Register | ![shift register ic](https://cloud.githubusercontent.com/assets/3673943/3596920/c4322616-0ccb-11e4-90b0-1e03290ad4db.jpg) |
|8 x 560Ω Resistors*	     | ![560 ohm resistor](https://cloud.githubusercontent.com/assets/3673943/3596919/c432143c-0ccb-11e4-954a-0efdd441872d.jpg) |
|8 x 5mm LEDs	             | ![redLed](https://cloud.githubusercontent.com/assets/3673943/3584985/ebd67d26-0c20-11e4-8372-295e58ab7e80.jpg) |
|0.1uF Capacitor             | ![0 1uf capacitor](https://cloud.githubusercontent.com/assets/3673943/3596918/c432102c-0ccb-11e4-9d11-677c7b900433.jpg) |

####Connect It Up
Examine the diagram carefully. Connect the 5V to the top rail of your breadboard and the ground to the bottom. The chip has a small dimple on one end; this dimple goes to the left. Pin 1 is below the dimple, pin 8 at bottom right, pin 9 at top right, and pin 16 at top left.

You now need wires to go from the 5V supply to pins 10 and 16, as well as wires from ground to pins 8 and 13.
A wire goes from digital pin 8 to pin 12 on the IC. Another one goes from digital pin 12 to pin 11 on the IC, and finally, one from digital pin 11 to pin 14 on the IC.

The 8 LED’s have a 560Ω resistor between the cathode and ground. The anode of LED 1 goes to pin 15. The anode of LEDs 2 to 8 goes to pins 1 to 7 on the IC.

You will need a bypass capacitor (otherwise known as a decoupling capacitor) to go between the power supply pin and ground. Make sure its voltage rating is higher than the supply voltage being used. The leads should be kept short, with the capacitor as close to the chip as possible. The purpose of this capacitor is to reduce the effect of electrical noise on the circuit.
![circuit - shift register](https://cloud.githubusercontent.com/assets/3673943/3596955/193240b0-0ccc-11e4-9e69-40be9853ede1.jpg)

Once you have connected everything up as in above, check that your wiring is correct and the IC and LEDs are the right way around. 


####Enter the code.
Enter the following code in Listing 6-1 and upload it to your Arduino. Once the code runs, you will see the LEDs turn on and off individually as they count in binary every second from 0 to 255, then start again.

```c
int latchPin = 8;  //Pin connected to Pin 12 of 74HC595 (Latch)
int clockPin = 12; //Pin connected to Pin 11 of 74HC595 (Clock)
int dataPin = 11;  //Pin connected to Pin 14 of 74HC595 (Data)

void setup() {
        //set pins to output
        pinMode(latchPin, OUTPUT);
        pinMode(clockPin, OUTPUT);
        pinMode(dataPin, OUTPUT);
}

void loop() {
        //count from 0 to 255
        for (int i = 0; i < 256; i++) {
                shiftDataOut(i);
                //set latchPin low then high to send data out
                digitalWrite(latchPin, LOW);
                digitalWrite(latchPin, HIGH);
                delay(1000);
        }
}

void shiftDataOut(byte dataOut) {
        // Shift out 8 bits LSB first, clocking each with a rising edge of the clock line
        boolean pinState;
   
        for (int i=0; i<=7; i++)  {              // for each bit in dataOut send out a bit
                digitalWrite(clockPin, LOW);     //set clockPin to LOW prior to sending bit

                // if the value of DataOut and (logical AND) a bitmask
                // are true, set pinState to 1 (HIGH)
                if ( dataOut & (1<<i) ) {
                        pinState = HIGH;
                }
                else {
                        pinState = LOW;
                }

                //sets dataPin to HIGH or LOW depending on pinState
                digitalWrite(dataPin, pinState); //send bit out before rising edge of clock
                digitalWrite(clockPin, HIGH);
        }
        digitalWrite(clockPin, LOW); //stop shifting out data
}
```

#####The Binary Number System
Before we take a look at the code and the hardware for Project 17, let’s take a look at the binary number system, as it is essential to understand binary to successfully program a microcontroller.

Human beings use a base 10, or decimal number system, because we have 10 fingers on our hands. Computers do not have fingers, and so the best way for a computer to count is by a state of either ON or OFF (1 or 0). A logic device, such as a computer, can detect if a voltage is there (1) or if it is not (0), and so uses a binary, or base 2 number system, since this number system can easily be represented in an electronic circuit with a high or low voltage state.

In our number system, base 10, we have 10 digits ranging from 0 to 9. When we count to the next digit after 9, the digit resets back to zero, but a 1 is incremented to the tens column to its left. Once the tens column reaches 9, incrementing this by 1 will reset it to zero, but we add 1 to the hundreds column to its left, and so on.

```
000,001,002,003,004,005,006,007,008,009
010,011,012,013,014,015,016,017,018,019
020,021,023 .........
```

In binary, the exact same thing happens, except the highest digit is 1. So, adding 1 to 1 results in the digit resetting to zero and 1 being added to the column to the left
```
000, 001
010, 011
100, 101...
```

An 8-bit number (or a byte) is represented as

![8bit binary no](https://cloud.githubusercontent.com/assets/3673943/3597005/9210846a-0ccc-11e4-9a5a-eba684bd6de6.jpg)

The number above in binary is 01001011; in decimal, this is 75.
This is worked out as follows:
1 x 1 = 1
1 x 2 = 2
1 x 8 = 8
1 x 64 = 64
Add all that together and you get 75.
Here are some other examples:
![binary numbers](https://cloud.githubusercontent.com/assets/3673943/3597020/b4cdc3be-0ccc-11e4-9a9f-de5841f2b515.JPG)

**_Tip: _**You can use Google to convert between a decimal and a binary number, and vice versa.

For example, to convert 171 decimal to binary, type **171 in binary** into the Google search box which returns **171 = 0b10101011**
The 0b prefix shows the number is a binary number and not a decimal one. So the answer is 10101011.

To convert a binary number to decimal, do the reverse, e.g., enter **0b11001100 in decimal** into the search box; this returns **0b11001100 = 204**

####Shift Register 8-Bit Binary Counter - Hardware Overview
We are going to do things in a different order for this project, and take a look at the hardware before we look at the code.
We are using a shift register, specifically, the 74HC595 type of shift register. This type of shift register is an 8-bit serial-in, serial or parallel-out shift register with output latches. This means that you can send data to the shift register in series and send it out in parallel. In series means 1 bit at a time. Parallel means lots of bits (in this case, 8) at a time. So you give the shift register data (in the form of 1s and 0s) one bit at a time. Each bit is shunted along as the next bit is entered. When a ninth bit is entered, the first bit entered will be shunted out the end of the shift register and lost (unless we do something with it—see the next project). When you raise the latch signal from LOW to HIGH, the current contents of the shift register are copied to the output latches. (Copying data to the output latches does not affect the contents of the shift register.)

This type of shift register is usually used for serial to parallel data conversion. Other shift registers are parallel in, serial out, and used for parallel to serial data conversion. Some shift registers can do both. In our case, as the data that is output is 1s and 0s (or 0V and 5V), we can use it to turn a bank of 8 LEDs on and off.

The shift register for this project requires only three inputs from the Arduino. The outputs of the Arduino and the inputs of the 595 are shown in Table 6-4). We are not using the features of pins 10 (master reset) and 13 (output enable input). However, these pins still need to be set to high (pin 10) or low (pin 13) for the project to work.

![shift register - pins table](https://cloud.githubusercontent.com/assets/3673943/3597062/2afae968-0ccd-11e4-8d73-b21fdae9d1df.JPG)

We are going to refer to pin 11 as the clock pin, pin 14 as the data pin, and pin 12 as the latch pin.
Imagine that the latch is like a camera that takes a snapshot of the contents of the shift register when the latch pin goes from LOW to HIGH and outputs that snapshot to the 8 pins (QA-QH or Q0 to Q7 depending on your datasheet—see image below). The clock is simply a pulse of 0s and 1s and the data pin is where we send data from the Arduino to the 595.

![pin diagram of a 595 chip](https://cloud.githubusercontent.com/assets/3673943/3597098/b505b2dc-0ccd-11e4-8003-fd49442923b9.JPG)

To use the shift register, the latch pin and clock pin must be set to LOW. The latch pin will remain at LOW until all 8 bits have been set. This allows data to be entered into the storage register (a place inside the IC for storing a 1 or a 0). We then present either a HIGH or LOW signal at the data pin and then set the clock pin to HIGH. By setting the clock pin to HIGH, we copy the data presented at the data pin into the storage register. Once this is done, we set the clock to LOW again, then present the next bit of data at the data pin. Once we have done this 8 times, we have sent a full 8 bit number into the 595. The latch pin is now raised, which copies the data from the storage register into the shift register and outputs it from Q0 to Q7 (pin 15, 1 to 7).
The sequence of events is described in below:
![sequence of events 595 chip](https://cloud.githubusercontent.com/assets/3673943/3597124/fe50b1bc-0ccd-11e4-8a29-200bc897cc63.JPG)

I have connected a logic analyzer (a device that lets you see the 1s and 0s coming out of a digital device) to my 595 while this program is running and the image below shows the output of the Arduino to the 595. You can see from this diagram that the binary number 00110111 (reading from right to left) or decimal 55, has been sent to the IC.

![595 shown in a logic analyzer](https://cloud.githubusercontent.com/assets/3673943/3597145/1ab20252-0cce-11e4-966d-7c46c5f3a153.jpg)

So to summarize the use of a single shift register in this project, we have 8 LEDs attached to the 8 outputs of the register. Data is sent to the data pin, one bit at a time, the cLock pin is set to HIGH to store that data, then back down to LOW, ready for the next bit. After all 8 bits have been entered, the latch is set to LOW and then to HIGH, which copies data from the shift register to the 8 output pins.

If you want to read more about shift registers, take a look at the part number on the IC (e.g. 74HC595N or SN74HC595N, etc.) and enter that into Google. You can then find the specific datasheet for the IC and read more about it.
I’m a huge fan of the 595 chip. It is very versatile and can increase the number of digital output pins that the Arduino has. The standard Arduino has 19 digital outputs (the 6 analog pins can also be used as digital pins numbered 14 to 19). Using 8-bit shift registers, you can expand that to 49 (6 x 595s plus one spare pin left over). They also operate very fast, typically at 100MHz, meaning you can send data out at approximately 100 million times per second (if you wanted to and if the Arduino was capable of doing so). This means you can also send PWM signals via software to the ICs and enable brightness control of the LEDs too.

As the outputs are simply logic HIGH or LOW of output voltages, they can also be used to switch other low-powered devices on and off (or even high-powered devices with the use of transistors or relays), or to send data to devices (e.g. an old dot-matrix printer or other serial device).

All of the 595 shift registers from any manufacturer are just about identical to each other. There are also larger shift registers with 16 outputs or higher. Some ICs advertised as LED driver chips are, when you examine the datasheet, simply larger shift registers (e.g., the M5450 and M5451 from STMicroelectronics).

####Code Overview
The code looks pretty daunting at first look. But when you break it down into its component parts, you’ll see it’s not as complex as it looks.
First, three variables are initialized for the three pins we are going to use.

```c
int latchPin = 8;
int clockPin = 12;
int dataPin = 11;
Then, in setup, the pins are all set to outputs.
pinMode(latchPin, OUTPUT);
pinMode(clockPin, OUTPUT);
pinMode(dataPin, OUTPUT);
```
The main loop simply runs a for loop counting from 0 to 255. On each iteration of the loop, the latchPin is set to LOW to enable data entry, then the function called shiftDataOut is called, passing the value of i in the for loop to the function. Then the latchPin is set to HIGH, transferring data from the shift register to the output latches and output pins. Finally there is a delay of half a second before the next iteration of the loop commences.

```c
void loop() {
  //count from 0 to 255
  for (int i = 0; i < 256; i++) {
    //set latchPin low to allow data flow
    digitalWrite(latchPin, LOW);
    shiftDataOut(i);
    //set latchPin to high to lock and send data
    digitalWrite(latchPin, HIGH);
    delay(500);
  }
}
```
The shiftDataOut function receives as a parameter a byte (8-bit number), which will be our number between 0 and 255. We have chosen a byte for this usage as it is exactly 8 bits in length and we need to send only 8 bits out to the shift register.

```c
void shiftDataOut(byte dataOut) {
```
Then a boolean variable called pinState is initialized. This will store the state we wish the relevant pin to be in when the data is sent out (1 or 0).
```c
boolean pinState;
```
After this, we are ready to send the 8 bits in series to the 595 one bit at a time.
A for loop that iterates 8 times is set up.
```c
for (int i=0; i<=7; i++)  {
```
The clock pin is set low prior to sending a data bit.
```c
digitalWrite(clockPin, LOW);
```
Now an if/else statement determines if the pinState variable should be set to a 1 or a 0.
```c
if ( dataOut & (1<<i) ) {
      pinState = HIGH;
}
else {
      pinState = LOW;
}
```
The condition for the if statement is:
```c
dataOut & (1<<i).
```
This is an example of what is called a “bitmask,” and we are now using bitwise operators. These are logical operators similar to the boolean operators we used in previous projects. However, the bitwise operators act on numbers at the bit level.

In this case we are using the bitwise and (&) operator to carry out a logical operation on two numbers. The first number is dataOut and the second is the result of (1<<i). Before we go any further, let’s take a look at the bitwise operators.

#####Bitwise AND (&)
The bitwise AND operator acts according to this rule:-
If both inputs are 1, the resulting outputs are 1, otherwise the output is 0.
Another way of looking at this is:

```
0 0 1 1       Operand1
0 1 0 1       Operand2
_________
0 0 0 1       (Operand1 & Operand2)
```

A type int is a 16-bit value, so using & between two int expressions causes 16 simultaneous AND operations to occur, as in a section of code like this:

```
int x = 77;    //binary: 0000000001001101
int y = 121;   //binary: 0000000001111001
int z = x & y; //result: 0000000001001001
```
In this case 77 & 121 = 73
Let’s look at the remaining operators.

#####Bitwise OR (|)
If either or both of the inputs is 1, the result is 1, otherwise it is 0.
```
0 0 1 1       Operand1
0 1 0 1       Operand2
_____________
0 1 1 1       (Operand1 | Operand2)
```
#####Bitwise XOR (^)
If only 1 of the inputs is 1, then the output is 1. If both inputs are 1, then the output 0.
```
0 0 1 1       Operand1
0 1 0 1       Operand2
__________
0 1 1 0       (Operand1 ^ Operand2)
```

#####Bitwise NOT (∼)
The bitwise NOT operator is applied to a single operand to its right.
The output becomes the opposite of the input. Zeros get converted to ones, and ones to zeros.
```
0 0 1 1       Operand1
__________
1 1 0 0       ∼Operand1
```

#####Bitshift Left (<<), Bitshift Right (>>)
The bitshift operators move all of the bits in the integer to the left or right. with the number of bits specified by the right operand.
```
variable << number_of_bits

E.g.

byte x = 9 ;     // binary: 00001001
byte y = x << 3; // binary: 01001000 (or 72 dec)
```

Any bits shifted off the end of the row are lost forever. You can use the left bitshift to multiply a number by powers of 2 and the right bitshift to divide by powers of 2 (work it out).
Now that we have taken a look at the bitshift operators, let’s return to our code.

#####Code Overview (continued)
The condition of the if/else statement was
```c
dataOut & (1 << i)
```
And we now know this is a bitwise AND (&) operation. The right-hand operand inside the parenthesis is a left bitshift operation. This is a “bitmask.” The 74HC595 will only accept data one bit at a time. We therefore need to convert the 8-bit number in dataOut into a single bit number representing each of the 8 bits in turn. The bitmask allows us to ensure that the pinState variable is set to either a 1 or a 0 depending on what the result of the bitmask calculation is. The right hand operand is the number 1 bit shifted i number of times. As the for loop makes the value of i go from 0 to 7, we can see that 1 bitshifted i times, each time through the loop, will result in these binary numbers (see Table 6-6).

![results in binary 1](https://cloud.githubusercontent.com/assets/3673943/3597214/f41c0cfe-0cce-11e4-9dd5-df720706e31f.JPG)

So you can see that the 1 moves from right to left as a result of this operation.
Now, the & operator’s rules state that
If both inputs are 1, the resulting outputs are 1, otherwise the output is 0.
So, the condition of

```c
dataOut & (1<<i)
```
will result in a 1 if the corresponding bit in the same place as the bitmask is a 1, otherwise it will be a zero. For example, if the value of dataOut was decimal 139 or 10001011 binary, then each iteration through the loop will result in the values below.
![results in binary 2](https://cloud.githubusercontent.com/assets/3673943/3597213/f41c1956-0cce-11e4-9d3c-5a782203d2a0.JPG)

So, every time there is a 1 in the I position (reading from right to left) the value comes out as a non-zero value (or TRUE) and every time there is a 0 in the I position, the value comes out at 0 (or FALSE).
The `if` condition will therefore carry out its code in the block if the value is higher than 0 (or in other words if the bit in that position is a 1) or `else` (if the bit in that position is a 0) it will carry out the code in the else block.
So looking at the if/else statement once more

```c
if ( dataOut & (1<<i) ) {
      pinState = HIGH;
    }
    else {
      pinState = LOW;
    }
```

And cross referencing this with the truth table above, we can see that for every bit in the value of dataOut that has the value of 1 that pinState will be set to HIGH and for every value of 0 it will be set to LOW.
The next piece of code writes either a HIGH or LOW state to the data pin and then sets the clock pin to HIGH to write that bit into the storage register.
```c
digitalWrite(dataPin, pinState);
digitalWrite(clockPin, HIGH);
```
Finally the Clock Pin is set to low to ensure no further bit writes.
```c
digitalWrite(clockPin, LOW);
```
So, in simple terms, this section of code looks at each of the 8 bits of the value in dataOut one by one and sets the data pin to HIGH or LOW accordingly, then writes that value into the storage register.

This is simply sending the 8-bit number out to the 595 one bit at a time. Then the main loop sets the latch pin to HIGH to send out those 8 bits simultaneously to pins 15 and 1 to 7 (QA to QH) of the shift register, thus making our 8 LEDs show a visual representation of the binary number stored in the shift register.

Your brain may hurt after this Project, so take a rest, stretch your legs, and take another deep breath before you dive into Project 18, in which we will now use two shift registers daisy-chained together.
