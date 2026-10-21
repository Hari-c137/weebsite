---
title: transferring data serially through specific pins of PORTx
tags:
  - embedded_system
  - low-level
---

in this program, we are going to send data serially meaning one bit at a time and transferring data parallelly. to acheive this we have to send each bit of the data in the extact order aswell as without any change in the orginal data. on this program we are only using a single pin of port to visualize the transferred data, hence we can enable only a particular pin we require and turn-off all other pins of the port.

it is also useful to add a timeout between each data transfer using ```_delay_ms()``` function in C

example using data ```55(hex)```

```c

#include <avr/io.h>
#include <util/delay.h>

int main(void) {
    DDRC |= (1 << 3);
    unsigned char data = 0x55;

    for (uint8_t i = 0; i < 8; i++) {
        if (data & 0x01)
            PORTC |= (1 << 3);
        else
            PORTC &= ~(1 << 3);
        _delay_ms(1000);
        data >>= 1;
    }
    return 0;
}

```
