#Serial Controlled Mood Lamp
Here we will revisit the circuit from Project 8, RGB Mood Lamp, but will now delve into the world of serial communications and control our lamp by sending commands from the PC to the Arduino using the serial monitor in the Arduino IDE. This project also introduces how we manipulate text strings. So set up the hardware as in Project 8 and enter the new code.

####Enter the Code
Open up your Arduino IDE, and type in the code from Listing 3-6:
Listing 3-6.  Code for Project 10

```c
char buffer[18];
int red, green, blue;

int RedPin = 11;
int GreenPin = 10;
int BluePin = 9;

void setup()
{
        Serial.begin(9600);
        while(Serial.available())
                Serial.read();
        pinMode(RedPin, OUTPUT);
        pinMode(GreenPin, OUTPUT);
        pinMode(BluePin, OUTPUT);
}

void loop()
{
        if (Serial.available() > 0) {
                int index=0;
                delay(100); // let the buffer fill up
                int numChar = Serial.available();
                if (numChar>15) {
                        numChar=15;
                }
                while (numChar--) {
                        buffer[index++] = Serial.read();
                }
                splitString(buffer);
        }
}

void splitString(char* data) {
        Serial.print("Data entered: ");
        Serial.println(data);
        char* parameter;
        parameter = strtok (data, " ,");          // Note that this is a space before the comma in " , "
        while (parameter != NULL) {
                setLED(parameter);
                parameter = strtok (NULL, " ,");  // space before the comma in " , "
}

         // Clear the text and serial buffers
        for (int x=0; x<16; x++) {
                buffer[x]='\0';
        }
        while(Serial.available())
                Serial.read();}

void setLED(char* data) {
        if ((data[0] == 'r') || (data[0] == 'R')) {
                int Ans = strtol(data+1, NULL, 10);
                Ans = constrain(Ans,0,255);
                analogWrite(RedPin, Ans);
                Serial.print("Red is set to: ");
                Serial.println(Ans);
        }
        if ((data[0] == 'g') || (data[0] == 'G')) {
                int Ans = strtol(data+1, NULL, 10);
                Ans = constrain(Ans,0,255);
                analogWrite(GreenPin, Ans);
                Serial.print("Green is set to: ");
                Serial.println(Ans);
        }
        if ((data[0] == 'b') || (data[0] == 'B')) {
                int Ans = strtol(data+1, NULL, 10);
                Ans = constrain(Ans,0,255);
                analogWrite(BluePin, Ans);
                Serial.print("Blue is set to: ");
                Serial.println(Ans);
        }
}
```

Once you’ve verified the code, upload it to your Arduino.

Now when you upload the program, nothing seems to happen. This is because the program is waiting for your input. Start the serial monitor by clicking its icon in the Arduino IDE taskbar.

In the serial monitor text window, you can now enter the R, G, and B values for each of the three LEDs manually; the LEDs will change to the color you have input. For example, if you enter R255, the Red LED will display at full brightness. If you enter R255, G255, then both the red and green LEDs will display at full brightness.

Now enter R127, G100, B255 and you will get a nice purplish color. If you type r0, g0, b0, all the LEDs will turn off.
The input text is designed to accept both a lower-case or upper-case R, G, and B and then a value from 0 to 255. Any values over 255 will be dropped down to 255 maximum. You can enter a comma or a space in between parameters and you can enter 1, 2, or 3 LED values at any one time. For example:

r255 b100

r127 b127 g127

G255, B0

B127, R0, G255

and so forth.

#Code Overview
This project introduces a whole bunch of new concepts, including serial communication, pointers, and string manipulation. So, hold on to your hats; this will take a lot of explaining.

First we set up an array of char (characters) to hold our text string. We have made it 18 characters long, which is longer than the maximum of 16 we will allow to ensure we don’t get “buffer overflow” errors.

char buffer[18];

We then set up the integers to hold the red, green, and blue values, as well as the values for the digital pins.
```c
int red, green, blue;

int RedPin = 11;
int GreenPin = 10;
int BluePin = 9;
```

In our setup function, we set the three digital pins to be outputs. But, before that we have the Serial.begin command.

```c
void setup()
{
        Serial.begin(9600);
        while(Serial.available())
                Serial.read();
        pinMode(RedPin, OUTPUT);
        pinMode(GreenPin, OUTPUT);
        pinMode(BluePin, OUTPUT);
}
```

Serial.begin tells the Arduino to start serial communications and the number within the parenthesis, in this case 9600, sets the baud rate (characters per second) at which the serial line will communicate.
Next,
```c
while(Serial.available())
        Serial.read();
```
will flush out any characters that happen to be in the serial line so that it is empty and ready for input/output. It does this by checking whether there is any serial data available (in our case, junk data) and if so, reading it. The data read clears out any data held in the serial buffer.

The serial communications line is simply a way for the Arduino to communicate with the outside world, in this case, to and from the PC and the Arduino IDE’s serial monitor.

In the main loop we have an if statement. The condition it is checking for is
```c
if (Serial.available() > 0) {
```
The Serial.available command checks to see if any characters have been received on the serial line. If any characters have been received, then the condition is met and the code within the if statements code block is now executed. The Serial.available() command returns the number of characters waiting to be read.
```c
if (Serial.available() > 0) {
        int index=0;
        delay(100); // let the buffer fill up
        int numChar = Serial.available();
        if (numChar>15) {
                numChar=15;
        }
        while (numChar--) {
                buffer[index++] = Serial.read();
        }
        splitString(buffer);
}
```
An integer called index is declared and initialized as zero. We will use index to keep track of our position within the character array.

We then set a delay of 100. The purpose of this is to give time for the full command to be received before we carry on and process the data. If we don’t do this, it is possible that the code will start to process the text string before we have received all of the data. The serial communications line is very slow compared to the speed at which the rest of the code is executing. When you send a string of characters, the Serial.available function will immediately have a value higher than zero and the “if” statement will start to execute. If we didn’t have the delay(100) statement in there, the Arduino could start to execute the code within the if statement before all of the text string had been received and the serial data processed may only be the first few characters of the line of text entered.
After we have waited 100 ms for the full text string to be received, we then declare and initialize the numChar integer to be the number of characters within the text string. For example, If we sent this text in the serial monitor:
R255, G255, B255

Then the value of numChar would be 17. It is 17, and not 16, as at the end of each line of text there is an invisible character called a NULL character. This is a “nothing” symbol and simply tells the Arduino that the end of the line of text has been reached.

The next if statement checks if the value of numChar is greater than 15 or not, and if it is, it sets it to be 15. This ensures that we don’t overflow the array char buffer[18];

After this comes a while command. This is something we haven’t come across before, so let me explain. We have already used the for loop, which will loop a set number of times. The while statement is also a loop, but one that executes only while a condition is true.

The syntax is
```c
while(expression) {
        // statement(s)
}
```

In our code the while loop is
```c
while (numChar--) {
      buffer[index++] = Serial.read();
}
```
The condition it is checking is simply numChar, so, in other words, it is checking that the value stored in the integer numChar is not zero. numChar has—after it. This is what is known as a post-decrement. This simply means that the numChar is decremented AFTER it is tested for the while(). If we had used –numChar, the value in numChar would be decremented (have one subtracted from it) before it was evaluated. In our case, the while loop checks the value of numChar and then subtracts one from it. If the value of numChar was not zero before the decrement, it then carries out the code within its code block.

numChar is set to the length of the text string that we have entered into the serial monitor window. So, the code within the while loop will execute that many times. The code within the while loop is
buffer[index++] = Serial.read();

This sets each element of the buffer array to each character read in from the serial line. In other words, it fills up the buffer array with the letters we have entered into the serial monitor’s text window.
The Serial.read() command reads incoming serial data, one byte at a time. So now that our character array has been filled with the characters we entered in the serial monitor, the while loop will end once numChar reaches zero (i.e., the length of the string).

After the while loop we have

```c
splitString(buffer);
```

This is a call to the function we have created called `splitString()`. The function looks like this:
```c
void splitString(char* data) {
        Serial.print("Data entered: ");
        Serial.println(data);
        char* parameter;
        parameter = strtok (data, " ,");
        while (parameter != NULL) {
                setLED(parameter);
                parameter = strtok (NULL, " ,");
        }

        // Clear the text and serial buffers
        for (int x=0; x<16; x++) {
                buffer[x]='\0';
        }
        Serial.flush();
}
```
We don’t plan to return any data from the function, so its data type has been set to void. We pass the function one parameter and that is a char data type that we have called data. Instead of passing all the elements within the array to the function, C passes a pointer to (in other words, the location in memory of) the first element of the array. (This is known as pass by reference rather than pass by value.) So our declaration of the function needs to accept a pointer argument. We tell the compiler that it is a pointer variable by adding an asterisk * to the front of the variable name.
Pointers in a Nutshell

Pointers are quite an advanced subject in C, so we won’t go into too much detail about them. If you need to know more, then refer to a book on programming in C. All you need to know for now is that by declaring “data” as a pointer, it is simply a variable that points to another variable.

The type in the pointer declaration tells what type of data the pointer points to.

char *mytext; 

declares mytext to be a pointer that can point to char (character) data.

int *nextnumber; 

declares nextnumber to be a pointer that can point to integers.

Pointers have to be initialized before they can be used. We can set the pointer to the location of another variable, array, or array element by using the & (address of) operator to get the address of the variable or element we want to point to.

We can assign one pointer variable to another pointer variable of the same type. They will then both point to the same thing. We can get the value stored at the memory location the pointer currently points to by using the * (dereference)operator. For example:

```c
char mychar;                            // declares a simple variable that can hold a character.
char buff[] = {'a','b','c','d'};        // declares an array of characters
char *mytext;                           // declares a variable that can point to character data
mytext = &buff[1];                      //  initializes mytext to point to the location of                                                                               buff[1] in memory;
mychar = *mytext;                       //  retrieves the character that mytext points to                                                                          (stored in buff[1]) and
                                        //  copies that character (in this case, 'b') to mychar.
```
Incrementing or decrementing a pointer causes it to point to the memory location following or preceding the location it pointed to. So if it points to an array element, incrementing the pointer causes it to point to the next array element.

mytext++;              //  mytext now points to buff[2];

We can change the contents of the memory the pointer points to by dereferencing the pointer on the left side of an assignment statement.

*mytext = ‘g’; 

stores a ‘g’ in the location mytext points to. (buff[2], after the lines above).

A special value (NULL) is used to represent a non-valid (empty) pointer value. If the value of a pointer is NULL, attempts to fetch or store a value at what it points to are undefined (in other words, may cause your program to stop working). Search functions will frequently return a pointer value of NULL to indicate they couldn’t find what you asked them to search for.

Since pointers frequently point to arrays, the language lets you subscript a pointer just like an array.
mytext[1] refers to the value stored in the first location after the location mytext currently points to. If mytext points to buff[2], mytext[1] refers to the contents of buff[2+1] (buff[3]), which holds ’d’ in this case. mytext[0] is the same as *mytext.

When we called splitString, we sent it the contents of “buffer” (actually a pointer to it as we saw above).
splitString(buffer);
So we have called the function and passed it (by reference) the entire contents of the buffer character array.
The first command is

Serial.print("Data entered: ");


and this is our way of sending data back from the Arduino to the PC. In this case, the print command sends whatever is within the parenthesis to the PC, via the USB cable, where we can read it in the serial monitor window. We have sent the words “Data entered: ”. Text must be enclosed within quotes “”. The next line is similar

Serial.println(data);

and again we have sent data back to the PC; this time, we send the character pointer variable called data. The character pointer variable we have called “data” points to the contents of the “buffer” character array that we passed to the function. So, if our text string entered was

R255 G127 B56
Then the
Serial.println(data);

command will send that text string back to the PC and print it out in the serial monitor window.
This time, the print command has ln on the end to make it println. This simply means “print” and then advance to the next line.

When we print using the print command, the cursor (the point at where the next symbol will appear) remains at the end of whatever we have printed. When we use the println command, a carriage return and linefeed follow our text, causing the cursor to drop down to the next line after our text is printed.
```c
Serial.print("Data entered: ");
Serial.println(data);
```

If we look at our two print commands, the first one prints out “Data entered: “ and then the cursor remains at the end of that text. The next print command will print “data,” or in other words, the contents of the array called “buffer,” and then issue a linefeed, or drop the cursor down to the next line. If we issue another print or println statement after this, whatever is printed in the serial monitor window will appear on the next line underneath the last.
We then create a new char pointer variable called parameter

Char* parameter;

and as we are going to use this variable to access elements of the “data” array, it must be the same type, hence the * symbol. You cannot pass data from one data type variable to another as the data must be converted first. This variable is another example of one that has “local scope.” It can be “seen” only by the code within this function. If you try to access the parameter variable outside of the splitString function, you will get an error.
We then use a strtok command, which is a very useful command which enables us to manipulate text strings. Strtok gets its name from String and Token as its purpose is to split a string using delimiters. In our case, the delimiter it is looking for is a space or a comma. It is used to split text strings into smaller strings called tokens.
We pass the “data” array to the strtok command as the first argument and the delimiters (enclosed within quotes) as the second argument. Hence

parameter = strtok (data, " ,");

And it splits the string at that point. So we are using it to set “parameter” to be the part of the string up to a space or a comma.

So, if our text string was

R127 G56 B98

Then after this statement the value of “parameter” will be

R127

as the strtok command would have split the string up to the first occurrence of a space of a comma.
After we have set the variable “parameter” to the part of the text string we want to strip out (i.e., the part up to the first space or comma), we then enter a while loop whose condition is that parameter is not empty (i.e., we haven’t reached the end of the string) using

```c
while (parameter != NULL) {
```
Within the loop we call our second function
```c
setLED(parameter);
```

We will look at this later on. Then it sets the variable “parameter” to the next part of the string up to the next space or comma. We do this by passing to strtok a NULL parameter
```c
parameter = strtok (NULL, " ,");
```

This tells the strtok command to carry on where it last left off.
So this whole part of the function

```c
char* parameter;
parameter = strtok (data, " ,");
while (parameter != NULL) {
        setLED(parameter);
        parameter = strtok (NULL, " ,");
}
```

is simply stripping out each part of the text string that is separated by spaces or commas and sending that part of the string to the next function called setLED().
The final part of this function simply fills the buffer array with NULL character, which is done with the \0 symbol, and then flushes the serial data out of the serial buffer, which is then ready for the next set of data to be entered.

```c
// Clear the text and serial buffers
for (int x=0; x<16; x++) {
        buffer[x]='\0';
}
while(Serial.available())
 Serial.read();
```

The setLED function is going to take each part of the text string and set the corresponding LED to the color we have chosen. So, if the text string we enter is

G125 B55

then the splitString() function splits that into the two separate components

G125
B55

and sends that shortened text string onto the setLED() function, which will read it, decide which LED we have chosen, and set it to the corresponding brightness value.
So let’s take a look at the second function called setLED().

```c
void setLED(char* data) {
        if ((data[0] == 'r') || (data[0] == 'R')) {
                int Ans = strtol(data+1, NULL, 10);
                Ans = constrain(Ans,0,255);
                analogWrite(RedPin, Ans);
                Serial.print("Red is set to: ");
                Serial.println(Ans);
        }
        if ((data[0] == 'g') || (data[0] == 'G')) {
                int Ans = strtol(data+1, NULL, 10);
                Ans = constrain(Ans,0,255);
                analogWrite(GreenPin, Ans);
                Serial.print("Green is set to: ");
                Serial.println(Ans);
        }
        if ((data[0] == 'b') || (data[0] == 'B')) {
                int Ans = strtol(data+1, NULL, 10);
                Ans = constrain(Ans,0,255);
                analogWrite(BluePin, Ans);
                Serial.print("Blue is set to: ");
                Serial.println(Ans);
        }
}
```

We can see that this function contains three very similar if statements. We will therefore take a look at just one of them as the other two are almost identical.

```c
if ((data[0] == 'r') || (data[0] == 'R')) {
    int Ans = strtol(data+1, NULL, 10);
    Ans = constrain(Ans,0,255);
    analogWrite(RedPin, Ans);
    Serial.print("Red is set to: ");
    Serial.println(Ans);
}
```
The if statement checks that the first character in the string (data[0]) is either the letter r or R (upper case and lower case characters are totally different as far as C is concerned). We use the logical OR command whose symbol is || to check if the letter is an r OR an R, as it is fine to enter the r in either upper or lower case.


If it is an r or an R, then the if statement knows we wish to change the brightness of the Red LED and so the code within executes. First we declare an integer called Ans (which has scope local to the setLED function only) and use the strtol (String to long integer) command to convert the characters after the letter R to an integer. The strtol command takes three parameters. These are the string we are passing it, a pointer to a variable where strtol( ) can store the character after the number string (which we don’t use as we have already stripped the string using the strtok command and hence pass a NULL pointer) and then the “base,” which in our case is base 10. The string we are passing it will contain decimal digits (as opposed to binary, octal, or hexadecimal digits which would be base 2, 8, and 16 respectively). So, in other words, we declare an integer and set it to the value of the text string after the letter R (or the numeric portion of it).


Next, we use the constrain command to make sure that Ans goes from 0 to 255 and no more. We then carry out an analogWrite command to the red pin and send it the value of Ans. The code then sends out “Red is set to: ” followed by the value of Ans back to the serial monitor. The other two if statements do exactly the same thing, but for the green and the blue LEDs.

We have covered a lot of ground and a lot of new concepts in this project. To make sure you understand exactly what is going on in this code, I am going to set the project code side by side with pseudo-code (the computer language translated into a language humans can understand).

![code](https://cloud.githubusercontent.com/assets/3673943/3586470/cf80eebc-0c30-11e4-8aa3-4c98154184be.JPG)

Hopefully, you can use this “pseudo-code” to make sure you understand exactly what is going on in this project’s code.
We are now going to leave LEDs behind for a little while and look at how to make sounds from your Arduino using a piezo sounder.

