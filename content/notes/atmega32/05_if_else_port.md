---
title: Read from a PORTx and based on condition send to their PORTx(s)
tags:
  - embedded_system
  - low-level
---

in this program we are going to use if-else condition to specify where data read from a port should be write to. we are going to set ```PORTA``` as input and read value from that port. if the value is greater than 100 send the data to ```PORTB``` else send the data to ```PORTC```


```c
#include <avr/io.h>

int main(void) {
  DDRA = 0x00;
  DDRB = DDRC = 0xFF;
  unsigned char x;
  while (1) {
    x = PINA;
    if (x > 100)
      PORTB = x;
    else
      PORTC = x;

  }
  return 0;
}
```
