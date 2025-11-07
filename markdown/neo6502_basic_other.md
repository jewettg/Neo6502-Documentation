# The Inline Assembler

The inline assembler works in a very similar way to that of the BBC Micro, except that it does not use the square brackets [ and ] to delimit assembler code. Assembler code is in normal BASIC programs.

A simple example shown below (in the samples directory).  It prints a row of 10 asterisks.

Most standard 65C02 syntax is supported, except currently you cannot use lsr a ; it has to be just lsr (and similarly for rol, asl, ror,inc and dec).

You can also pass A X Y as variables. So you could delete line 150 and run it with X = 12: sys start which would print 12 asterisks.

| **Line** | **Code** <img src="images/empty.gif" width="200" height="1"> | **Notes**                                                                                                             |
| -------- | ------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------- |
| `100`    | `mem= alloc(32)`                                             | Allocate 32 bytes of memory to store the program code.                                                                |
| `110`    | `for i = 0 to 1`                                             | We pass through the code twice because of forward referenced labels. This actually doesn't apply here.                |
| `120`    | `p = mem`                                                    | P is the code pointer -- it is like `$* = {xx}` - it means put the code here                                          |
| `130`    | `o = i * 3`                                                  | Bit 0 is the pass (0 or 1) Bit 1 should display the code generated on pass 2 only, this is stored in 'O' for options. |
| `140`    | `.start`                                                     | Superfluous -- creates a label '`start`' -- which contains the address here                                           |
| `150`    | `ldx #10`                                                    | Use X to count the starts                                                                                             |
| `160`    | `.loop1`                                                     | Loop position. We can't use loop because it's a keyword                                                               |
| `170`    | `lda #42`                                                    | ASCII code for asterisk                                                                                               |
| `180`    | `jsr $fff1`                                                  | Monitor instruction to print a character                                                                              |
| `190`    | `dex`                                                        | Classic 6502 loop                                                                                                     |
| `200`    | `bne loop1`                                                  | If loop counter has not been reach, repeat loop (loop1).                                                              |
| `210`    | `rts`                                                        | Return to caller                                                                                                      |
| `220`    | `next`                                                       | Do it twice and complete both passes                                                                                  |
| `230`    | `sys mem`                                                    | BASIC instruction to 'call 6502 code'. Could do sys start here.                                                       |

## [] Operator

The [] operator is used like an array, but it is a syntactic equivalent of deek and doke, e.g. reading and writing 16 bytes. mem[x] means the 16 bit value in mem + x$* 2, so if mem = 813 then mem[2] = -1 writes a 16-bit word to 817 and 818, and print mem[2] reads it. The index can only be from 0 .. 127

The purpose of this is to provide a clean readable interface to data in 65C02 and other programs running under assembly language; often accessing elements in the 'array' as a structure.

## Zero-Page Usage

Neo6502 is a clean machine, rather like the Sharp machines in the 1980s. When BASIC is not running it has no effect on anything, nor does the firmware.  For example, unlike on the Commodore 64, changing some zero-page locations can cause crashes.

However, BASIC does make use of zero-page. At the time of writing this is memory locations `$10` - `$41 `.

These can however be used in machine code programs called via `SYS`. Only 4 bytes of that usage is system critical (the line pointer and the stack pointer), those are saved on the stack by `SYS`, so even if you overwrite them it does not matter.

However, you can't use this range to store intermediate values *between* sys calls. It is advised that you work usage backwards from `$FF` (as BASIC is
developed forwards from `$10`). It is very unlikely that these will meet in the middle.

`\$00` and `$01` are used on BASIC boot (and maybe other languages later) but this should not affect anything.
