# IoT based Home Automation Solution

## Problem Statement:
To control home devices using Blynk IoT app through Blynk Cloud Server.

## Tech Stack:

**Hardware used in PICSim Lab Emulator:**
- LDR Sensor
- LED
- CLCD hd 44780
- Arduino UNO (atmega328p)
- Ethernet Shield (ETH w5500)
- Temperature System (Heater and Cooler)
- Water Tank (Serial Remote Tank)


**Software/Tools Used:** 
- Arduino IDE
- PICSim Lab Emulator
- Null Emulator (Setup)
- Blynk Application (both Mobile and Web version)
 Active internet connection is also required.

**Languages Used:**
- Embedded C/C++
  
## Project Requirements:

- Garden Light Control:- It is based on LDR sensor. Here during day time lights must be OFF and during night time lights must be ON.
- Temperature System Control:- By using BLYNK Application. Here, we can turn on and off the heater and cooler using BLYNK app. We also print the notification on the mobile application on the dashboard. Here, if the heater is on and temperature raises above 35 degree Celsius, then notification is sent to the mobile phone and heater is turned off automatically.
- Water Tank Control :-Here the tank inlet and outlet valve is controlled using button widget. Suppose the volume of the water tank is less than 2000L, then a notification is sent on the app that water level is less than 2000 and then inlet valve will be on automatically . After this , once volume becomes 3000 that is full, inlet valve will turn off automatically and a notification that water level is full and inlet is disabled is displayed on the app.

## Breif Description:
- Garden Lights Control: 
    - It is based on LDR sensor.LDR is used to indicate the presence or absence of light, or to measure the light intensity. 

    - Goal:- Here during day lights must be off and during night lights must be on.
    
    - Working:- During night Resistance will be more , so the LED will turn ON. Similarly, during day time  resistance will be less, so the LED will turn OFF. So as it becomes darker and darker , brightness of the led increases.And more and more brighter the sunlight, brightness of led decreases.

-  Temperature Control:
    - Here we read temperature from temperature system and display it on the CLCD and also on mobile app. We can turn on and off the heater and cooler using app.
    
    - A notification is printed on the mobile application on the dashboard if the heater is on and if temperature raises above 35 degree c , the notification ‘Temperature is above 35 degree Celsius . Turning OFF the Heater’ is sent to the mobile phone and the heater is turned off automatically.
    
     - When Heater is in ON condition -> HT_R ON will be displayed on CLCD.
   
    - When Heater is in OFF condition -> HT_R OFF will be displayed on CLCD.
    
   -  When Cooler is in ON condition -> CO_LR OFF will be displayed on CLCD.
    
    - When Cooler is in OFF condition -> CO_LR OFF will be displayed on CLCD.
    - Formula used for converting digital values from adc to temperature degree celsius
    - temperature = (((analogRead(A0) *(float)5/ 1023)) /(float) 0.01)


- Water Tank Control: 
    - Here we read the volume of the tank and display it on the CLCD and at the same time also on the mobile application.
    - Here we control the tank inlet and outlet valve using button widget. Message is displayed on CLCD whether the inlet or outlet is on or off.
    - When Inlet Valve is in ON condition -> IN_FL_ON will be displayed on CLCD.
    - When Inlet Valve  is in OFF condition -> IN_FL_OFF will be displayed on CLCD.
   - When Outlet Valve is in ON condition -> OT_FL_OFF will be displayed on CLCD.
   - When Outlet Valve is in OFF condition -> OT_FL_OFF will be displayed on CLCD.
    - Suppose the volume of the water tank is less than 2000L, then a notification ‘Water Level is less than 2000L. Water Inflow Enabled’ will be printed on the app dashboard and then inlet valve will be switch on automatically. then once volume becomes 3000 that is full, ‘Water Level is FULL. Water Inflow Disabled’ will be printed on the dashboard and inlet valve will be turned off automatically. 

## Working of code:
- home_automation_blynk_controlled.ino file:

1. **Setup and Definitions:**
   - Libraries are included, and Blynk Template ID, Device Name, and Auth Token are defined.
   
2. **Initial Setup:**
   - Blynk, LCD, temperature system, LDR, and serial tank are set up.
   - Timer is set to update temperature reading every second.

3. **Looping:**
   - Blynk and timer tasks are executed in the loop.

4. **Displaying Temperature and Volume:**
   - Temperature and tank volume are read and shown on the LCD.

5. **Brightness Control:** (Specifics not shown)
   - Garden lights are controlled based on light intensity.

6. **Temperature Handling:**
   - If temperature is above 35°C, heater is turned off.
   - Notifications are sent to Blynk app.

7. **Tank Volume Handling:**
   - Inlet valve is turned on if volume is low and inlet valve is off.
   - Inlet valve is turned off when volume is high and inlet valve is on.
   - Notifications and Blynk updates are sent.

8. **Temperature and Volume Update:**
   - Blynk app is updated with current temperature and tank volume.

- ldr.cpp file:
1. **Initializing LDR Module:**
   - The code includes necessary headers for LDR module and Arduino.
   - The `init_ldr` function is defined to initialize the LDR module.
   - The digital pin connected to the garden light is set as an output.

2. **Brightness Control:**
   - The `brightness_control` function is defined for controlling the garden light brightness based on LDR sensor readings.
   - An unsigned short variable `inputVal` is declared to hold sensor readings.
   - The code reads the analog value from the LDR sensor using `analogRead`.
   - The analog value is scaled down from a range of 0-1023 to 0-255.
   - The PWM value is set inversely proportional to the scaled analog value (255 - inputVal).
   - The analog output to control the garden light (PWM) is set using `analogWrite`.
   - A delay of 100 milliseconds is added.

- ldr.h file:

1. **Header Guard:**
   - `#ifndef LDR_H` and `#define LDR_H` create a header guard to prevent multiple inclusion of this header file in the same compilation unit.

2. **Constants and Pins:**
   - `LDR_SENSOR` is defined as analog pin A1, which is used to connect the LDR sensor.
   - `GARDEN_LIGHT` is defined as digital pin 3, which controls the garden light.

3. **Function Prototypes:**
   - `void init_ldr(void);` is a function prototype for initializing the LDR module. This function is expected to be implemented in the corresponding `.cpp` file.
   - `void brightness_control(void);` is a function prototype for controlling the brightness of the garden light based on LDR sensor readings. This function is also expected to be implemented in the corresponding `.cpp` file.

4. **Header Guard Closure:**
   - `#endif` closes the header guard, ensuring that the content within this header file is only included once during compilation.
     
- main.h file: 
 
1. **Header Guard:**
   - `#ifndef MAIN_H` and `#define MAIN_H` create a header guard to prevent multiple inclusion of this header file in the same compilation unit.

2. **Constants:**
   - `ON` is defined as the value 1, typically representing an "ON" state or condition.
   - `OFF` is defined as the value 0, typically representing an "OFF" state or condition.

3. **Virtual Pin Assignments:**
   - `TEMPERATURE_GAUGE`, `COOLER_V_PIN`, `HEATER_V_PIN`, `WATER_VOL_GAUGE`, `INLET_V_PIN`, `OUTLET_V_PIN`, and `BLYNK_TERMINAL_V_PIN` are defined with their respective virtual pin assignments. These assignments are used for interacting with the Blynk app and controlling various aspects of the home automation system.

4. **Header Guard Closure:**
   - `#endif` closes the header guard, ensuring that the content within this header file is only included once during compilation.

- serial_tank.cpp file:

1. **Include Statements:**
   - The code includes necessary headers for `serial_tank` and Arduino, as well as the `main.h` header we discussed earlier.

2. **Global Variables:**
   - `volume_value` is an unsigned integer variable that will store the tank volume value.
   - `valueh` and `valuel` are unsigned char variables used to store high and low bytes of received data.

3. **Initializing Serial Communication:**
   - The `init_serial_tank` function initializes serial communication with a baud rate of 19200.
   - Three synchronization bytes (0xFF) are sent to establish communication.

4. **Tank Volume Reading:**
   - The `volume` function is used to read the tank's volume.
   - `VOLUME` is sent to request volume data from the tank.
   - The function waits until serial data is available.
   - High and low bytes of data are read, combined, and stored in `volume_value`.

5. **Inlet Valve Control:**
   - `enable_inlet` and `disable_inlet` functions control the tank's inlet valve.
   - They send `INLET_VALVE` followed by `ENABLE` or `DISABLE` to open or close the inlet valve.

6. **Outlet Valve Control:**
   - `enable_outlet` and `disable_outlet` functions control the tank's outlet valve.
   - They send `OUTLET_VALVE` followed by `ENABLE` or `DISABLE` to open or close the outlet valve.
   
- serial_tank.h file:
1. **Header Guard:**
   - `#ifndef SERIAL_TANK_H` and `#define SERIAL_TANK_H` create a header guard to prevent multiple inclusion of this header file in the same compilation unit.

2. **Constants for Valve Control:**
   - `INLET_VALVE` is defined as `0x00`, representing the digital control for the inlet valve.
   - `OUTLET_VALVE` is defined as `0x01`, representing the digital control for the outlet valve.

3. **Constants for Sensor Digital Readings:**
   - `HIGH_FLOAT` is defined as `0x10`, representing the digital reading from the high float sensor.
   - `LOW_FLOAT` is defined as `0x11`, representing the digital reading from the low float sensor.

4. **Constant for Analog Sensor Reading:**
   - `VOLUME` is defined as `0x30`, representing the analog reading for the tank volume.

5. **Constants for Valve Control:**
   - `ENABLE` is defined as `0x01`, representing the enable command.
   - `DISABLE` is defined as `0x00`, representing the disable command.

6. **Function Prototypes:**
   - `void init_serial_tank(void);` is a function prototype to initialize the serial communication with the tank system.
   - `void enable_inlet(void);` is a function prototype to enable the tank's inlet valve.
   - `void enable_outlet(void);` is a function prototype to enable the tank's outlet valve.
   - `void disable_inlet(void);` is a function prototype to disable the tank's inlet valve.
   - `void disable_outlet(void);` is a function prototype to disable the tank's outlet valve.
   - `unsigned int volume(void);` is a function prototype to read the tank's volume.

7. **Header Guard Closure:**
   - `#endif` closes the header guard, ensuring that the content within this header file is only included once during compilation.

- temperature_system.cpp file:

1. **Include Statements:**
   - The code includes necessary headers for `temperature_system` and Arduino, as well as the `main.h` header we discussed earlier.

2. **Initializing Temperature System:**
   - The `init_temperature_system` function is defined to initialize the temperature control system.
   - Pins for the cooler and heater are set as outputs.
   - Initial states of the cooler and heater are set to LOW (off).

3. **Reading Temperature:**
   - The `read_temperature` function is defined to read the temperature using an analog sensor.
   - The analog value from A0 pin is converted to temperature in degrees Celsius.
   - The formula used is based on the characteristics of the temperature sensor.

4. **Cooler Control:**
   - The `cooler_control` function is defined to control the cooler based on a boolean parameter.
   - It sets the cooler pin to the specified control state (HIGH for on, LOW for off).

5. **Heater Control:**
   - The `heater_control` function is defined to control the heater based on a boolean parameter.
   - It sets the heater pin to the specified control state (HIGH for on, LOW for off).

- temperature_system.h file:

1. **Header Guard:**
   - `#ifndef TEMPERATURE_SYSTEM_H` and `#define TEMPERATURE_SYSTEM_H` create a header guard to prevent multiple inclusion of this header file in the same compilation unit.

2. **Constants for Heater and Cooler Control:**
   - `HEATER` is defined as the digital pin number for the heater.
   - `COOLER` is defined as the digital pin number for the cooler.

3. **Constant for Temperature Sensor:**
   - `TEMPERATURE_SENSOR` is defined as the analog pin number for the temperature sensor.

4. **Function Prototypes:**
   - `float read_temperature(void);` is a function prototype to read and return the temperature value.
   - `void init_temperature_system(void);` is a function prototype to initialize the temperature control system.
   - `void cooler_control(bool control);` is a function prototype to control the cooler.
   - `void heater_control(bool control);` is a function prototype to control the heater.

5. **Header Guard Closure:**
   - `#endif` closes the header guard, ensuring that the content within this header file is only included once during compilation.

## Steps to setup Blynk IoT App:

- Create BLYNK Account in Web Version.
- Create template as ‘Home Automation’.
- Add new device.
- We must remember to copy Template ID, Device Name and Authentication Token. (Important step)
  ![App Screenshot](./imgs/blynk1.png)
- Add Datastreams
   ![App Screenshot](./imgs/blynk2.png)
- Setup the widgets required in Mobile Phone and hence we are ready to control the device.
   ![App Screenshot](./imgs/blynk3.png)


## Steps in Arduino IDE:
- Install the required Library files like SPI.h, Ethernet.h, BlynkSimpleEthernet.h .
- Coding.
- Save the file.
- Compile
- Export the compiled Binary.
- Load the HEX file in PICSimlab Simulator.
   ![App Screenshot](./imgs/ard1.png)
## Complete Demonstration available at: 
- YouTube video: https://www.youtube.com/watch?v=JTmT7ICp2aE&ab_channel=SourabhShenoy

## Screenshot of Output:
 ![App Screenshot](./imgs/aed2.png)
![App Screenshot](./imgs/finalop.png)

