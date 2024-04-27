# The Luwu

ESP32 Board for RGB LED integration into Home Assistant
![ ](https://github.com/SirBramble/The-Luwu/blob/595da0b069d216a6ebaaa53a5030eae053b21326/The%20Luwu%20PCB/pictures/The%20Luwu%20PCB%20TOP.png?raw=true)
## Motivation

My 3D Printer has a standard RGB LED Strip installed in its enclosure. This Board allows me to control it with Home Assistant. There are easier and cheaper ways of doing this, but this Board also doubled as a test board for implementing Ideal Diodes and MOSFET drivers.

## Features

The Board uses a ESP32C3 to interface with Home Assistant. The ESP controls 3 RGB channels that are each capable of (in theory) driving 10A. These channels can also be mapped to control addressable RGB Strips, although this can only be done for all channels at once and **ONLY** if the level shifter logic voltage is set to 5V. There is also one dedicated addressable RGB Strip Port, that can be used at any time.

To accomplish these features, it needs a 5V supply to drive the logic and optionally the addressable LED Strips and a 12V supply to drive the non addressable Strip. To accomplish this in a 3D Printer environment a Voltage converter for the 24V Power Supply is needed. For this [the Supluwu](https://github.com/SirBramble/The-Supluwu) could be used.

## Integration into Home Assistant

ESP Home Config:

```yaml
esphome:
  name: NAME
  friendly_name: FRIENDLY_NAME

esp32:
  board: esp32-c3-devkitm-1
  framework:
    type: arduino

# Enable logging
logger:
  level: DEBUG
# Enable Home Assistant API
api:
  encryption:
    key: ""     #let this auto generate or set it yourself

ota:
  password: ""  #let this auto generate or set it yourself

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "FALLBACK_HOTSPOT_NAME"
    password: "SUPER_SECURE_PASSWORD123"

light:
  - platform: rgb
    id: oldstyle_rgb
    name: "Oldstyle RGB"
    red: gpio_red
    green: gpio_green
    blue: gpio_blue
    effects:
      - lambda:
          name: Rainbow
          update_interval: 5s
          lambda: |-
            static int state = 0;
            auto call = id(oldstyle_rgb).turn_on();
            call.set_transition_length(4000);
            if (state == 0) {
              call.set_rgb(1.0, 0.0, 0.0);
            } else if (state == 1) {
              call.set_rgb(1.0, 0.5, 0.0);
            } else if (state == 2) {
              call.set_rgb(1.0, 1.0, 0.0);
            } else if (state == 3) {
              call.set_rgb(0.5, 1.0, 0.0);
            } else if (state == 4) {
              call.set_rgb(0.0, 1.0, 0.0);
            } else if (state == 5) {
              call.set_rgb(0.0, 1.0, 0.5);
            } else if (state == 6) {
              call.set_rgb(0.0, 1.0, 1.0);
            } else if (state == 7) {
              call.set_rgb(0.0, 0.5, 1.0);
            } else if (state == 8) {
              call.set_rgb(0.0, 0.0, 1.0);
            } else if (state == 9) {
              call.set_rgb(0.5, 0.0, 1.0);
            } else if (state == 10) {
              call.set_rgb(1.0, 0.0, 1.0);
            } else if (state == 11) {
              call.set_rgb(1.0, 0.0, 0.5);
            }
            call.perform();
            state++;
            if (state == 12)
              state = 0;
      - lambda:
            name: Fast Rainbow
            update_interval: 1s
            lambda: |-
              static int state = 0;
              auto call = id(oldstyle_rgb).turn_on();
              call.set_transition_length(1000);
              if (state == 0) {
                call.set_rgb(1.0, 0.0, 0.0);
              } else if (state == 1) {
                call.set_rgb(1.0, 0.5, 0.0);
              } else if (state == 2) {
                call.set_rgb(1.0, 1.0, 0.0);
              } else if (state == 3) {
                call.set_rgb(0.5, 1.0, 0.0);
              } else if (state == 4) {
                call.set_rgb(0.0, 1.0, 0.0);
              } else if (state == 5) {
                call.set_rgb(0.0, 1.0, 0.5);
              } else if (state == 6) {
                call.set_rgb(0.0, 1.0, 1.0);
              } else if (state == 7) {
                call.set_rgb(0.0, 0.5, 1.0);
              } else if (state == 8) {
                call.set_rgb(0.0, 0.0, 1.0);
              } else if (state == 9) {
                call.set_rgb(0.5, 0.0, 1.0);
              } else if (state == 10) {
                call.set_rgb(1.0, 0.0, 1.0);
              } else if (state == 11) {
                call.set_rgb(1.0, 0.0, 0.5);
              }
              call.perform();
              state++;
              if (state == 12)
                state = 0;


# Example output entry (default)
output:
  - platform: ledc
    id: gpio_red
    pin: GPIO6
  - platform: ledc
    id: gpio_green
    pin: GPIO7
  - platform: ledc
    id: gpio_blue
    pin: GPIO10

#uncomment to enable
# Example output entry (modified)
#output:
#  - platform: ledc
#    id: gpio_red
#    pin: GPIO7
#  - platform: ledc
#    id: gpio_green
#    pin: GPIO10
#  - platform: ledc
#    id: gpio_blue
#    pin: GPIO6

captive_portal:
    
```

### Flashing firmware

I had some issues when flashing the Firmware.

This helped: [https://github.com/espressif/esp-idf/issues/9005](https://github.com/espressif/esp-idf/issues/9005)

## Schematic

The Schematic can be found here: [The Luwu PCB/print/The Luwu PCB.pdf](https://github.com/SirBramble/The-Luwu/blob/f4439437ab35bf7dc6665ce1337c5cff792b409b/The%20Luwu%20PCB/print/The%20Luwu%20PCB.pdf)

## Pictures

![ ](https://github.com/SirBramble/The-Luwu/blob/595da0b069d216a6ebaaa53a5030eae053b21326/The%20Luwu%20PCB/pictures/The%20Luwu%20PCB%20TOP.png?raw=true)

![ ](https://github.com/SirBramble/The-Luwu/blob/595da0b069d216a6ebaaa53a5030eae053b21326/The%20Luwu%20PCB/pictures/The%20Luwu%20PCB%20SIDE.png?raw=true)

![ ](https://github.com/SirBramble/The-Luwu/blob/595da0b069d216a6ebaaa53a5030eae053b21326/The%20Luwu%20PCB/pictures/The%20Luwu%20PCB%20CLOSE.png?raw=true)
