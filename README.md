# Lessons on Arduino
Arduino is an open-source electronics platform that makes it easy to create interactive projects. With Arduino, you can control LEDs, motors, sensors, usiong  C++, *with just a few tiny differences*, in the Arduino IDE. It's widely used for prototyping and robotics making it a great tool for anyone to use.
<br><br>
The following is a introduction to the basic skill you will need to know get started. There will be less emphasis on the electrical wiring but we will go over hardware and software.

# Gettting Started
We will use [wokwi](https://wokwi.com/) for simulation. We will be using the [ESP32](https://wokwi.com/esp32) environment. The ESP32 is currently one of the best microcontroler you can work with. Again we won't dive into the electronal aspect of the ESP32, but it is importent to know the (pinout sheet)[] of the board. This will tell you what the numbers are for your pins are and what the pins are able to do what. We don't have to worry too much however because most of the pins are pwm allowing easy converstion from analog to digital.

# New Project
After selecting the ESP32 simulator, scroll down untill you find 'Starter Templates' and select 'ESP32'. Welcom to your first project! Here you will see a few essentials; 'setup()', 'loop()', 'Serial', 'delay()'.
```ino
void setup() {
  // put your setup code here, to run once:
  Serial.begin(115200); // starts a serial connection
  while(!Serial){} // wait until Serial is created
  Serial.println("Hello, ESP32!"); // prints to the serial
}

void loop() {
  // put your main code here, to run repeatedly:
  delay(10); // this speeds up execution time
}
```
* setup(): Runs once at the beginning, or on startup. Here we will define importent variable like pins and serial communication.
* loop(): Runs continuously while active. Here is where all executing logic goes.
* Serial: Used as a output stream.
> [!IMPORTANT] 
> Always check your microcontroler for which port number to use, standerd is 9600 however the simutator uses 115200. 
* delay(): This is your 'wait' statment. This is used to limit the amout of cycles the 'loop()' will make to ensure memory isn't being wasted.

<br><br><br><br>
# Digital Signals
A digital signal is a type of signal that has only two discrete states: HIGH (1) or LOW (0). In Arduino, digital signals are used to communicate with components like LEDs, buttons, motors, and sensors, *however with limited control.*
![digital](images/digital.png)
<br><br>

## Output
To start, open the diagram.json file in the simulator and copy [this json](digital/write-one.json). You should see a circit with a LED. Lets learn how to control this LED.
<br>
Lets first define a varable for the pin, it is currently connected to pin 19.
```ino
int pin = 19;

void setup() {
  Serial.begin(115200);
  while(!Serial){}
  Serial.println("Hello, ESP32!");
}

void loop() {
  delay(10);
}
```
Next we need to let the microcontroller, *ESP32*, know how to hanndle this pin. We can use pinMode with the parameter of the pin and OUTPUT telling it we will be outputting though this pin.
```ino
void setup() {
  Serial.begin(115200);
  while(!Serial){}
  Serial.println("Hello, ESP32!");

  pinMode(pin, OUTPUT);
}
```
Now we can give the pin a state. To do this we can use digitalWrite with the parameter of the pin and the current state we want the LED to be in. For digital output we can anly use HIGH and LOW.
```ino
void setup() {
  Serial.begin(115200);
  while(!Serial){}
  Serial.println("Hello, ESP32!");

  pinMode(pin, OUTPUT);
  digitalWrite(pin, HIGH);
}
```
Now that we know how to use the LED let us add some basic logic into the loop() to toggle the LED on and off after some time.
```ino
int pin = 19;
bool toggle = false;

void setup() {
  Serial.begin(115200);
  while(!Serial){}
  Serial.println("Hello, ESP32!");

  pinMode(pin, OUTPUT);
}


void loop() {
  digitalWrite(pin, toggle ? HIGH : LOW);

  Serial.println(toggle);
  toggle = !toggle;
  
  delay(1500);
}
```

## Input
Now that we know how change the LED in code, lets add a button to control it insead. Open the diagram.json file in the simulator and copy [this json](digital/read-one.json). You should the circit with a LED now with a button.
<br>
Let us redifine the LED pin and add a button pin that is connected to pin 14. Let us also set the pinMode to the button to INPUT so we can read the state.
```ino
int LED_pin = 19;
int Button_pin = 14;

void setup() {
  Serial.begin(115200);
  while(!Serial){}
  Serial.println("Hello, ESP32!");

  pinMode(LED_pin, OUTPUT);
  pinMode(Button_pin, INPUT);
}
```
To read from the button we use digitalRead. This will return the state of the button, HIGH or LOW. Now let us modify our logic from before and have digitalRead cast to a boolean.
```ino
void loop() {
  bool pressed = digitalRead(Button_pin);
  digitalWrite(LED_pin, pressed ? HIGH : LOW);

  Serial.println(pressed);
  
  delay(100);
}
```
As you can see the digitalRead and digitalWrite are now inside the loop() allowing it to update every tick, defined by the delay.
<br>
If you ran this you might have noticed something, the buttton inconsistently updates the LED. This is due to the mircocontroller flowing unsteady elecreicty though the button. We can fix the by changing the INPUT to INPUT_PULLUP which will ensure a HIGH stream of eletricty to flow.
```ino
  pinMode(Button_pin, INPUT_PULLUP);
```

### For Fun
I have created a model that uses three LEDs that change one after eachother [here is the json](digital/digital-three).
```ino
int pinR = 18;
int pinG = 5;
int pinB = 17;
int pinList[] = {pinR, pinG, pinB};
int state = 0;

void setup() {
  Serial.begin(115200);
  while(!Serial){}
  Serial.println("Hello, ESP32!");

  for(int pins : pinList) {
    pinMode(pins, OUTPUT);  // Use a valid GPIO number
  }
}


void loop() {
  digitalWrite(pinR, state==0 ? HIGH : LOW);
  digitalWrite(pinG, state==1 ? HIGH : LOW);
  digitalWrite(pinB, state==2 ? HIGH : LOW);

  Serial.println(state);
  state = (state == 2 ? 0 : state+1);
  
  delay(1500);
}
```


<br><br><br><br>
# Analog Signals
An analog signal is a continuous signal that varies over time and can take any value within a given range. Unlike digital signals that are either HIGH (1) or LOW (0), analog signals can have infinite possible values. Analog signals are able to use the same components as digital; LEDs, buttons, motors, and sensors, however with finer control.
![analog](images/analog.png)
<br><br>

## Output
We will start once again with editing the diagram.json file, use [this json](analog/write-one.json). This diagram has a potentiometer in the form of a slide lever. A potentiometer is a variable resistor that allows you to adjust resistance manually. We can read this change as a integer inside the program. For a ESP32 the range of any potentiometers is 0-4095.
<br>
Running this program allows you to see the change in-real time.
```ino
int pot = 14;

void setup() {
  Serial.begin(115200);
  while(!Serial){}
  Serial.println("Hello, ESP32!");

  pinMode(pot, INPUT);
}

void loop() {
  int value = analogRead(pot);
  Serial.println(value);

  delay(100);
}
```
Now lets change the output with one simple trick, map()! Using a map allows you to modify the input range and output range. The first parameter is your potentiometer, the second and third is the range of your potentiometer, 0-4095, and the fourth and five parameter is the new range.
```ino
void loop() {
  int value = map(analogRead(pot), 0, 4095, 0, 100);
  Serial.println(value);

  delay(100);
}
```

## Input
Now that we know how read the potentiometer, lets see how we can use it. Open the diagram.json file in the simulator and copy [this json](analog/read-one.json). You should the circit with teh potentiometer and now a LED.
<br>
Using the value from the potentiometer we can change how bright the LED is. We can use  analogWrite for this. The range for analogWrite is 0-255 so lets make sure the potentiometer matches.
```ino
int pot = 14;
int LED = 19;

void setup() {
  Serial.begin(115200);
  while(!Serial){}
  Serial.println("Hello, ESP32!");
  
  pinMode(pot, INPUT);
  pinMode(LED, OUTPUT);
}

void loop() {
  int value = map(analogRead(pot), 0, 4095, 0, 255);
  analogWrite(LED, value);
  
  Serial.println(value);
  delay(100);
}
```

### For Fun
I have created a array of LEDs in the RGB colors along with a RGB LED. Potentiometer come in all styles too, so here I am using a nob. Each nob controls the brightness of their respective RGB colors and mixing them you can see how the RGB LED reacts. [Here is the json](analog/analog-RGB)
```ino
int RPin = 19;
int GPin = 18;
int BPin = 5;
int ponRPin = 26;
int ponGPin = 27;
int ponBPin = 14;

void anode(int R, int G, int B) {
  analogWrite(RPin, map(R, 0, 4095, 0, 255));
  analogWrite(GPin, map(G, 0, 4095, 0, 255));
  analogWrite(BPin, map(B, 0, 4095, 0, 255));
}

void printRGB(int R, int G, int B) {
  Serial.println("-------------------------------");
  Serial.println("R:"+String(map(R, 0, 4095, 0, 255)));
  Serial.println("G:"+String(map(G, 0, 4095, 0, 255)));
  Serial.println("B:"+String(map(B, 0, 4095, 0, 255)));
}

void setup() {
  Serial.begin(115200);
  while(!Serial){}
  Serial.println("Hello, ESP32!");

  pinMode(ponRPin, INPUT);
  pinMode(ponGPin, INPUT);
  pinMode(ponBPin, INPUT);

  pinMode(RPin, OUTPUT);
  pinMode(GPin, OUTPUT);
  pinMode(BPin, OUTPUT);
}

void loop() {
  int red = analogRead(ponRPin);
  int green = analogRead(ponGPin);
  int blue = analogRead(ponBPin);

  anode(red, green, blue);
  printRGB(red, green, blue);

  delay(100);
}
```

## Servos
Another basic componet are servos. They are a type of motor that allows for precise control of angular position, speed, and torque. The unige thing about servos are that they can hold a position rather than continuously rotating.
<br>
 [Diagram here](servo/servo-one).







