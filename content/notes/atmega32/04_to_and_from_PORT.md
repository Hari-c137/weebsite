---
title: Reading value from one PORTx and sending to another PORTx
tags:
  - embedded_system
  - low-level
---

in this program, we are going to read bytes of data from a PORTx and send that data to a differnt PORTx. hence we have to set one port as input and another as output.

for our program we are going to set ```PORTB``` as input and ```PORTC``` as output


```c
#include<avr/io.h>

int main(void) {

 DDRB = 0x00;
 DDRC = 0xFF;
 unsigned char x;

 while(1) {
  x = PINB;
  PORTC = x;
 }
return 0;
}
```
