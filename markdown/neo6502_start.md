<img title="" src="images/title_page/the_ultimate_guide.png" alt="" width="100%" align="center"  data-align="center">

<img title="" src="images/title_page/neo6502_swirl.png" alt="" width="371" align="center" data-align="center">

<img title="" src="images/title_page/NEO6502_Logo_Blue_Hoz.png" alt="" width="100%" align="center"  data-align="center">

<img title="" src="images/title_page/olimex_logo.png" alt="" width="344" align="center"  data-align="left">

<table width="100%">
  <tr>
    <td align="top" width="35%">
<b>OLIMEX Ltd.</b><br/>2 Pravda St., P.O. Box 237, <br>Plovdiv 4000 BULGARIA
    </td>
    <td  width="65%">
      <b>Contact:</b> Mr. Tsvetan Usunov<br/><b>Email:</b> <a href="mailto:info@olimex.com">info@olimex.com</a> <br/><b>Voice:</b> +359-32-626259, +359-32-267407, <br>+359-32-621270 
    </td>
  </tr>
  </table>

# Welcome – please read!

Welcome to the modern retro computer world, where you can experience the technology from the 70s and 80s, but with a modern spin on it!

This document covers both the Neo6502 and Neo6502pc computer.  Detailed specifications and the differences between the two can be found in Appendix A.

Neither of the devices (the Neo6502 and Neo6502pc) are turn-key solutions.  Both devi<tabces require intermediate electronics and computer use knowledge.  While both devices have appeared in social media as an out-of-the-box video game platform, it will require that you read this document, so that you gain the best experience!

## Please Note

<div>
<img title="please note" src="images/please_note.png" alt="please note" width="138" data-align="left">
Regardless of the function you are hoping to utilize the Neo6502 or Neo6502pc, you must be familiar with the process of reprogramming (also known as flashing firmware) the 2MB flash memory utilized by the RP2040.  The firmware defines what function the Neo6502 or Neo6502pc will perform.  Current firmware available provide a BASIC interpreter (NeoBASIC) that is continues to be developed and improved, an Apple ][ emulator (using the real W6502), and an Oric Atmos.  Many and other firmware packages are currently being developed, so explore the various user forums, Discord, and Facebook to discover the endless possibilities of the Neo6502 and Neo6502pc.
<br>

<img src="images/arrow_right_blue.png" title="arrow" alt="arrow" align="left" vertical-align="top" width="59"> Please read the Programming the RP2040 Section

</div>

**Both devices require that you obtain or supply the following for proper operation:**

**Neo6502**

- USB-C Power Source (5v, 1 amp minimum, more based on peripherals attached).

- A USB cable with a USB-A on one end, and the appropriate end that will connect to your computer *(used to re-program the RP2040)*.

- *Optional,* enclosing case for the Neo6502, *available from Olimex.*

- *Optional*, USB-A Flash Drive (*highly recommend USB3, ~8 GB*), formatted FAT32.

- *Optional*, USB Hub (*Olimex USB-NeoHub is highly recommended for compatibility*).

- *Optional,* USB Gamepad.

**Neo6502pc**

- USB-C Power Source (5v, 1 amp minimum, more based on peripherals attached).

- A USB cable with a USB-C on one end, and the appropriate end that will connect to your computer.

- USB-A Flash Drive (highly recommend USB3, ~8 GB), formatted FAT32.

- USB Keyboard *(wired and wireless w/USB dongle), bluetooth is not supported.*

- *Optional,* USB Gamepad.

----

# Document Formatting Conventions

In this documentation, the following standards will be used:

- Code examples and parameters will be displayed in a fixed-space font.

- parameters are listed in italics, and have a descriptive name to know what should be substituted, and the entire parameter is replaced with the described value.

- parameters enclosed with square braces [] are optional.

- The symbol ↩︎ indicates the entry of the key carriage return (return key).

- The use of an ellipse (…) indicates a range, starting the alphanumeric and ending with the last alphanumeric.

## Other Symbols

<div>
<table width="100%">
  <tr>
    <td width="10%">
         <img title="" src="images/title_page/eye_.png" alt="" width="50" data-align="center">
    </td> 
    <td width = "90%"> Shown near a paragraph or example code indicates an unusual feature to which you should pay attention to syntax details for proper execution.
  </tr>
  <tr>
    <td width="10%"><img title="" src="images/title_page/exclamation_mark.png" alt="" data-align="center" width="50">
    </td>
      <td width="90%">Shown near a paragraph or example code indicates the need to be careful or alert when using following the directions or code example.  It may cause a crash or problem there you may lose your work and may need to restart the Neo6502.
    </td>
  </tr></table>
  </div>

## Immediate -vs- Deferred Execution Commands

Many, but not all commands can be executed immediately by just typing the command at the beginning of a line and pressing return.

All commands (with a few exceptions) can be used within a program and can be preceded with a line number to add it to a program.   Execute those commands as part of the program by typing "run ↩︎”, and to see a listing of your program type “list ↩︎ “.

## MOS -vs- mos

There are several locations in the guide where the acronym “mos” is used.  To help ensure that the correct acronym expansion is interpreted correctly, here is a key:

- **MOS** (all uppercase letters) = Machine Operating System.

- **mos** (all lowercase letters) = metal oxide semiconductor.

Also note, that the command “mos” can be both “MOS” and “mos” and the command comes from the uppercase “MOS” (Machine Operating System).
