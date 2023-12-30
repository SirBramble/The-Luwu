# The Luwu

ESP32 Board for RGB LED integration in to Home assistant

## Motivation

My 3D Printer has a standard RGB LED Strip installed in its enclosure. This Board allows me to control it with Home Assistant. There are easier and cheaper ways of doing this, but this Board also doubled as a test board for implementing Ideal Diodes and MOSFET drivers.

## Features

The Board uses a ESP32C3 to interface with Home Assistant. The ESP controls 3 RGB channels that are each capable of (in theory) driving 10A. These channels can also be mapped to control addressable RGB Strips, although this can only be done for all channels at once. There is also one dedicated addressable RGB Strip Port, that can be used at any time.

To accomplish these features, it needs a 5V supply to drive the logic and optionally the addressable LED Strips and a 12V supply to drive the non addressable Strip. To accomplish this in a 3D Printer environment a Voltage converter for the 24V Power Supply is needed. For this [the Supluwu](https://github.com/SirBramble/The-Supluwu) could be used.

## Integration in to Home Assistant

TBD

## Pictures

![ ](https://github.com/SirBramble/The-Luwu/blob/main/The%20Luwu%20PCB/Pictures/The%20Luwu%20PCB%20TOP.png?raw=true)

![ ](https://github.com/SirBramble/The-Luwu/blob/main/The%20Luwu%20PCB/Pictures/The%20Luwu%20PCB%20SIDE.png?raw=true)

![ ](https://github.com/SirBramble/The-Luwu/blob/main/The%20Luwu%20PCB/Pictures/The%20Luwu%20PCB%20CLOSE.png?raw=true)
