# CPAP Extension for BTT Octopus
... and some other boards  

![render image of the PCB](/images/pcb_render.jpg)


## Extension to add the CPAP function without giving up the Neopixel port.
The WS7040 “CPAP Cooling Controller” requires a positive 5V switching signal to control the speed of the motor. For some boards like the BTT Octopus, the workaround is to use the pin for Neopixel (PB0 or ​​PB10).
But if you don't want to loose out the fancy Neopixel Blinky thing, you can attach this little board to the EXP1 port. In most cases we don't need EXP1 because we have a display connected to the SBC (Raspberry Pi, etc.).


### Pin mapping
I choosed the ports for **BEEPER** and **LCD_RS**.
On the Octopus Pro V1.1 this corresponds to pins `PE8` and `PE10`.
According to the data sheet of the used STM, they can PWM, and that's exactly what we need.

![schematic of EXP1](/images/exp1_schematic.jpg)  

The beeper is additional. Some extra informations via speaker are helpful. :smile:  

Information about the pin mapping on other boards.  
Do you miss one here? Please let me know!  

| Board (Version)  | Description | Mapping new | PIN
| ---              | ---         | ---         | :---: |
| BTT Octopus Pro V1.1 | *STM32H723ZET6*|            |       |
|                  | BEEPER      | BUZZER      | `PE8`  |
|                  | LCD_RS      | CPAP Control| `PE10`  |
| BTT Octopus V1.0     | *STM32F446ZET6*|            |       |
|                  | BEEPER      | BUZZER      | `PE8`   |
|                  | LCD_RS      | CPAP Control| `PE10`  |
| Fysetc Cheetah V3.0 (EXP3)    | *STM32F446RCT6* |            |       |
|                  | BEEP        | BUZZER      | `PB10`   |
|                  | LCD_RS      | CPAP Control| `PB14`  |


### Parts
| Position | Description | Value      | Information
| ---      | ---         | ---        | ---
| R1       | Resistor    | 4.7k       | SMD 0805
| U1       | Buffer Gate | 74LVC1G125 | https://www.ti.com/lit/ds/symlink/sn74lvc1g125.pdf
| J1       | IDC Connector for EXP1 | SFH11-PBPC-D05-ST-BK | https://www.digikey.com/de/products/detail/sullins-connector-solutions/SFH11-PBPC-D05-ST-BK/1990087
| J2       | JST XH male | pitch 2.54 | https://de.aliexpress.com/item/1005003422202370.html
| optional |             |            |just if you want the buzzer
| R2       | Resistor    | 330        | SMD 0805
| R3       | Resistor    | 4.7K / 10k | SMD 0805
| Q1       | N-MOSFET    | AO4300A    | SOT-23
| BZ1      | Buzzer (passive) | 12x8.5mm  6.6mm Pitch | https://www.amazon.de/dp/B0179I6LIK 


All SMD parts are specified in the production file. I have no experience with assembling THT parts via the SMT service, so I always solder them by hand. In this case this applies to the buzzer, the JST connector and the IDC connector.

U1 buffer gate is the same level shifter as the one in the Octopus Board. The IDC Connector J1 is hard to find. Sometimes it's available at aliexpress, or you have a friendly Guy, he can order at digikey.
A 2x5 (2.54mm) pin header will also work, but please be aware to check the direction if you put it in your board.

### Place the addon into your board
Here you can see where the addon is located on the BTT Octopus Pro.  

![place in board](/images/place_in_board.jpg)

### Connection to controller board
Connect the + pin from the expansion board to the VSR pin from the controller (Pin 2).
In addition, the - pin with the GND pin (Pin 3), even if it is not absolutely necessary.

![wiring](/images/wiring.jpg)

### Klipper config
I've set the cycle time to 0.00004 (25 kHz.) This is how I got the best results and can set the power down to 6%.

```
[fan]
pin: PE10
max_power: 1.0
kick_start_time: 0.1
cycle_time: 0.00004 #25kHz
hardware_pwm: False
off_below: 0.06
```

### Additional buzzer function
Please check the configuration file in *klipper* folder.
You can copy the *macro_buzzer.cfg* into your configuration folder and include it in your *printer.cfg* with `[include macro_buzzer.cfg]`.
Enable the correct pin depending on your board.
Now you will get some tones about `M300`.
For example, you can use `M300 S440 P1000` to play a tone at 440 Hz for 1 second. In *macro_buzzer.cfg* you'll find some macros that play small melodies.

### Resistor for the buzzer
I have added a resistor in series to the buzzer with 10Ω to reduce resonances in some fequencies. If you need it louder or quieter, you can chance R4 with other values.

![R4 Resistor](/images/R4_resistor.jpg)

### FAQ
*Why not to connect to one of the many FAN ports?*  
The fan ports are controlled by a N-channel MOSFET. You just can controll the negative side with PWM but you need the positive side, because the VSR input has common GND.

*Why not to connect to the GPIO of my Raspberry Pi?*  
It's the same like at some pins of your Octopus Board. They just have a logic level of 3.3V. The VSR Input of the CPAP controller needs 5V for 100% cooling power. You lose around 30%.

*Does this also work with other boards?*  
So far I have only tested it with the Octopus, Octopus Pro and the Fysetc Cheetah 3.0. It should work with many boards that offer a slot for a 12864 display. However, you then have to check whether a pin with a PWM signal arrives at the positions I used.

--- 

### This is Open Source Hardware

![open source hardware](/images/oshw-logo-200-px.webp)  
Feel free to create your own modifiactions and versions!

Gerbers are made with [Fabrication Toolkit](https://github.com/bennymeg/JLC-Plugin-for-KiCad).
