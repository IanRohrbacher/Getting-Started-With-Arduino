# Lessons on Arduino
Arduino is an open-source electronics platform that makes it easy to create interactive projects. With Arduino, 
you can control LEDs, motors, sensors, usiong  C++, *with just a few tiny differences*, in the Arduino IDE. It's 
widely used for prototyping and robotics making it a great tool for anyone to use.
<br><br>
The following is a introduction to the basic skill you will need to know get started. There will be less emphasis 
on the electrical wiring but we will go over hardware and software.

## Gettting Started
We will use [wokwi](https://wokwi.com/) for simulation. Once you open it feel free to make a account if you 
want to save your work. We will be using the [ESP32](https://wokwi.com/esp32) environment. The ESP32 is currently 
one of the best microcontroler you can work with. Again we won't dive into the electronal aspect of the ESP32, but 
it is importent to know the (pinout sheet)[] of the board. This will tell you what the numbers are for your pins are 
and what the pins are able to do what. We don't have to worry too much however because most of the pins are pwm allowing 
easy converstion from analog to digital.

### New Project
After selecting the ESP32 simulator, scroll down untill you find 'Starter Templates' and select 'ESP32'. Welcom to your 
first project! Here you will see a few essentials; 'setup()', 'loop()', 'Serial', 'delay()'.
* setup(): Runs once at the beginning, or on startup. Here we will define importent variable like pins and serial communication.
* loop(): Runs continuously while active. Here is where all executing logic goes.
* Serial: Used as a output stream.
> [!IMPORTANT] 
> Always check your microcontroler for which port number to use, standerd is 9600 however the simutator uses 115200. 
* delay(): This is your 'wait' statment. This is used to limit the amout of cycles the 'loop()' will make to ensure memory isn't being wasted.

## Digital Signals
# Output

# Input


## Analog Signals
# Output

# Input
