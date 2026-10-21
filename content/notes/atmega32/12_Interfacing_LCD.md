---
title: Connecting and interfacing with LCD in ATmega32 
tags:
  - embedded_system
  - low-level
---

in this program, we are going to connect an external **LCD** (Liquid-crystal Display) to our ATmega32 to display characters on the lcd. for our program we are using the ```Hd44780-2``` LCD family. these type 16x2 LCD can interface in 2-modes; 4-bit mode and 8-bit mode. here we will be discussing about 8-bit mode variant.

 ### sending commands to LCD
 **1.RS: -> 0 selects command register** \
 **2.RW: -> 0 select write operation** \
 **3.En: -> make enable pin HIGH or LOW** 
 
 ### sending data to LCD
 **1.RS: -> 1 selects command register** \
 **2.RW: -> 0 select write operation** \
 **3.En: -> make enable pin HIGH or LOW** 
 
 ```c
 
#include <avr/io.h>
#include <util/delay.h>

#define RS PC0
#define EN PC2

void command(unsigned char x) {
  PORTD = x;
  PORTC &= ~(1 << RS);
  PORTC |= (1 << EN);
  _delay_ms(1);
  PORTC &= ~(1 << EN);
}

void data(unsigned char x) {
  PORTD = x;
  PORTC |= (1 << RS);
  PORTC |= (1 << EN);
  _delay_ms(1);
  PORTC &= ~(1 << EN);
  _delay_ms(100);
}

int main(void) {
  DDRD = DDRC = 0xFF;
  command(0x38);
  command(0x0C);
  command(0x01);
  command(0x06);
  command(0x80);

  unsigned char i;

  unsigned char str[15]={"EMBEDDED SYSTEM"};
  for(i=0; str[i] != '\0'; i++) {
    data(str[i]);
  }
  return 0;
}
 
 ```
 

smaller experimental version

```c
#include <avr/io.h>
#include <util/delay.h>

#define RS PC0
#define EN PC2

void w(uint8_t v, uint8_t rs) {
    PORTD = v;
    if(rs) PORTC |=  (1<<RS);
    else   PORTC &= ~(1<<RS);
    PORTC |=  (1<<EN);
    _delay_ms(1);
    PORTC &= ~(1<<EN);
    if(rs) _delay_ms(2);
}

int main(void) {
    DDRD = DDRC = 0xFF;

    w(0x38,0);
    w(0x0C,0);
    w(0x01,0);
    w(0x06,0);

    char s[]="EMBEDDED SYSTEM";
    for(uint8_t i=0; s[i]; i++)
        w(s[i],1);

    while(1);
}
```
