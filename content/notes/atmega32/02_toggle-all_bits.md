---
title: toggle all bits of PORTx with a delay
tags:
  - embedded_system
  - low-level
---

### **Creating a AVR program to toggle all bits of PORTx** 

firstly we need to set **PORTA** as ouput by setting ```DDRA = OxFF``` and set the initial value at PORTA as ```0x00``` (to remove any garbage value already present in the PORTA register and for the XOR operation to work correctly.

we can then toggle ```PORTA``` by XOR-ing it with the ```0xFF```. in C we can use ```^``` bitwise operator.

```c
#include <avr/io.h>
#include <util/delay.h>

int main(void) {
    DDRA = 0xFF;
    PORTA = 0x00;

    while (1) {
        PORTA ^= 0xFF;
        _delay_ms(500);
    }
}
```
