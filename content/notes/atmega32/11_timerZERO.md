---
title: creating a custom ATmega32 delay() using timer0
tags:
  - embedded_system
  - low-level
---

so far all the programs we did use the in-built C delay function ```_delay_ms()``` from the ```util/delay.h``` library. but on embedded system development it is more suitable to create your own native delay implementation through **Timers**

ATmega32 provides 3 Timer:

* Timer0: 8-bit timer
* Timer1: 16-bit timer
* Timer2: 8-bit timer

for keeping the explantion short i wont be going to going into details (not today.. cause i have to keep this short for college purpose(my college is shit), but after the exams are over, i will be nerding out on timers in a seperate page)

if you want to know a more about timers in ATmega32 for the time being refer to this page [here](https://www.electronicwings.com/avr-atmega/atmega1632-timer)

in this example are going set **CLOCK SOURCE SELECT** CS00: 1, CS01: 0 and CS02: 1 to acheive the maximum delay possible which is ```clk / 1024``` without any use of external clock source or prescaler

this program contains 2 functions: our custom delay implementation called  ```ourdelay()``` and a function that toggles a specified PORTx similar to the previous program [[10_singleToggle]]

## creating our own delay()

```c
#include <avr/io.h>

void ourdelay() {
    uint8_t i;
        TCCR0 = (1 << CS02) | (1 << CS00);
        TCNT0 = 0;
        TIFR |= (1 << TOV0);

        while ((TIFR & (1 << TOV0)) == 0)
            ;
        TCCR0 = 0;

}

```

## creating our simple main()

```c
int main(void) {
    DDRB |= (1 << 5);
    while(1) {
        PORTB ^= (1 << 5);
        ourdelay();
    }
    return 0;
} 
```

# full implementation

```c

#include <avr/io.h>

void ourdelay() {
        TCCR0 = (1 << CS02) | (1 << CS00);
        TCNT0 = 0;
        TIFR |= (1 << TOV0); /* toggling TOVO register on TIFR */

        while ((TIFR & (1 << TOV0)) == 0) /* priority order important */
            ;
        TCCR0 = 0;

}
int main(void) {
    DDRB |= (1 << 5);
    while(1) {
        PORTB ^= (1 << 5);
        ourdelay();
    }
    return 0;
}

```

*warning while compiling this program!. all the timer0 calculation were done under a assumption that the system is running at 8Mhz Clock speed. therefore if you are compiling with Higher Clock speed modify the above program to accomodate the changes*


