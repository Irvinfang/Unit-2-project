
![CoverPhoto](mars.jpg)

Unit 2 project: Communication to the Moon and Mars
==================================================

Creating a communication and translation software system to communicate between the Earth, Moon and Mars.


Contents
-----
  1. [Planning](#planning)
  1. [Design](#design)
  1. [Development](#development)
  1. [Evalution](#evaluation)
  1. [Improvements](#improvements)


Planning
----------
### Definition of the problem
There are 3 different stations, each being on a different planet/moon in the solar system. One is on Earth, one is on the Moon and the last one is on Mars. The station on Earth can only communicate using Morse code, the station in the Moon can only communicate in Binary code. Communication with the station in Mars must be provided. This will be tested here on campus, between 3 different houses.

Any station can only communicate in their "language". For example, the station on Mars must communicate back to the Moon through binary. Also, its important that the station on Earth **can't** communicate with the station on Mars, it had to go through the Moon.

Another challenge is that none of the people at the stations know morse or binary by heart, meaning everything has to be translated to and from English, and be received as English as well.

The last requirement of the system is that the input method of English characters to create the message only uses **2** push buttons. 

### Justification
**Bash vs. Arduino (C)**

Bash:

Pros
- Highly useful for scripting when running code directly from terminal to perform simple, repetitive jobs
- Gives users a lot of control over the system being used
- Widely adopted

Cons
- Unsatisfactory, complicated syntax
- Small mistakes of little relevance
- Can only run in specific environments, not including Arduino hardwares


Arduino (C):

Pros
- Industry standard, commonly used
- Lower level (closer to the computer hardware)
- Relatively simple
- Very optimized and fast

Cons
- Syntax-wise isn't as simple compared to other high level languages

Why I chose Arduino (C)
Both programming languages have their pros and cons, and it's hard to compare which one's better since they're used in different areas. When choosing the language for this project, the only option is Arduino (C), because it is the only option that meets the hardware requirements, functionality, and usability. Also because Arduino is more cheap and easy to use compared to other hardware.


Success Criteria
1. The system has an easy user interface
2. The message can be sent out perfectly throughout the different languages
3. The message can be sent out perfectly from the sender to the receiver
4. The system is easy to install
5. All messages have to be input from English

Below is the test plan to check if all functions meet the success criteria:

| Test plan                                                                                      | Input | Output | Check |
|------------------------------------------------------------------------------------------------|-------|--------|-------|
| 1. The system has an easy user interface                                                       |       |        |       |
| 2. The message can be sent out perfectly throughout the different languages                    |       |        |       |
| 3. The message can be sent out perfectly from the sender to the receiver                       |       |        |       |
| 4. The system is easy to install                                                               |       |        |       |
| 5. All messages have to be input from English                                                  |       |        |       |

Design
----------

A diagram of the flow of information looks like this:
![PlanetDiagram](planetDiagram.png)
Fig. A. Diagram of the flow of information

A system diagram of this project looks like this:
![SystemDiagram](SystemDiagram.jpg)
Fig. 2. System diagram of this project

Explaining the system diagram:
1. Message should be sent in English string, needs a specific destination
2. 2 button input (further explained in development section)
3. English message will be translated to the appropriate language according to where it was sent to
4. Message will be sent and translated to the right stations
5. Message in either morse or binary will be translated back to English
6. Stations will be able to receive the message

Key principles: Usability and Human Centered Design

### Definition of usability
-The fact of something being easy to use, or the degree to which it is easy to use (Part of the users experience)
-The degree of ease with which products such as software and Web applications can be used to achieve required goals effectively and efficiently. Usability assesses the level of difficulty involved in using a user interface. Although usability can only be quantified through indirect measures and is therefore a nonfunctional requirement, it is closely related to a product's functionality.

### Definition of Human Centered Design
-An approach to interactive systems development that aims to make systems usable and useful by focusing on the users, their needs and requirements, and by applying human factors/ergonomics, usability knowledge, and techniques. This approach enhances effectiveness and efficiency, improves human well-being, user satisfaction, accessibility and sustainability; and counteracts possible adverse effects of use on human health, safety and performance


Development
----------

The most important part of the development is the translations in between English, morse and binary. People will have to understand both morse and binary communication in order to complete this project.

### Communication with binary
Binary is a number expressed in the base-2 numeral system or binary numeral system, which uses only two symbols: typically "0" (zero) and "1" (one).

Each digit is referred as a bit
* Using 1 bit, you can display 2 numbers (0-1).
* Using 2 bits, you can display 4 numbers (0-3)
* Using 3 bits, you can display 8 numbers (0-7)
* Using 4 bits, you can display 16 numbers (0-15)
etc...

**Converting a number from binary to decimal**
1. Write down the binary number
1. Multiply the LSB (Least significant bit - furthest to the right) with 2 to the power of the position number (meaning first bit = 2^1, second bit = 2^2, third bit 2^3 etc.)
1. Continue doing step 2 until reaching the MSB (most significant bit - furthest to the left)
1. Add all the numbers together to find the decimal output


**To convert a number from decimal to binary, the flowchart below must be used:**
![flowchartDecToBin](decimalToBinaryFlowchart.jpg)
Fig. 2. Flow diagram for the program that converts a number from binary to decimal representation.


#### Counting to 31, with decimal input to binary output
The code below shows how one could code from 0 to 31 with binary. The output (binary) is represented as lights either being on or off.
```.c
void setup()
{
  for (int n = 0; n <= 31; n++) {
  	//bit E
    if (n % 2 == 1) {
    	digitalWrite(bitE, HIGH);
  	}else
    {
      digitalWrite(bitE, LOW);
    }
    //bit D 
    if (n % 4 > 1) {
      digitalWrite(bitD, HIGH);
     }else
    {
      digitalWrite(bitD, LOW);
    }
     //bit C
    if (n % 8 > 3) {
      digitalWrite(bitC, HIGH);
    }else
    {
      digitalWrite(bitC, LOW);
    }
     //bit B
    if (n % 16 > 7) {
      digitalWrite(bitB, HIGH);
    }else
    {
      digitalWrite(bitB, LOW);
    }
     //bit A
    if (n % 32 > 15) {
      digitalWrite(bitA, HIGH);
    }else
    {
      digitalWrite(bitA, LOW);
    }
    delay(2000);
  }
}


void loop()
{
  
}
```
The wiring for this problem is:
![WiringBinaryCounter](wiringBinaryCounter.png)
Fig. 3. Wiring for the binary counter.


### 7-segment display
It is an electronic display device for displaying decimal numeral, and widely used in digital clocks, electronic meters, basic calculators, etc.
It consists of 8 LEDs connected in parallel that can be lit in different combinations to display the numbers

A boolean or bool is a data type in coding that has two possible values: either true or false
In a 7-segment display, it is able to show the status of a button.
The code below shows how to read the status of a button connected to port 13 in the Arduino:
```.c
bool A = digitalRead(13);
```
![7-segmentDisplay](sevenSegmentDisplay.jpg)
![7-segmentDisplay](sevenSegmentDisplayTable.jpg)
Fig. 4. Table for 7-segment display.
![7-segmentDisplay](sevenSegmentDisplayWiring.jpg)
Fig. 5. Wiring for 7-segment display.

This is the code for the 7-segment display
```.c
bool A = 0;
bool B = 0;
bool C = 0;

void loop()
{
  A = digitalRead(butA);
  B = digitalRead(butB);
  C = digitalRead(butC);
  Serial.print (A);
  Serial.print (B);
  Serial.println (C);
  // Light A
  digitalWrite(outA, B || (C && A) || (!C && !A));
  // Light B
  digitalWrite(outB, !A || (!B && !C) || (C && B));
  // Light C
  digitalWrite(outC, C || (!C && !B) || (A && B));
  // Light D
  digitalWrite(outD, (!C && !A) || (B && !C) || (B && !A) || (A && C && !B));
  // Light E
  digitalWrite(outE, (!A && !C) || (B && !C));
  // Light F
  digitalWrite(outF, (A && !B) || (A && !C) || (!C && !B));
  // Light G
  digitalWrite(outG, (!A && B) || (A && !C) || (A && !B));
}
```

### English Input system
Stations will have to communicate seamlessly with English using a two button input sytem.

The two buttons are labeled A & B:
- When A is pressed the selection changes, going through a constant rotation
- When B is pressed the character or action (SEND & DEL) is selected, tracked to the final word.

This is the flowchart of the code:
![2-buttonInput](2buttonInputflowchart.jpg)

This is the code for the English input system
```.c
//This program consists of a English input system that has letters A-Z, 0-9, and a send and select function
String text = "";
int index = 0; 
// add all the letters and digits to the keyboard
String keyboard[]={"A", "B", "C", "D", “E”, “F” “G”, “H”, “I”, “J”, “K”, “L”, “M”, “N”, “O”, “P”, “Q”, “R”, “S”, “T”, “U”, “V”, “W”, “S”, “Y”, “Z”, “0”, “1”, “2”, “3”, “4”, “5”, “6”, “7”, “8”, “9”, “ “, “SENT", "DEL"};
int numOptions = 39; 

void setup()
{
  Serial.begin(9600);
  attachInterrupt(0, changeLetter, RISING);//button A in port 2
  attachInterrupt(1, selected, RISING);//button B in port 3
}

void loop()
{
  Serial.println("Option (Select:butB, Change:butA): " + keyboard[index]);
  Serial.println("Message: "+ text);
  delay(100);
}

//This function changes the letter in the keyboard
void changeLetter(){
  index++;
//If the user reaches the end of the letters array, it goes back to the first letter
if(index>numOptions)
{
  	index=0; 
} 

//this function adds the letter to the text or send the msg
void selected()
{
if (key==”DEL”)
{
int len= text.length();
text.remove(len -1);
}

else if (key==”SEND”)
{
Serial.println(“Message sent”);
text=”“;
}

else
{
text += key;
}
index=0;
}
```


Fig. 6 Circuit used for testing the program in this document. Green button is for changing the option. Button purple is for selecting.
![EnglisbInputSystem](EnglishInputSystemWiring.jpg)



Evaluation
----------





Improvements
----------
