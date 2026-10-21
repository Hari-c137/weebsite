---
title: converting ASCII to Packed BCD format
tags:
  - embedded_system
  - low-level
---
on the previous program [[06_BCDtoASCII.md]] we converted a packed **BCD** to ASCII. now we are going to do the reverse the conversion, we are going to take 2 ASCII values and combine them together to create a packed BCD

```c

#include <avr/io.h>
#include <util/delay.h>

int main(void) {
    DDRA = 0xFF;
    unsigned char x = '4';
    unsigned char y = '7';
    unsigned char z;
    
    x = x - '0'; x = x << 4;
    y = y - '0';
     z = x|y;
    
    for(;;) {
        PORTA = z;
        _delay_ms(1000);
    }
    return 0;
}

```


a shorter version for this is: \
```c
#include <avr/io.h>
#include <util/delay.h>

int main(void) {
    DDRA = 0xFF;

    for(;;) {
        PORTA = (('4'-'0') << 4) | ('7'-'0');  // compute 0x47 directly
        _delay_ms(1000);
    }
}

```
