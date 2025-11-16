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

#define D PORTC  //(yes they are fucking backwards)
#define C PORTD 
#define RS 0 
#define EN 2

void lcd_cmd(char c){D=c; C=(1<<EN); _delay_ms(2); C=0;}
void lcd_data(char d){D=d; C=(1<<RS)|(1<<EN); _delay_ms(2); C=0;}

int main(void){
    DDRC = DDRD = 0xFF;
    _delay_ms(20);
    lcd_cmd(0x38); lcd_cmd(0x0C); lcd_cmd(0x01); _delay_ms(2); lcd_cmd(0x06);

    char *s="Hello World";
    while(*s) lcd_data(*s++);
    while(1);
}

```
