##What You Will Need
In order to follow along with the projects in this book, you will need various components. To carry out all of the projects will require purchasing a lot of parts first. This could be expensive, so I suggest that you start by purchasing the components for the projects in the first few chapters and obtain the parts listed at the start of the project pages. As you progress through the book, you can obtain the parts needed for subsequent projects.

There are a handful of other items you will need or may find useful. Of course, you will need to obtain an Arduino board or one of the many clone boards on the market such as the Freeduino, Seeeduino (yes, there really are three eees), Boarduino, Sanguino, Roboduino or any of the other “duino” variants. These are all fully compatible with the Arduino IDE, Arduino Shields and everything else that you can use with an official Arduino Board. Remember that the Arduino is an Open Source project; therefore anyone is free to make a clone or other variant of the Arduino. However, if you wish to support the development team of the original Arduino board, get an official board from one of the recognized distributors. For the projects in this book, we will be using an Arduino Uno, although any of the available Arduino boards will work just as well.
You will need access to the Internet to download the Arduino IDE, the software used to write your Arduino code and upload it to the board, and also to download the Code Samples within this book (if you don’t want to type them out yourself), as well as any code libraries that may be necessary to get your project working.

You will also need a well-lit table or other flat surface to lay out your components; this will need to be next to your desktop or laptop PC to enable you to upload the code to the Arduino. Remember that you are working with electricity (although it is low voltage DC), and therefore metal tables or surfaces will need to be covered in a non-conductive material such as a tablecloth or paper before laying out your materials. Also of some benefit, although not essential, may be a pair of wire cutters, a pair of long-nosed pliers, and a wire stripper. A notepad and pen will also come in handy for drawing out rough schematics and working out concepts and designs.

Finally, the most important thing you will need is enthusiasm and a willingness to learn. The Arduino is designed as a simple and cheap way to get involved in microcontroller electronics and nothing is too hard to learn if you are willing to give it a go. This book will help you on that journey, and introduce you to this exciting and creative hobby.


Setting Up Your Arduino
This section will explain how to set up your Arduino and the IDE for the first time. The instructions for both Windows and Macs are given. If you use Linux, then refer to the Getting Started instructions on the Arduino website at http://playground.arduino.cc/learning/linux. I will also presume you are using an Arduino Uno (see Figure 1-4), Duemilanove, Nano, Diecimila or Mega 2560 (or their equivalent clone) and are installing on either Windows 7 or a recent version of OSX (Lion or Mountain Lion). If you have a different type of board, then refer to the corresponding page in the Getting Started guide of the Arduino website.
9781430250166_Fig01-04.jpg

Figure 1-4. An Arduino Uno (Image courtesy of Earthshine Electronics)

You will also need a USB cable (A to B plug type) which is the same kind of cable used for most modern USB printers. If you have an Arduino Nano, you will need a USB A to Mini-B cable instead.
Next, you need to download the Arduino IDE. This is the software you will use to write your programs (or sketches) and upload them to your board. For the latest IDE go to the Arduino download page at http://arduino.cc/en/Main/Software and obtain appropriate the version for your operating system.
If you have a Mac, once the Zip file has downloaded, unzip it and then you will see the Arduino icon. Drag it across to the Applications folder and drop it in there to install the program. You simply double-click the icon to start it. For Windows, download the ZIP file and, once complete, unzip it. Then put the unzipped folder in a place that suits you, keeping the directory structure in place.
Now you need to connect your Arduino board before installing the drivers and software. Connect the USB cable to the Arduino and plug the other end into a USB socket on your computer. You will see the green Power LED (marked PWR) light up on your board to show you it has power. If you are on a Mac, then there are no drivers to install. If you are on Windows, then it will now attempt to install the drivers for the Arduino. This auto attempt will fail and you will get a message that the “Device driver software was not successfully installed” (Figure 1-5); do not worry about this.
9781430250166_Fig01-05.jpg

Figure 1-5. The automatic attempt by Windows to install the drivers will fail. This is normal

Click on the Start Menu and then click on Control Panel. Navigate to System and Security, click on System, and then open the Device Manager. On the list of hardware underneath Other Devices, you should see something similar to Figure 1-6 in which you have “Arduino Uno” with a yellow hazard icon over it.
9781430250166_Fig01-06.jpg

Figure 1-6. The Windows Device Manager

Right click on the Arduino Uno icon in the list and choose “Update Driver Software” (Figure 1-7).
9781430250166_Fig01-07.jpg

Figure 1-7. Right click and choose ”Update Driver Software”

Now choose “Browse my computer for driver software”.
9781430250166_Fig01-08.jpg

Figure 1-8. Click on “Browse my computer for driver software”

Next, browse to the driver folder of the Arduino installation, and then click the Next button. Windows will now finish the driver installation. If you get a message that says “Windows can’t verify the publisher of this driver software” then click the “Install this driver software anyway.” If you have a Mac, then there are no drivers to install.
Now that the drivers are installed, you are ready to open up the Arduino IDE. For Windows, double-click the arduino.exe file inside the unzipped Arduino folder. For a Mac, click the Arduino icon in the Applications folder. The IDE will now open up and present you with a blank sketch as in Figure 1-9.
9781430250166_Fig01-09.jpg

Figure 1-9. The Arduino IDE

Next, open up an example sketch to test out the IDE and the Arduino. Click File, then Examples, then 01.Basics, and finally, Blink (see Figure 1-10).
9781430250166_Fig01-10.jpg

Figure 1-10. The Arduino File Menu. Choose the Blink sketch

This will load the Blink example sketch into the IDE and will look something like Figure 1-11.
9781430250166_Fig01-11.jpg

Figure 1-11. The IDE with the Blink sketch loaded

Next, you will need to select your board from the list (see Figure 1-12) in Tools image Board. For an Arduino Uno, select this from the top of the list. If you have an older Arduino Duemilanove or clone with an Atmega328 chip, you will need to select Arduino Duemilanove or Nano w/ Atmega328. If you have an even older board with an Atmega168 chip, select Arduino Diecimila, Duemilanove, or Nano w/ ATmega168, or you may even have a Leonardo, Mega or a DUE. Choose whichever board matches yours.
9781430250166_Fig01-12.jpg

Figure 1-12. Select your board type

Select the serial device of the Arduino board from Tools image Serial Port (see Figure 1-13). If you are not sure what your port is, disconnect the Arduino and check the ports available, then reconnect the Arduino and see which port has now appeared (you may need to close and reopen the menu to get it to show).
![Figure 1-13. Select the port](http://techbus.safaribooksonline.com/getfile?item=MnI0czNhNjc1czZhZDAvaThnL2Mxczl0L3BtZTEwMHMwMTJnXzExMGZlZTVpYTRzN3QzOThpbS9nLzY2My1nMWpwLg--)

Figure 1-13. Select the port
