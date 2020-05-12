# Switch-Fightstick Fork - Use your New 3DS as a Switch Controller

## Hardware Requirements
- see [shinyquagsire23/Switch-Fightstick's README](https://github.com/shinyquagsire23/Switch-Fightstick/blob/master/README.md) for microcontroller requirements (I used an Arduino R3, but you could also use a Teensy, Arduino Micro or possibly an Arduino Leonardo).
- NodeMCU
- USB micro-b cable
- 4 male-to-female jumper cables
- A hacked New 3/2DS. You can also use an old Old 3/2DS, however it won't be as enjoyable experience, since you will lack the extra controls.

## Usage
This uses the Arduino/teensy/ATMega board to emulate a USB "controller" that the Switch is able to recognize and record key presses from. The NodeMCU is used to receive UDP packets from the 3DS, interpret them, and send over commands to the Arduino that will in turn be sent to the Switch as key presses, circle pad movements etc.

## Setup
1. Download this repository.
2. Download [CTurt's 3DSController repository](https://github.com/CTurt/3DSController) and build it, then place the 3DS software on your 3DS. The releases are outdated and do not include New 3DS support, therefore you must build it. If you have trouble building it take a look at the forks.
3. After downloading necessary components, build this repository using ```make```. Then follow the readme linked in the requirements section to flash ```Joystick.hex``` to your microcontroller.
4. Next, to flash the NodeMCU, you will need to set up the Arduino environment if you already haven't. There are plenty online tutorials on how to do this, but you can start with [this one](https://www.instructables.com/id/Steps-to-Setup-Arduino-IDE-for-NODEMCU-ESP8266-WiF/).
5. Open ```nodemcu_3ds_udp_packet_receiver.ino``` in the Arduino IDE. Next, on line 94 where it says ```WiFi.begin("SSID", "PASSWORD");``` enter your WiFi credentials. The 3DS and NodeMCU need to be connected to the same WiFi network. You will also want to change the static IP address of your NodeMCU and the gateway of your router on lines 13 and 14. I'd recommend first uploading a simple sketch to the NodeMCU that prints out its IP address and use that. Modify your gateway IP address as well. Once that's all done, upload the .ino to the NodeMCU
6. Edit the ```3DSController.ini``` on your 3DS to enter the IP address of the NodeMCU.
7. Finally, connect the NodeMCU's RX pin to the microcontroller's RX pin (on the Arduino Uno R3 this is D0); TX pin to the microcontroller's TX pin (one the Arduino Uno R3 this is D1); the VIn pin to the other microcontroller's 5-volt output (although it can be higher); and ground pin to ground pin. Plug into the Switch Dock with the Switch inside, boot the software on your 3DS and it should be working

## Caveats
This software is, at best, imperfect. Occasionally, key presses/releases won't be registered, or the circle pad or c-stick won't return to their neutral value when you let go. This software is more of a proof-of-concept than a final product.
