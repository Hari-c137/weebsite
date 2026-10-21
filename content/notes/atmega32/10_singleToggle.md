---
title: toggling single bit of a PORTx
tags:
  - embedded_system
  - low-level
---

this is similiar to previous program [[09_serialToggle.md]] where we only turn on a single pin of the port. and instead of transfering data serially, here we just simply toggle the pin with a ```_delay_ms(1000)```

```c

#include <avr/io.h>
#include <util/delay.h>

int main(void) {
    DDRB |= (1 << 4);
    
    for(;;) {
    PORTB ^= (1 << 4); /* using XOR for toggling */
    _delay_ms(1000);
    }
    return 0;
}

```
