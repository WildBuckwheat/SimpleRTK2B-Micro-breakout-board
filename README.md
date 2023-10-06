# SimpleRTK2B-Micro-breakout-board

A small break out board for the ArduSimple SimpleRTK2B Micro. The PCB is intended for [AgOpenGPS](https://github.com/farmerbriantee/AgOpenGPS "github.com/farmerbriantee/AgOpenGPS") users that wish to use a [Micro F9P](https://www.ardusimple.com/product/simplertk2b-micro/ "Ardusimple") as a  base station, rover, or just want a way to update F9P software.

The PCB features a USB 2.0 Micro B connection, 3.3v voltage regulator, and IO breakout points.

New for V2.0 is an RTK status LED and a small pinout change that allows for connection to an HC05 bluetooth module.

<img src="https://github.com/WildBuckwheat/SimpleRTK2B-Micro-breakout-board/blob/main/Images/Batch.jpg" width="820"> 

<img src="https://github.com/WildBuckwheat/SimpleRTK2B-Micro-breakout-board/blob/main/Images/FrontBack.jpg" width="820"> 

<img src="https://github.com/WildBuckwheat/SimpleRTK2B-Micro-breakout-board/blob/main/Images/RenderFront.jpg" width="820"> 

<img src="https://github.com/WildBuckwheat/SimpleRTK2B-Micro-breakout-board/blob/main/Images/RenderBack.jpg" width="820"> 



# Features
1. A USB Micro B port and 3.3v regulator. The USB port can power the Micro F9P and also allows for USB connection to the Micro F9P. Plug the USB cable into your PC, Raspberry Pi, Orange Pi, etc. Use the USB connection for U-Center setup, base station communication, AgOpenGPS, etc.
2. Power status LED (green). If the Micro F9P has 3.3v power then the LED is lit.
3. RTK status LED (red). LED on = no corrections, LED blink = float, LED off = RTK.
4. Breakout connections (sides) for all the Micro F9P's IO points. 
5. Bottom breakout connection with serial 1 conenctions and power. A great connection point if you want to use a wired UART connection or bluetooth HC05 module.
6. Mounting holes. 1"x1.25" pattern, M3 or 4-40 holes.



# Power
You can power the board in 1 of these 3 ways:
1. Through the USB plug.
2. Through the VUSB connection on a breakout point. Connect 5v.
3. Through the 3V3 connection on a breakout point. Connect 3.3v. This will bypass the SMD voltage regulator.

Warning: Do not attempt to power the board in more than 1 way. If you do, you may damage your USB port or damage the SMD voltage regulator.



# Ordering using JLCPCB's SMT Service
You'll need the Gerber file, the BOM file, and the Pick and Place (CPL) file. Set "Remove Order Number" to "Specify a Location," all the other JLCPCB default options are ok. 1 oz fill is ok. Snazz it up and change the board colour if you like. Use the SMT assembly service for the Top Side and the board will arrive assembled.



# For the Tinkerers
The EasyEDA files are provided. The design is licensed under CERN-OHL-S-2.0, any improvements or modifications should be made available under the same licence. If you see any isues or have any suggestions please create a ticket on GitHub.



# Use case examples
There are many ways that you can use this PCB, here are a few.

## Use example 1 - RTK base station
No soldering required. Plug a Micro F9P into the headers and connect a micro USB cord to your Raspberry Pi, Orange Pi, Windows machine, etc. Use RTKBase, SNIP, etc. The PCB's voltage regulator will regulate USB power to the required 3.3v and and allow USB communication to the host. Note that the red RTK LED will remain on, since a base station never has an RTK fix.

<img src="https://github.com/WildBuckwheat/SimpleRTK2B-Micro-breakout-board/blob/main/Images/Powered.jpg" width="820"> 

## Use example 2 - update F9P firmware/configuration
Same as above, just plug the Micro F9P into the headers and connect a micro USB cord. Open U-Center in Windows and update your configuration or firmware. Perfect if the USB connections have broken off of your All In One board.

## Use example 3 - Development
All of the Micro F9P IO points are broken out to pads on standard 0.1" spacing. You can solder male headers to the bottom of the PCB and stick it on a breadboard. Or solder female headers to the PCB and use jumper wires. Or solder wires straight to the pads.

## Use example 4 - Rover
Open the USB COM port in AgIO and use the Micro F9P with AgOpenGPS. Use it for mapping, lightbar, or autosteer.

## Use example 6 - Bluetooth
The bottom breakout point matches that of an inexpensive HC05 bluetooth module. Connect the bluetooth to your tablet, laptop, or phone (iPhone does not support HC05). With a USB power bank you can have a totally wireless solution, a 5000mah USB powerbank provides close to 24 hours of power. This makes a great temporary setup for mapping with an ATV, lightbar use, implement steer, etc.

<img src="https://github.com/WildBuckwheat/SimpleRTK2B-Micro-breakout-board/blob/main/Images/WithBluetoothSoldered.jpg" width="820"> 



### Recommended Setup for F9P with HC05 Bluetooth
There are different ways to setup the F9P and HC05 bluetooth module. Here is how I setup mine. I recommend that you approach this by setting up the HC05 before soldering, with the intention of only doing it once and never changing it again.

First connect the HC05 to a windows COM port. I am using a dedicated FTDI adapter. You could also use an arduino with the ATmega removed, Teensy as a serial pass through, etc. If you have already soldered the HC05 then remove the Micro F9P and make your FTDI connections using the VUSB, TX1, RX1, and GND breakout points. Remember to connect TX to RX and RX to TX.

<img src="https://github.com/WildBuckwheat/SimpleRTK2B-Micro-breakout-board/blob/main/Images/BluetoothSetup/FTDI_HC05.jpg" width="820"> 

Hold down the HC05 button and apply power to enter programming mode, the HC05 LED should blink slowly to indicate that it is in programming mode. Open the Arduino serial monitor and select "Both NL & CR" and "38400 baud." If you type and send "AT" (without quotations), the HC05 should return "OK." Do not forget the "Both NL & CR" step.

All the below without quotations

   
- Send ```bash AT+NAME=F9P ``` to set the name to F9P. Send "AT+NAME?" to have the name echo'd back to you.
- Send "AT+UART=115200,1,0" to set the serial baud rate to 115,200 bps. Send "AT+UART?" to have the new baud rate echo'd back to you. This baud rate is only for the serial when not in programming mode. Programing mode is always 38,400 bps and cannot be changed.
- Send "AT+PSWD=1234" to set the bluetooth password. Send "AT+PSWD?" to check the bluetooth password. Some HC05 versions won't let you change the password without further permissions, just check the password and use the default which is always 1234 or 0000.

<img src="https://github.com/WildBuckwheat/SimpleRTK2B-Micro-breakout-board/blob/main/Images/BluetoothSetup/AT_Commands.jpg" width="820"> 


Connect the HC05 to the PCB. Only the center 4 connections are required. You can solder the HC05 State pin to the PCB NC pin if you want to. If you solder the HC05 EN pin to the PCB 3V3 pin then no damage will occur, but the HC05 will be stuck in programming mode.

Plug in the Micro F9P and connect it with a Micro USB cord to Windows. Follow the steps here to configure the F9P with the latest SingleAntennaRover config.
https://github.com/farmerbriantee/AgOpenGPS_Boards/wiki/configuring-the-zed-f9p

Once this has been completed, open the configuration view and navigate to PRT (Ports). Change the baudrate of UART1 to 115200. Press the send button. You can save this new configuration if you wish. All we just did is grab the latest AgOpenGPS config and change the serial 1 baud rate to the maximum baud rate supported by Windows bluetooth.

<img src="https://github.com/WildBuckwheat/SimpleRTK2B-Micro-breakout-board/blob/main/Images/BluetoothSetup/UCenter_bluetooth.jpg" width="820"> 