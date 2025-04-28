# ESP32 Based Smart Switch
![image](https://github.com/user-attachments/assets/7e53fbcb-2700-4a48-b6a7-aa6a3e6c3a13)

# Introduction
This is a build log and guide on how to make a smart switch controlled by hidden touch switches for use with multiple lamps. This idea came about simply because we grew tired of reaching beneath a desk and under a bed to flip the lamp switches on. So, we came up with a solution to incorporate an ESP32, relay board, and two capacitive touch sensors to control each lamp independently. With the ability to activate the touch sensor through about a quarter inch in material, we hid the touch sensors so a simple tap on the corner of each desk can turn the lamps on. 

# Parts List
All of the parts used in this project can be easily found on Amazon for a low price. 

- 1x ESP32
- 2x TTP223 Capacitive Touch Switch (For two independent switches in our project)
- 1x 2 Channel 5V Relay Module
- 1x 5V 700mA mini AC-DC converter board
- 2x 6ft 3 prong extension cables

Originally we were going to use an STM32 microcontroller for this project, but with the board only having 3.3V logic and not 5V our hardware would not function.

# Code
We made our prototype using the Arduino language, then later adapted with C to the ESP32 processor. While C is not going to have significant performance increases due to the nature of the smart switch, we figured it would be a fun challenge to tailor it like an equivalent embedded system found on the market.

## Arduino Language
'''cpp
int input1 = 22;
int input2 = 23;

int output1 = 18;
int output2 = 19;

void setup() {
  // Setting input1 and input2 to use pull up resistors to stabilize floating pins
  pinMode(input1, INPUT_PULLUP);
  pinMode(input2, INPUT_PULLUP);

  pinMode(output1, OUTPUT);
  pinMode(output2, OUTPUT);

}

void loop() {
  // Setting input1 and input2 to read touch sensor signal
  int state1 = digitalRead(input1);
  int state2 = digitalRead(input2);


  if (state1 == HIGH) 
  {
    digitalWrite(output1, HIGH);
  }

  else
  {
    digitalWrite(output1, LOW);
  }

  if (state2 == HIGH)
  {
    digitalWrite(output2, HIGH);
  }

  else
  {
    digitalWrite(output2, LOW);
  }
}'''
# Enclosure
In an effort to create an all-in-one unit for reliable usage, we opted to make a single enclosure to house the hardware with Fusion 360.

## Version 1
Version 1 of our smart switch incorporates an ESP32, relay board, and two touch sensors for independent operation of two lamps. No network connection is necessary. 
![image](https://github.com/user-attachments/assets/7e53fbcb-2700-4a48-b6a7-aa6a3e6c3a13)

## Version 2
![image](https://github.com/user-attachments/assets/bde380b6-c8fd-4679-add4-41384b7a300b)
Version 2 of our smart switch incorporates an ESP32 and a single touch sensor for the operation of smart devices connected to Home Assistant using the ESPHome protocol. We chose Home Assistant and ESPHome because it is an all-in-one open-source software that can easily interact with smart devices. We also designed a smaller enclosure since a relay board was not needed for this model. As all that is going to be housed is an ESP32, a touch sensor, and a step-down transformer board to power the system.

The way this works is through ESPHome, where you can configure the behavior of the ESP32 depending on what is connected. Since we have a single touch sensor to toggle smart devices on or off, we adjusted the yaml file to include a single touch sensor as an input toggle, and the output being the alias device name as shown through Home Assistant. When the yaml file in completed, the code will be translated to C++ and uploaded via the network to our ESP32 system. 

This is currently in development and will be updated accordingly.

# Closing
