---
title: converting Hexadecimal value to decimal and displaying in 3 seperata PORTx
tags:
  - embedded_system
  - low-level
---

this program converts a provided hexadecimal value to a 3 digit decimal value and store each value to a variable to be later displayed in PORTB, PORTC and PORTD of ATmega32 microcontroller. in this example; we are using hex ```0xFE```

we split the hexadecimal value to its positional valus (ones, tens and hundreds) and pass the value to the output port.splitting of hexadecimal doesnt require any complex methods.

```c

#include <avr/io.h>
#include <util/delay.h>
#define F_CPU 16000000UL

int main(void){
    DDRB = DDRC = DDRD = 0xFF;
    unsigned char x = 0xFE;

    for(;;){
        PORTB = x/100;     // 2
        PORTC = (x/10)%10; // 5
        PORTD = x%10;      // 4
        _delay_ms(1000);
    }
}

```
