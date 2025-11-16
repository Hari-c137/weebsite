---
title: sending a range of value to a specific port
tags:
  - embedded_system
  - low-level
---

in this example. we are going to send a range of value [ x1, x2, x3, ... xN ] to a specific PORT. we can do this in C by using a basic ```for loop```

we can use a variable ```i``` with initial value of -4 and use a for-loop to iterate till +4. the datatype ```i``` should be of ```int8_t ``` to ensure proper 8-bit signed values. then we can pass the var ```i``` to ```PORTB``` by **casting**  which will converts the negative numbers  to their corresponding **Two's-complement** 8-bit value

```c
#include <avr/io.h>
#include <util/delay.h>
#define F_CPU 16000000UL

int main(void) {
 DDRB = 0xFF;
 for(int8_t i = -4; i <= 4; i++) { /* unt8_t for signed value */
     PORTB = (uint8_t)i; /* casting for proper 2's complement */
    _delay_ms(1000);
  }
}
```
