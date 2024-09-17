# About the Neo6502

The Neo6502 is a standalone modern retro computer with a real W65C02 processor and RP2040 co-processor.  This small device works 3-times-faster than any of the other recent 6502 competitors and 30-times-faster than 6502 based machines from the 1980s. 

The “Neo” name was used two reasons: First it implies a modern design; Second came
from the analogy with the movie The Matrix where the W65C02 lives in virtual world – thinking it has real memory, video and keyboard – however in reality it is all virtual and emulated with the RP2040.  

Both the Neo6502 and Neo6502pc are [open-source hardware](https://freedomdefined.org/OSHW), with all CAD files and firmware available to support the future development of software and enhancements to the hardware.

There are two models available:

- The Neo6502, an open circuit board computer (2 revisions, A & B).

- The Neo6502pc, a Neo6502 enclosed in a 3D-printed case with a LCD display, USB ports, UEXT and 6502 interface ports and more.

More technical specifications can be found in Appendix A (page 75).  More information about the Neo6502 project, please refer to the Neo6502 website: [www.neo6502.com](http://www.neo6502.com)

## About the W65C02 processor

The W65C02, being a more modern 6502 than the old retro metal oxide semiconductor chip (mos) – in that it can go much faster than was possible in the 1970s and 1980s.  The W65C02 can even be overclocked to 16 MHz, but on the Neo6502 it is running at 6.25 Mhz, which is closer to the clock speed of the Amiga and Atari ST than the Atari or C64, and a lot faster than when most of the retro games were being coded. 

The Neo6502 features a real W65C02S processor, which does all the computing with real timing versus emulation, but the real power of the machine coms from the RP2040
which provides the memory, video, keyboard input, and additional IO for SPI, I2C, UART, and so on.

Things like complex math (multiplication, floating-point) and graphics are also handled by the RP2040, acting like a co-processor.  Unlike other similar architectures, the RP2040 has direct memory access (providing the memory for the 6502) so there are no additional big data transfers between the chips to wait for, making things all much more efficient.

The processor gets 64kb of RAM, but there is 2 MB of flash memory on board, access to USB flash drive for storage via USB or expansion port (for SD card support), and there is a 40-pin connector that offers up a bus of all the 6502 signals and pins that can be used to interface with or use for experiments.  The UEXT ports already support quite a few modules from Olimex that support UEXT specification ([UEXT Modules](https://www.olimex.com/Products/Modules/)).

# 
