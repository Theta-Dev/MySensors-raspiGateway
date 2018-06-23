# MySensors-raspiGateway
Raspberry Pi Gateway for your MySensors network
## Features
- MySensors Serial Gateway + Raspberry Pi in one convenient package
- Reset + Inclusion button available on the top
- Bi-color-LED to indicate RF communication
- Compact (95x83x25mm) and lightweight (100g)
## How to build
### Step 1: PCB assembly
Download the Gerber files for the PCB from this repository. Get the PCB manufactured by your favourite boardhouse (Size: 25x50mm). I chose AllPCB who sent me 12 50x100mm boards (green soldermask, HASL with lead, 1.2mm thickness) for about 15€. Note that you will save some cost if you order a larger board with multiple designs placed next to each other like I did.

Prepare the electronics components listed on the BOM. Start by soldering the male 8pin JST connector to the side of the board. Be careful not to melt the plastic with your iron. Also don't apply solder to the top part of the contact blades since that will lead to bridges. Then continue with the ATmega 328p microcontroller, the two 100nF capacitors C1/C3 and the resistor R1

Double check your solder connections, then continue with step 2
### Step 2: Programming and testing the microcontroller
To program your ATmega you will need adapter cables for ICSP and Serial. Instructions on how to solder these can be found on my website ([https://thdev.org/?Projects___misc___micro_JST](https://thdev.org/?Projects___misc___micro_JST)). You'll also need to add the barebone ATmega328p to the board definitions of your Arduino IDE.

Plug the ICSP adapter into your board and connect a 3.3V FTDI adapter to it. Then connect the FTDI adapter to your computer via USB. Select "ATmega328p (8 MHz internal clock)" from the Board menu and make sure the correct serial port is selected. Then select Tools > Burn Bootloader and wait for the process to complete. Then unplug the USB cable and the ICSP adapter and connect the Serial adapter to your board. Plug the adapter's header into your FTDI board and reconnect the USB cable.
To test your micro you can just write a little sketch that outputs something to the serial port. Upload that to your board and test it using the serial monitor.

Once you have tested your microcontroller you can continue soldering the other components.

### Step 3: PCB assembly goes on
Assemble all the other parts: begin with the small parts on the top side (resistors R3/R4/R7/R8, voltage regulator U3, capacitor C4). Flip the PCB and continue with R1/R2/R5/R6, C2 and the RFM69HW module. To complete the assembly solder the tactile switches S1/S2, and the bi-color LED D1 on the front side as well as the 5pin female header for the Raspberry Pi on the back. Thn you need to make your RF antenna. To do this, cut a piece of solid core wire to the quarter of your RF module's wavelength. The easiest way to calculate the antenna length in meters is dividing 75 by the frequency of you module in (in my case 868).
Then solder your antenna to the pin labled "ANT", and you're done! As always: check your soldering, then continue to testing.

**Addition:** I noticed that using a dipole antenna greatly increases the range and reliability of your system. To create such an antenna just cut another piece of wire to the quarter of the wavelength and solder it to one of the ground pads of the RF module.
### Step 4: Testing
Flash the Arduino sketch to your board using the Serial adapter. Open the serial monitor and make sure that the gateway starts up correctly (the [MySensors log parser](https://www.mysensors.org/build/parser) might be helpful).
### Step 5: Printing the case
Download the provided STL files for the case, place the bottom part next to the top part in your slicer and print them using a 3D printer. I've used my Prusa MK3, silver and blue PLA filament and 0.15mm layer height.

To get the cool 2-color look you can use Joe Prusas ColorPrint tool ([https://www.prusaprinters.org/color-print/](https://www.prusaprinters.org/color-print/)) and add a color change at 0.5 mm. Start the print with the colorful filament (in my case blue). The printer will print the first two layers and then ask you to change the filament. Now load the other color and continue the print.
### Step 6: Putting it all together
Put the top piece upside down on your table. Then insert the PCB (make sure buttons, LED and the mounting holes line up withe the case) and mount it using two self-tapping screws. Place your Raspberry Pi in the bottom part and screw it down as well. The antenna cables can be secured using hot glue.
Now you can put the top part onto the bottom. Make sure that the male headers of the Pi slide into the female ones of the board and close the case using 4 screws.
### Step 7: Setting up the Raspi
to be continued
## How to modify

This project is licensed under the CC-BY-SA license, so you can modify and re-publish it, as long as you'll keep it under that license and give me, the ThetaDev, credits.

I've created the PCB design using EasyEDA. The source files are located in the  _PCB_  folder. But you can also directly visit my project on EasyEDA ([https://easyeda.com/ThetaDev/MySen_raspiGatewayV2-b6f6b606683a4c1fb81e02fe21017fce](https://easyeda.com/ThetaDev/MySen_raspiGatewayV2-b6f6b606683a4c1fb81e02fe21017fce)) to edit it.

The microcontroller gets programmed using the Arduino IDE, you'll find the sketch in the  _Arduino_  folder. The required libraries are:

-   MySensors ([https://github.com/mysensors/MySensors](https://github.com/mysensors/MySensors))

The case is based on the Raspberry Pi 3 case from **0110-M-P** (https://www.thingiverse.com/thing:922740).
The case was modified using Autodesk Fusion 360. Source files are in the  _Case_  folder.
