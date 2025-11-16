---
title: converting packed bcd encoded value to ASCII 
tags:
  - embedded_system
  - low-level
---

**packed BCD** (binary coded decimal) is a format that contains decimal values in its high and low **nibble**. in this program we are going to convert packed decimal's high and low nibble value's corresponding **ASCII**value and display it on both ports of the ATmega32 microcontroller


first we need set 2 PORTx as output to show the **ASCII** values via LED. in this program we are going to set those as ```x = { A, B }```. then we need to do the packed BCD to ASCII convertion.

## packed BCD to ASCII Convertion 

consider a packed BCD ```0x39```. we store this value in variable called x. \
x contains a high nibble and low nibble that represents 2 decimal numbers. now we need to split those decimal values to seperate variables **x** & **y**. 

the spliting of the low level is as follows, we mask the low nibble with ```0x0f``` which will only returns the low nibble value and ignore the high nibble. then we need to add 0x30 to convert it to it's ascii equalvent. we can also write 0x30 as '0' since **0x30** hexadecimal is equal to decimal 0. then we can store the **ascii** value in the x variable and pass it to porta

the high nibble splitting is similar to explained above but the only difference here is that we need to perform  left-wise bit_shifting of 4 to get to the high nibble position ```y = ((z << 4) & 0x0F) + '0'```

```c

#include <util/delay.h>
#include <avr/io.h>

int main(void) {
    DDRA = DDRB = 0xFF;
    unsigned char z = 0x39;
    unsigned char x, y;
    
    for(;;) {
    x = (z & 0x0F) + '0';
    PORTA = x;
    
    y = ((z >> 4) & 0x0F) + '0';
    PORTB = y;
    _delay_ms(500);
    }
 return 0;
}

```


here is a short version of the above code that works indentically

```c

#include <avr/io.h>
#include <util/delay.h>
#define F_CPU 16000000UL

int main(void) {
    DDRA = DDRB = 0xFF;
    for(;; PORTA = (0x39 & 0x0F)+'0', PORTB = ((0x39>>4)&0x0F)+'0', _delay_ms(500));
}

```
