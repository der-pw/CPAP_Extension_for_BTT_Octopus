# !!! PROJECT IS STILL WIP !!!


# CPAP Extension for BTT Octopus
... and some others  

![render image of the PCB](/images/pcb_render.jpg)

## Extension to add the CPAP function without giving up the Neopixel port.
The WS7040 "CPAP cooling controller" requires a positive 5V switching signal to control the speed of the motor. The workaround on some boards like the BTT Octopus is to use the pin for Neopixel (PB0 or ​​PB10).
But if you don't want to do without the fancy Neopixel blinky thing, you can add this little board on the EXT1 port. In most cases we don't need EXP1 because we have a display connected to the SBC (Raspberry Pi etc.).

### Pin mapping
I choose the ports for ```BEEPER``` and ```LCD_RS```.
On the Octopus Pro V1.1 this corresponds to pins PE8 and PE10.
According to the data sheet of the used STM, they can PWM, and that's exactly what we need.

The beeper is additional. Some extra informations via speaker are helpful. :smile:  

![schemativ of EXP1](/images/exp1_schematic.jpg)

Information about the pin mapping on other boards.  
Do you miss one here? Please let me know!  

| Board (Version)  | Description | Mapping new | PIN
| ---              | ---         | ---         | :---: |
| BTT Octopus Pro V1.1 | STM32H723ZET6|            |       |
|                  | BEEPER      | BUZZER      | PE8   |
|                  | LCD_RS      | CPAP Control| PE10  |
| BTT Octopus V1.0     | STM32F446ZET6|            |       |
|                  | BEEPER      | BUZZER      | PE8   |
|                  | LCD_RS      | CPAP Control| PE10  |
| Fystec Cheetah V3.0     | STM32F446RCT6 |            |       |
|                  | BEEP        | BUZZER      | PB10   |
|                  | LCD_RS      | CPAP Control| PB12  |


## Parts
| Position | Description | Value      | Information
| ---      | ---         | ---        | ---
| R1       | Resistor    | 4.7k       | SMD 0805
| U1       | Buffer Gate | 74LVC1G125 | https://www.ti.com/lit/ds/symlink/sn74lvc1g125.pdf
| J1       | Connector for EXP1 | SFH11-PBPC-D05-ST-BK | https://www.digikey.com/de/products/detail/sullins-connector-solutions/SFH11-PBPC-D05-ST-BK/1990087
| J2       | JST XH male | pitch 2.54
| optional |             |            |just if you want the buzzer
| R2       | Resistor    | 330        | SMD 0805
| R3       | Resistor    | 4.7K / 10k | SMD 0805
| Q1       | N-MOSFET    | 
| BZ1      | Buzzer (passive) | 12x8.5mm |


U1 buffer gate is the same level shifter as the one in the Octopus Board. The IDC Connector J1 is hard to find. Sometimes it's available at aliexpress, or you have a friendly Guy, he can order at digikey.

### Place the addon into your board
![animation](/images/animation.gif)
