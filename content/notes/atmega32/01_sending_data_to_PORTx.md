---
title: Sending data to ATmega32 PORTx
tags:
  - embedded_system
  - low-level
---
 
### **Creating a AVR program to send bits of data to a specified PORTx** 

this is program, we are going to send decimal value 1 (0x01 in hexadecimal) from 0xFF to PORTB of the ATmega32

we can use set the **Data Direction Register** or ```DDRx``` where *x = {A,B,C,D}*  to set which port to be output; in our case it's PORTB. to set as output we set DDRx as ```0xFF``` and ```0x00``` for input

then we store the value 0x01 in a variable with ```uint8_t``` datatype for passing it the ```PORTB``` then we increment the variable until 0xFF.

```c
#include <util/delay.h>
#include <avr/io.h> /*  header file for avr i/o opertions */
#define F_CPU 16000000UL

int main(void) {
    DDRB = 0xFF; 
    uint8_t x = 0x01;
    
    for(;;) { /* loop for x++ increments */
        PORTB = x;
        _delay_ms(300);
        x++;
        if(x == 0x00)
            x = 0x01; /* restarting after 0xFF rolls over to 0x00*/
    }
}
```


## Compiling 

```sh
avr-gcc -mmcu=atmega32 -DF_CPU=16000000UL -Os -o main.elf main.c
avr-objcopy -O ihex -R .eeprom main.elf main.hex
```
