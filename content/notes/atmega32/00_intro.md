---
title: ATmega32 Development Shell
tags:
  - embedded_system
  - low-level
  - NixOS
---
 
this is my college lab notes for Semester 5 subject **Embedded System Development** that mainly goes over basics of low level system architecture and development of atmega32 on a simulator like proteus or simulide( this notes focuses on development with the simulide)
  
before starting we need to setup a development enviroment for using simulide, avr-libc, GCC compiler with avr compatability.
we can do this by creating a simple devshell using nix

## **devshell for ATmega32**

writing a devshell for ATmega32 is similar to making any other devshell for other languages or development but with only one major difference, in ATmega32 we need native build packages ```x86_64``` aswell as cross compiled packages ```avr```

```avr packages ``` \
**avr-gcc:** gcc compiler compatable with avr \
**avr-libc:** standard C-library for avr \
**binutils:** build against avr  \
```x86_64 package ``` \
**simulide:** for simulating ATmega32 chip

```nix
{
  description = "devshell for ATmega32 development";

  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";
    flake-utils.url = "github:numtide/flake-utils";
  };

  outputs = { self, nixpkgs, flake-utils, ... }:
    flake-utils.lib.eachDefaultSystem (system:
      let
        pkgs = nixpkgs.legacyPackages.${system};
        cross = pkgs.pkgsCross.avr;
      in {
        devShells.default = pkgs.mkShell {
          name = "avr-devshell";

          buildInputs = [
            cross.buildPackages.gcc
            cross.buildPackages.binutils
            cross.avrlibc
            pkgs.simulide
          ];

          shellHook = ''
            echo "AVR dev shell active"
            echo "CC = $CC"
            echo "AR = $AR"
            echo "Simulide binary: $(which simulide)"
          '';

        };
      });
}
```

```sh 
nix develop . # activate the devshell from the flake dir
```

after executing the devshell activation, you will enter the
devshell with all the required dependencies installed. to check if all is working correctly we can run a simple C program.

## **AVR C program to toggle a led at PB0**

```c
#include <avr/io.h>
#include <util/delay.h>

int main(void) {
    DDRB |= (1 << 0); // PB0 as output
    for(;;) {
        PORTB ^= (1 << 0); // Toggle LED
        _delay_ms(500);
    }
}
```

you can compile this code snippet by:

```shell
$ avr-gcc -mmcu=atmega32 -Os -o main.elf main.c
$ avr-objcopy -O ihex -R .eeprom main.elf main.hex 
```


this will generate an intel hex binary with stripped eeprom daa  for our toggle program that we then can use on simulide.
