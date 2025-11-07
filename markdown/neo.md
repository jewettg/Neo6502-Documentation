



## --------------------------------------------------------

### MOS Commands

Using the MOS commands to access OS disk/file functionality.

MOS commands can be used in four different ways:

- On the NeoBASIC command line, prefixing the command with an
  asterisk.\
  **Example**: \*del myfile.txt

- On the NeoBASIC command line, use the mos BASIC command. Surround
  the MOS command in quotes.\
  **Example**: mos \"del myfile.txt\"

- Within a NeoBASIC program, you can use the mos BASIC function.
  Surround the MOS command in quotes.\
  **Example**: if mos(\"del myfile.txt\") \> 0 then ...

**Available MOS Commands**

+----+----------------------------------+----+-----------------------+
| c  | Prints out the file listing of   | r  | Will rename a file or |
| at | the current directory.           | en | directory. Works      |
|    |                                  |    | identically as the    |
|    |                                  |    | copy command.         |
+====+==================================+====+=======================+
| d  | Delete the specified file.\      | md | Make a directory.     |
| el | Can be used to delete a          |    | Specify the name of   |
|    | directory, only if it is empty.  |    | the directory you     |
|    | *If a directory is not empty,    |    | want to create.       |
|    | the command generates an error.* |    |                       |
+----+----------------------------------+----+-----------------------+
| co | Copy on path to another.         | cd | Change Directory.     |
| py |                                  |    |                       |
|    | Requires two parameters, the     |    | Specify the name of   |
|    | first is the source path and     |    | the directory that    |
|    | file, and the second is the      |    | you wish to traverse. |
|    | destination path and file.       |    |                       |
|    |                                  |    |                       |
|    | If no path is given, but         |    |                       |
|    | different names are given, then  |    |                       |
|    | you get a copy of the source     |    |                       |
|    | file specified in the first      |    |                       |
|    | parameter in the current         |    |                       |
|    | directory.                       |    |                       |
|    |                                  |    |                       |
|    | You cannot make copies of        |    |                       |
|    | directories.                     |    |                       |
+----+----------------------------------+----+-----------------------+
| fi | Will verify that a file (*not a  |    |                       |
| le | directory*) exists. Return zero  |    |                       |
|    | if it exists, and non-zero if it |    |                       |
|    | does not exist.                  |    |                       |
+----+----------------------------------+----+-----------------------+

**Standard Unix POSIX paths are used with the MOS commands.**

  -----------------------------------------------------------------------

  ./    Current directory

  ----- -----------------------------------------------------------------

  ../   Go up in the hierarchy, relative to the current directory
        (*sometime referred as the going back a directory*).

/     The root or top-level directory.
-----------------------------------------------------------------------

**MOS Commands (*continued)***

### MOS Error Codes

Most of the functions will return an error/status code to indicate
whether the operation succeeded or not.

  ---------------------------------------------------------------------------

  Name                          Value  Meaning

  ---------------------------- ------- --------------------------------------

  FIOERROR_OK                   0x00   Operation succeeded (not an error)

  FIOERROR_UNKNOWN              0x01   Something went wrong, but we don\'t
                                       know what

  FIOERROR_EOF                  0x02   A read or directory enumeration
                                       operation reached the end of the file

  FIOERROR_UNIMPLEMENTED        0x03   Operation is not implemented

  FIOERROR_NO_FILE              0x11   Could not find the file

  FIOERROR_NO_PATH              0x12   Could not find the path

  FIOERROR_INVALID_DRIVE        0x13   The logical drive number is invalid

  FIOERROR_INVALID_NAME         0x14   The path name format is invalid

  FIOERROR_INVALID_PARAMETER    0x15   Given parameter is invalid

  FIOERROR_DENIED               0x21   Access denied due to prohibited access
                                       or disk or directory full

  FIOERROR_EXIST                0x22   Access denied due to prohibited access

  FIOERROR_INVALID_OBJECT       0x23   The file/directory object is invalid

  FIOERROR_WRITE_PROTECTED      0x24   The physical drive is write-protected

  FIOERROR_LOCKED               0x25   File is in use

  FIOERROR_DISK_ERR             0x31   A hard error occurred in the low-level
                                       disk I/O layer

  FIOERROR_INT_ERR              0x32   Assertion failed

  FIOERROR_NOT_READY            0x33   The physical drive cannot work

  FIOERROR_NOT_ENABLED          0x34   The volume has no work area

FIOERROR_NO_FILESYSTEM        0x35   The filesystem is invalid
---------------------------------------------------------------------------

### File Attributes

  -------------------------------------------------------------------------

  Name                Value   Meaning

  ------------------- ------- ---------------------------------------------

  FIOATTR_DIR         0x01    This is a directory (may not be modified)

  FIOATTR_SYSTEM      0x02    This is a system file and will be hidden from
                              directory listings

  FIOATTR_ARCHIVE     0x04    File is archived; automatically cleared when
                              the file is modified

  FIOATTR_READONLY    0x08    File is read only and may not be overwritten
                              or modified

FIOATTR_HIDDEN      0x10    This will be hidden from directory listings
-------------------------------------------------------------------------

### 

### 

### 

## The Inline Assembler

The inline assembler works in a very similar way to that of the BBC
Micro, except that it does not use the square brackets \[ and \] to
delimit assembler code. Assembler code is in normal BASIC programs.

A simple example shown below (in the samples directory). It prints a row
of 10 asterisks.

Most standard 65C02 syntax is supported, except currently you cannot use
lsr a ; it has to be just lsr (and similarly for rol, asl, ror,inc and
dec).

You can also pass A X Y as variables. So you could delete line 150 and
run it with X = 12: sys start which would print 12 asterisks.

  -----------------------------------------------------------------------------

  **Line**   **Code**         **Notes**

  ---------- ---------------- -------------------------------------------------

  100        mem = alloc(32)  Allocate 32 bytes of memory to store the program
                              code.

  110        for i = 0 to 1   We pass through the code twice because of forward
                              referenced labels. This actually doesn\'t apply
                              here.

  120        p = mem          P is the code pointer \-- it is like \$\* =
                              {xx} - it means put the code here

  130        o = i \* 3       Bit 0 is the pass (0 or 1) Bit 1 should display
                              the code generated on pass 2 only, this is stored
                              in \'O\' for options.

  140        .start           Superfluous \-- creates a label \'start\' \--
                              which contains the address here

  150        ldx #10          Use X to count the starts

  160        .loop1           Loop position. We can\'t use loop because it\'s a
                              keyword

  170        lda #42          ASCII code for asterisk

  180        jsr \$fff1       Monitor instruction to print a character

  190        dex              Classic 6502 loop

  200        bne loop1        

  210        rts              Return to caller

  220        next             Do it twice and complete both passes

  230        sys mem          BASIC instruction to \'call 6502 code\'. Could do
                              sys start here.

  -----------------------------------------------------------------------------

## \[\] Operator

The \[\] operator is used like an array, but it is a syntactic
equivalent of deek and doke, e.g. reading and writing 16 bytes. mem\[x\]
means the 16 bit value in mem + x \$\* 2, so if mem = 813 then mem\[2\]
= -1 writes a 16-bit word to 817 and 818, and print mem\[2\] reads it.
The index can only be from 0 ..127

The purpose of this is to provide a clean readable interface to data in
65C02 and other programs running under assembly language; often
accessing elements in the \'array\' as a structure.

## Zero-Page Usage

Neo6502 is a clean machine, rather like the Sharp machines in the 1980s.
When BASIC is not running it has no effect on anything, nor does the
firmware. For example, unlike on the Commodore 64, changing some
zero-page locations can cause crashes.

However, BASIC does make use of zero-page. At the time of writing this
is memory locations \$10-\$41.

These can however be used in machine code programs called via SYS. Only
4 bytes of that usage is system critical (the line pointer and the stack
pointer), those are saved on the stack by SYS, so even if you overwrite
them it does not matter.

However, you can\'t use this range to store intermediate
values *between* sys calls. It is advised that you work usage backwards
from \$FF (as BASIC is developed forwards from \$10). It is very
unlikely that these will meet in the middle.

\$00 and \$01 are used on BASIC boot (and maybe other languages later)
but this should not affect anything.

# Raspberry PI 2040 Messaging API

The Neo6502 uses a Raspberry PI 2040 to provide memory, graphics
support, file I/O capabilities and external (UEXT) interface. While the
BASIC and other programming languages implement many of these
capabilities as command and functions, many of the routines can be
access natively via the RP2040 messaging API.

The Neo6502 API is a messaging system. There are no methods to access
the hardware directly. Messages are passed via the block of memory from
\$FF00 to \$FF0F, as specified in the \"API Messaging Addresses\" table
below.

**API Messaging Addresses**

+-------+--------+-----------------------------------------------------+
| Ad    | Type   | Notes                                               |
| dress |        |                                                     |
+=======+========+=====================================================+
| \     | Group  | Group selector and status. Writing a non-zero value |
| $FF00 |        | to this location triggers the routine specified in  |
|       |        | \$FF01. The system will respond by setting the      |
|       |        | "error" and "parameters" values appropriately. Upon |
|       |        | completion, this memory location will be will       |
|       |        | cleared.                                            |
+-------+--------+-----------------------------------------------------+
| \     | Fu     | A command or function within the specified group.   |
| $FF01 | nction |                                                     |
+-------+--------+-----------------------------------------------------+
| \     | Error  | Return any error values, 0 = no error.              |
| $FF02 |        |                                                     |
+-------+--------+-----------------------------------------------------+
| \$F   | Status | Set (1) if the ESCape key has been pressed. This is |
| F03:7 |        | not automatically reset.                            |
+-------+--------+-----------------------------------------------------+
| Note: |        |                                                     |
| F     |        |                                                     |
| F03:0 |        |                                                     |
| th    |        |                                                     |
| rough |        |                                                     |
| F     |        |                                                     |
| F03:6 |        |                                                     |
| are   |        |                                                     |
| not   |        |                                                     |
| used. |        |                                                     |
+-------+--------+-----------------------------------------------------+
| \     | Para   | This memory block is notated in this document as    |
| $FF04 | meters | Param\[0\] (\$FF04) through Param\[7\] (\$FF0B),    |
|       |        | each a single byte, or combined (Param\[0,1\] is a  |
| ...   |        | word *representing a larger value*). The addressing |
|       |        | space is little-endian, least significant byte      |
| \     |        | (LSB) at the lowest memory address.                 |
| $FF0B |        |                                                     |
|       |        | Many functions require values be present in these   |
|       |        | memory locations (as parameters of the function).   |
|       |        | Functions may also return values in these memory    |
|       |        | locations.                                          |
+-------+--------+-----------------------------------------------------+

The above address map and the tables describing the functions found in
this section can be found in numerous sections of the firmware release
download:

- examples/assembly/neo6502.inc

- examples/C/neo6502.h

**\
**

## Using RP2040 messaging API in NeoBASIC

### Procedure to make Messaging API call

  -----------------------------------------------------------------------

  proc sendmsg(g,f)                   Define a procedure to "send a
                                      message" using parameters f
                                      (function), and p (parameters)

  ----------------------------------- -----------------------------------

  while peek(\$FF00):wend             Wait until the messaging API is
                                      ready to accept a message (all
                                      previous commands in queue have
                                      completed)

  poke \$FF01,f:poke \$FF00,g         Poke the function and group. If
                                      there are parameters to pass, you
                                      must have those entered into memory
                                      (poke) prior to performing "poke
                                      \$ff00,g", as this call the
                                      function immediately.

  while peek(\$FF00):wend             Wait until for the function to
                                      complete.

endproc                             End the procedure.
-----------------------------------------------------------------------

The above code allows you to make a messaging API call. Example:
sendmsg(2,12) will clear the screen.

### Messaging API calls with parameters in NeoBASIC

Since most functions need parameters, you need to specify those in the
appropriate memory addresses before making the messaging API call.

x1=100,y1=100,x2=200,y2=200

doke \$FF04

**\
\
**

## API Commands/Functions

*Grouped by functionality in the tables.*

**Note:** That these are referring to a mapping to memory locations. The
numbers \[0..7\] represent offsets from the parameters base address
\$FF04. The actual bytes are not necessarily all distinct
\"parameters\", depending on the function or routine, a parameter may be
an individual byte; one or more bits of a byte interpreted as a
composite or bit-field; or multiple adjacent bytes interpreted as 16 or
32 bit values.

**For example**: The list Param\[0,1\] would indicate a single logical
parameter, comprised of the two adjacent bytes \$FF04 and \$FF05. The
range Params\[4..7\] would indicate a single logical parameter, spanning
consecutive bytes between \$FF08 and \$FF0B.\
\
An integer like "320" would be represented as \$FF04 = 40, \$FF05= 01.

**Note: Strings referenced by parameters are not ASCIIZ, but are
length-prefixed.** The first byte represents the length of the string
(not counting itself). The string begins at the second byte.
Consequently, strings must be 255 bytes or less (not counting the length
header).

## System

+----+------------+---------------------------------------------------+
| G  | Function   | Description and Example                           |
| ,F |            |                                                   |
+====+============+===================================================+
| 1  | DSP Reset  | Resets the messaging system and component         |
| ,0 |            | systems. Normally, should not be used.            |
+----+------------+---------------------------------------------------+
| 1  | Timer      | Deposit the value (32-bits) of the 100Hz system   |
| ,1 |            | timer into parameters:0..3.                       |
+----+------------+---------------------------------------------------+
| 1  | Key Status | Deposits the state of the specified keyboard key  |
| ,2 |            | into Parameter:0.\                                |
|    |            | State of keyboard modifiers (Shift/Ctrl/Alt/Meta) |
|    |            | is returned in Parameter:1.                       |
|    |            |                                                   |
|    |            | The key which to query is specified in            |
|    |            | Parameter:0.                                      |
+----+------------+---------------------------------------------------+
| 1  | Basic      | Loads and allows the execution of BASIC via an    |
| ,3 |            | indirect jump through address zero.               |
+----+------------+---------------------------------------------------+
| 1  | Credits    | Print the Neo6502 project contributors (stored in |
| ,4 |            | flash memory).                                    |
+----+------------+---------------------------------------------------+
| 1  | Serial     | Check the serial port to see if there is a data   |
| ,5 | Status     | transmission.                                     |
+----+------------+---------------------------------------------------+
| 1  | Locale     | Set the locale code specified in parameters:0,1   |
| ,6 |            | as upper-case ASCII letters. Parameter:0 takes    |
|    |            | the first letter and Parameter:1 takes the second |
|    |            | letter.                                           |
|    |            |                                                   |
|    |            | For example: French (FR) would require Parameter  |
|    |            | 0 being \$46 and Parameter 1 being \$52           |
+----+------------+---------------------------------------------------+
| 1  | System     | System Reset. This is a full hardware reset. It   |
| ,7 | Reset      | resets the RP2040 using the Watchdog timer, and   |
|    |            | this also resets the 65C02.                       |
+----+------------+---------------------------------------------------+
| 1  | MOS        | Perform a MOS command.                            |
| ,8 |            |                                                   |
+----+------------+---------------------------------------------------+
| 1, | Write      | Writes a single character to the debug port (the  |
| 10 | character  | UART on the Pico, or stderr on the emulator).     |
|    | to debug   | This allows maximum flexibility.                  |
+----+------------+---------------------------------------------------+
| 1, | Return     | Reads the current version: major.minor.patch into |
| 11 | Version    | parameters: 0-2.\                                 |
|    | I          | These values are guaranteed to be in the range 0  |
|    | nformation | -- 255.                                           |
+----+------------+---------------------------------------------------+

## Console

+----+------------+---------------------------------------------------+
| G  | Function   | Description and Example                           |
| ,F |            |                                                   |
+====+============+===================================================+
| 2  | Write      | Console out.                                      |
| ,0 | Character  |                                                   |
|    |            | *This is a duplicate* *of* function 2,6 *for      |
|    |            | backward compatibility.*                          |
+----+------------+---------------------------------------------------+
| 2  | Read       | Read and remove a key press from the keyboard     |
| ,1 | Character  | queue into Parameter:0. This is the ASCII value   |
|    |            | of the keystroke. If there are no key presses in  |
|    |            | the queue, Parameter:0 will be zero.              |
|    |            |                                                   |
|    |            | **Note:** This function is better suited for text |
|    |            | input, but not for games. See function 7,1 (read  |
|    |            | default controller) is better suited for games,   |
|    |            | as this only detects key presses. It does not     |
|    |            | include the ability to check whether the key is   |
|    |            | currently down or not.                            |
+----+------------+---------------------------------------------------+
| 2  | Console    | Check to see if the keyboard queue is empty. If   |
| ,2 | Status     | it is, Parameter:0 will be \$FF, otherwise it     |
|    |            | will be \$00                                      |
+----+------------+---------------------------------------------------+
| 2  | Read Line  | Input the current line below the cursor into      |
| ,3 |            | parameters:0,1 as a length-prefixed string; and   |
|    |            | move the cursor to the line below. Handles        |
|    |            | multiple-line input.                              |
+----+------------+---------------------------------------------------+
| 2  | Define     | Define the function key F1..F10 specified in      |
| ,4 | Hotkey     | Parameter:0 as 1..10 to emit the length-prefixed  |
|    |            | string stored at the memory location specified in |
|    |            | parameters:2,3.                                   |
|    |            |                                                   |
|    |            | F11 and F12 cannot currently be defined.          |
+----+------------+---------------------------------------------------+
| 2  | Define     | Define a font character specified in Parameter:0  |
| ,5 | Character  | within the range of 192..255. Fill bits 0..5      |
|    |            | (columns) of parameters:1..7 (rows) with the      |
|    |            | character bitmap.                                 |
+----+------------+---------------------------------------------------+
| 2  | Write      | Write the character specified in Parameter:0 to   |
| ,6 | Character  | the console at the cursor position. Refer to      |
|    |            | Section \"Console Codes\" for details.            |
+----+------------+---------------------------------------------------+
| 2  | Set Cursor | Move the cursor to the screen character cell      |
| ,7 | Pos        | Parameter:0 (X), Parameter:1 (Y).                 |
+----+------------+---------------------------------------------------+
| 2  | List       | Display the current function key definitions      |
| ,8 | Hotkeys    |                                                   |
+----+------------+---------------------------------------------------+
| 2  | Screen     | Returns the console size in characters, in        |
| ,9 | Size       | Parameter:0 (height) and Parameter:1 (width).     |
+----+------------+---------------------------------------------------+
| 2, | Insert     | This is an internal function (inserting a blank   |
| 10 | Line       | line on the console) and is not officially        |
|    |            | supported. *It is not recommended for use and     |
|    |            | maybe obsoleted or the functionality may change.* |
+----+------------+---------------------------------------------------+
| 2, | Delete     | This is an internal function (deletes line from   |
| 11 | Line       | the console) and is not officially supported. *It |
|    |            | is not recommended for use and maybe obsoleted or |
|    |            | the functionality may change.*                    |
+----+------------+---------------------------------------------------+
| 2, | Clear      | Clears the screen. Equivalent to the "cls" BASIC  |
| 12 | Screen     | command.                                          |
+----+------------+---------------------------------------------------+
| 2, | Get Cursor | Returns the current screen character cell of the  |
| 13 | Position   | cursor in Parameter:0 (X), Parameter:1 (Y).       |
+----+------------+---------------------------------------------------+
| 2, | Clear Text | Erase all characters within the rectangular       |
| 14 | Region     | region specified in parameters:0,1 (begin X,Y)    |
|    |            | and parameters:2,3 (end X,Y).                     |
+----+------------+---------------------------------------------------+
| 2, | Set Text   | Sets the foreground color to Parameter:0 and the  |
| 15 | Color      | background color to Parameter:1                   |
+----+------------+---------------------------------------------------+
| 2, | Cursor     | This is an internal function (inverts/swaps the   |
| 16 | Inverse    | foreground and background colors -- normal -vs-   |
|    |            | inverse) and is not officially supported. *It is  |
|    |            | not recommended for use and maybe obsoleted or    |
|    |            | the functionality may change.*                    |
+----+------------+---------------------------------------------------+
| 2, | Tab        | Moves the cursor to the right until it reaches    |
| 17 |            | the position in Parameter 0. This is an internal  |
|    |            | helper function. *It is not recommended for use   |
|    |            | and maybe obsoleted or the functionality may      |
|    |            | change.*                                          |
+----+------------+---------------------------------------------------+
| 2, | Read       | Read the foreground and background RGB colors     |
| 18 | foreground | into Param\[0\] and Param\[1\]                    |
|    | and        |                                                   |
|    | background |                                                   |
|    | colors     |                                                   |
+----+------------+---------------------------------------------------+
| 2, | Show/Hide  | Set the cursor visibility to Param\[0\]. This is  |
| 19 | Cursor     | reset by clearing the screen.                     |
|    | Reversing  |                                                   |
+----+------------+---------------------------------------------------+

## File I/O

+----+------------+---------------------------------------------------+
| G  | Function   | Description and Example                           |
| ,F |            |                                                   |
+====+============+===================================================+
| 3  | List       | Display the file listing of the present           |
| ,1 | Directory  | directory.                                        |
+----+------------+---------------------------------------------------+
| 3  | Load File  | Load a file by name into memory.                  |
| ,2 |            |                                                   |
|    |            | On input:                                         |
|    |            |                                                   |
|    |            | parameters:0,1 points to the length-prefixed      |
|    |            | filename string;                                  |
|    |            |                                                   |
|    |            | parameters:2,3 contains the location to write the |
|    |            | data to. If the address is \$FFFF, the file will  |
|    |            | instead be loaded into the graphics working       |
|    |            | memory, used for sprites, tiles, images.          |
|    |            |                                                   |
|    |            | On output:                                        |
|    |            |                                                   |
|    |            | Error location contains an error/status code.     |
+----+------------+---------------------------------------------------+
| 3  | Store File | Saves data in memory to a file. On input:         |
| ,3 |            |                                                   |
|    |            | parameters:0,1 points to the length-prefixed      |
|    |            | filename string;                                  |
|    |            |                                                   |
|    |            | parameters:2,3 contains the location to read data |
|    |            | from;                                             |
|    |            |                                                   |
|    |            | parameters:4,5 specified the number of bytes to   |
|    |            | store.                                            |
|    |            |                                                   |
|    |            | On output:                                        |
|    |            |                                                   |
|    |            | Error location contains an error/status code.     |
+----+------------+---------------------------------------------------+
| 3  | File Open  | Opens a file into a specific channel. On input:   |
| ,4 |            |                                                   |
|    |            | Parameter:0 contains the file channel to open;    |
|    |            |                                                   |
|    |            | parameters:1,2 points to the length-prefixed      |
|    |            | filename string;                                  |
|    |            |                                                   |
|    |            | Parameter:3 contains the open mode. See below.    |
|    |            |                                                   |
|    |            | Valid open modes are:                             |
|    |            |                                                   |
|    |            | 0 opens the file for read-only access;            |
|    |            |                                                   |
|    |            | 1 opens the file for write-only access;           |
|    |            |                                                   |
|    |            | 2 opens the file for read-write access;           |
|    |            |                                                   |
|    |            | 3 creates the file if it doesn\'t already exist,  |
|    |            | truncates it if it does, and opens the file for   |
|    |            | read-write access.                                |
|    |            |                                                   |
|    |            | Modes 0 to 2 will fail if the file does not       |
|    |            | already exist. If the channel is already open,    |
|    |            | the call fails. Opening the same file more than   |
|    |            | once on different channels has undefined          |
|    |            | behaviour, and is not recommended.                |
+----+------------+---------------------------------------------------+
| 3  | File Close | Closes a particular channel. On input:            |
| ,5 |            |                                                   |
|    |            | Parameter:0 contains the file channel to close.   |
|    |            | If this is \$FF this closes all open files.       |
+----+------------+---------------------------------------------------+
| 3  | File Seek  | Seeks the file opened on a particular channel to  |
| ,6 |            | a location. On input:                             |
|    |            |                                                   |
|    |            | Parameter:0 contains the file channel to operate  |
|    |            | on;                                               |
|    |            |                                                   |
|    |            | parameters:1..4 contains the file location.       |
|    |            |                                                   |
|    |            | You can seek beyond the end of a file to extend   |
|    |            | the file. However, whether the file size changes  |
|    |            | when the seek happens, or when you perform the    |
|    |            | write is undefined behavior.                      |
+----+------------+---------------------------------------------------+
| 3  | File Tell  | Returns the current seek location for the file    |
| ,7 |            | opened on a particular channel. On input:         |
|    |            |                                                   |
|    |            | Parameter:0 contains the file channel to operate  |
|    |            | on.                                               |
|    |            |                                                   |
|    |            | On output:                                        |
|    |            |                                                   |
|    |            | parameters:1..4 contains the seek location within |
|    |            | the file.                                         |
+----+------------+---------------------------------------------------+
| 3  | File Read  | Reads data from an opened file. On input:         |
| ,8 |            |                                                   |
|    |            | Parameter:0 contains the file channel to operate  |
|    |            | on.                                               |
|    |            |                                                   |
|    |            | parameters:1,2 points to the destination in       |
|    |            | memory,                                           |
|    |            |                                                   |
|    |            | or \$FFFF to read into graphics memory.           |
|    |            |                                                   |
|    |            | parameters:3,4 contains the amount of data to     |
|    |            | read.                                             |
|    |            |                                                   |
|    |            | On output:                                        |
|    |            |                                                   |
|    |            | parameters:3,4 is updated to contain the amount   |
|    |            | of data actually read.                            |
|    |            |                                                   |
|    |            | Data is read from the current seek position,      |
|    |            | which is advanced after the read.                 |
+----+------------+---------------------------------------------------+
| 3  | File Write | Writes data to an opened file. On input:          |
| ,9 |            |                                                   |
|    |            | Parameter:0 contains the file channel to operate  |
|    |            | on;                                               |
|    |            |                                                   |
|    |            | parameters:1,2 points to the data in memory;      |
|    |            |                                                   |
|    |            | parameters:3,4 contains the amount of data to     |
|    |            | write.                                            |
|    |            |                                                   |
|    |            | On output:                                        |
|    |            |                                                   |
|    |            | parameters:3,4 is updated to contain the amount   |
|    |            | of data actually written.                         |
|    |            |                                                   |
|    |            | Data is written to the current seek position,     |
|    |            | which is advanced after the write.                |
+----+------------+---------------------------------------------------+
| 3, | File Size  | Returns the current size of an opened file. On    |
| 10 |            | input:                                            |
|    |            |                                                   |
|    |            | Parameter:0 contains the file channel to operate  |
|    |            | on.                                               |
|    |            |                                                   |
|    |            | On output:                                        |
|    |            |                                                   |
|    |            | parameters:1..4 contains the size of the file.    |
|    |            |                                                   |
|    |            | This call should be used on open files, and takes |
|    |            | into account any buffered data which has not yet  |
|    |            | been written to disk. Consequently, this may      |
|    |            | return a different size than Function 3,16 \"File |
|    |            | Stat\".                                           |
+----+------------+---------------------------------------------------+
| 3, | File Set   | Extends or truncates an opened file to a          |
| 11 | Size       | particular size. On input:                        |
|    |            |                                                   |
|    |            | Parameter:0 contains the file channel to operate  |
|    |            | on;                                               |
|    |            |                                                   |
|    |            | parameters:1..4 contains the new size of the      |
|    |            | file.                                             |
+----+------------+---------------------------------------------------+
| 3, | File       | Renames a file. On input:                         |
| 12 | Rename     |                                                   |
|    |            | parameters:0,1 points to the length-prefixed      |
|    |            | string for the old name;                          |
|    |            |                                                   |
|    |            | parameters:2,3 points to the length-prefixed      |
|    |            | string for the new name.                          |
|    |            |                                                   |
|    |            | Files may be renamed across directories.          |
+----+------------+---------------------------------------------------+
| 3, | File       | Deletes a file or directory. On input:            |
| 13 | Delete     |                                                   |
|    |            | parameters:0,1 points to the length-prefixed      |
|    |            | filename string.                                  |
|    |            |                                                   |
|    |            | Deleting a file which is open has undefined       |
|    |            | behavior. Directories may                         |
|    |            |                                                   |
|    |            | only be deleted if they are empty.                |
+----+------------+---------------------------------------------------+
| 3, | Create     | Creates a new directory. On input:                |
| 14 | Directory  |                                                   |
|    |            | parameters:0,1 points to the length-prefixed      |
|    |            | filename string.                                  |
+----+------------+---------------------------------------------------+
| 3, | Change     | Changes the current working directory. On input:  |
| 15 | Directory  |                                                   |
|    |            | parameters:0,1 points to the length-prefixed path |
|    |            | string.                                           |
+----+------------+---------------------------------------------------+
| 3, | File Stat  | Retrieves information about a file by name. On    |
| 16 |            | input:                                            |
|    |            |                                                   |
|    |            | parameters:0,1 points to the length-prefixed      |
|    |            | filename string.                                  |
|    |            |                                                   |
|    |            | parameters:0..3 contains the length of the file;  |
|    |            |                                                   |
|    |            | Parameter:4 contains the attributes bit-field of  |
|    |            | the file.                                         |
|    |            |                                                   |
|    |            | If the file is open for writing, this may not     |
|    |            | return the correct size due to buffered data not  |
|    |            | having been flushed to disk.                      |
|    |            |                                                   |
|    |            | File attributes are a bitfield as follows:        |
|    |            | 0,0,0,Hidden, Read Only, Archive, System,         |
|    |            | Directory.                                        |
+----+------------+---------------------------------------------------+
| 3, | Open       | Opens a directory for enumeration. On input:      |
| 17 | Directory  |                                                   |
|    |            | parameters:0,1 points to the length-prefixed      |
|    |            | filename string.                                  |
|    |            |                                                   |
|    |            | Only one directory at a time may be opened. If a  |
|    |            | directory is already open when this call is made, |
|    |            | it is automatically closed. However, an open      |
|    |            | directory may make it impossible to delete the    |
|    |            | directory; so closing the directory after use is  |
|    |            | good practice.                                    |
+----+------------+---------------------------------------------------+
| 3, | Read       | Reads an item from the currently open directory.  |
| 18 | Directory  | On input:                                         |
|    |            |                                                   |
|    |            | parameters:0,1 points to a length-prefixed buffer |
|    |            | for returning the filename.                       |
|    |            |                                                   |
|    |            | parameters:0,1 is unchanged, but the buffer is    |
|    |            | updated to contain the                            |
|    |            |                                                   |
|    |            | length-prefixed filename (without any leading     |
|    |            | path);                                            |
|    |            |                                                   |
|    |            | parameters:2..5 contains the length of the file;  |
|    |            |                                                   |
|    |            | Parameter:6 contains the file attributes, as      |
|    |            | described by Function 3,16 \"File Stat\".         |
|    |            |                                                   |
|    |            | If there are no more items to read, this call     |
|    |            | fails and an error is flagged.                    |
+----+------------+---------------------------------------------------+
| 3, | Close      | Closes any directory opened previously by         |
| 19 | Directory  | Function 3,17 \"Open Directory\".                 |
+----+------------+---------------------------------------------------+
| 3, | Copy File  | Copies a file. On input:                          |
| 20 |            |                                                   |
|    |            | parameters:0,1 points to the length-prefixed old  |
|    |            | filename;                                         |
|    |            |                                                   |
|    |            | parameters:2,3 points to the length-prefixed new  |
|    |            | filename.                                         |
|    |            |                                                   |
|    |            | Only single files may be copied, not directories. |
+----+------------+---------------------------------------------------+
| 3, | Set File   | Sets the attributes for a file. On input:         |
| 21 | Attributes |                                                   |
|    |            | parameters:0,1 points to the length-prefixed      |
|    |            | filename;                                         |
|    |            |                                                   |
|    |            | Parameter:2 is the attribute bitfield. (See Stat  |
|    |            | File for details.)                                |
|    |            |                                                   |
|    |            | The directory bit cannot be changed. Obviously.   |
+----+------------+---------------------------------------------------+
| 3, | Check End  | Returns the end of file status of an opened file. |
| 22 | of File    | On input:                                         |
|    | (EOF)      |                                                   |
|    |            | Parameter:0 contains the file channel to operate  |
|    |            | on.                                               |
|    |            |                                                   |
|    |            | On output:                                        |
|    |            |                                                   |
|    |            | Parameter:0 is non-zero if the file is at the end |
|    |            | of the file.                                      |
|    |            |                                                   |
|    |            | This call should be used on open files and may    |
|    |            | return an error if the file is closed.            |
+----+------------+---------------------------------------------------+
| 3, | List       | Prints a filtered file listing of the current     |
| 32 | Filtered   | directory to the console. On input:               |
|    |            |                                                   |
|    |            | parameters:0,1 points to the filename search      |
|    |            | string.                                           |
|    |            |                                                   |
|    |            | Files will only be shown if the name contains the |
|    |            | search string (ie: a substring match).            |
+----+------------+---------------------------------------------------+

## Mathematics

The mathematical interface of the API functions largely as a helper
system for the BASIC interpreted, but it is open to any developer who
wishes to avail themselves of the functionality.

The interface is used in a stack environment but is designed so it could
be used in either a stack environment or a fixed location environment.
The Neo6502 BASIC stack is also \'split\', so elements are not
consecutive, though they can be.

Parameter 0 and 1 specify the address of the registers 1 and 2. Register
1 starts at this address, Register 2 starts at the next address.
Parameter 2 specifies the step to the next register. Therefore they are
interleaved by default at present.

So if parameters 0 and 1 are 8100 and Parameter 2 is 4, the 5 byte
registers are

Register 1: 8100,8104,8108,810C,8110

Register 2: 8101,8105,8109,810D,8111

Bytes 1-4 of the \'register\' are the number, which can be either an
integer (32 bit signed) or a standard \'C\' float (e.g. the IEEE Single
Precision Float format). Bit 0 is the type byte, and the relevant bit is
bit 6, which is set to indicate bytes 1-4 are a float value, and is set
on return.

Binary functions that use int and float combined (one is int and one is
float) normally return a float.

  --------------------------------------------------------------------------------

  G,F    Function              Description and Example

  ------ --------------------- ---------------------------------------------------

  4,0    Addition              Register1 := Register 1 + Register2

  4,1    Subtraction           Register1 := Register 1 - Register2

  4,2    Multiplication        Register1 := Register 1 \* Register2

  4,3    Decimal Division      Register1 := Register 1 / Register2 (floating
                               point)

  4,4    Integer Division      Register1 := Register 1 / Register2 (integer
                               result)

  4,5    Integer Modulus       Register1 := Register 1 mod Register2

  4,6    Compare               Parameter:0 := Register 1 compare Register2 :
                               returns \$FF, 0, 1 for less equal and greater.

  4,7    Power                 Register1 := Register 1 to the power of Register2
                               (floating point result whatever)

  4,8    Distance              Register1 := Square root of (Register1 \*
         (counter-rectangle)   Register1) + (Register2 \* Register2)

  4,9    Angle calculation     Register1 := arctangent2(Register 1,Register 2) -
         (arctangent2)         angle in degrees/radians

  4,16   Negate                Register1 := -Register 1

  4,17   Floor                 Register1 := floor(Register 1)

  4,18   Square Root           Register1 := square root(Register 1)

  4,19   Sine                  Register1 := sine(Register 1) angles in
                               degrees/radians

  4,20   Cosine                Register1 := cosine(Register 1) angles in
                               degrees/radians

  4,21   Tangent               Register1 := tangent(Register 1) angles in
                               degrees/radians

  4,22   Arctangent            Register1 := arctangent(Register 1) angles in
                               degrees/radians

  4,23   Exponent              Register1 := e to the power of Register 1

  4,24   Logarithm             Register1 := log(Register 1) natural logarithm

  4,25   Absolute Value        Register1 := absolute value(Register 1)

  4,26   Sign                  Register1 := sign(Register 1), returns -1 0 or 1

  4,27   Random Decimal        Register1 := random float from 0-1

  4,28   Random Integer        Register1 := random integer from 0 to (Register
                               1-1)

  4,32   Number to Decimal     Helper function for tokenizer, do not use.

  4,33   String to Number      Convert the length prefixed string at
                               parameters:4,5 to a constant in Register1.

  4,34   Number to String      Convert the constant in Register1 to a length
                               prefixed string which is stored at parameters:4,5

  4,35   Set Degree/Radian     Sets the use of degrees (the default) when non
         Mode                  zero, radians when zero.

  --------------------------------------------------------------------------------

## 

## Graphics

  ------------------------------------------------------------------------

  G,F    Function      Description and Example

  ------ ------------- ---------------------------------------------------

  5,1    Set Defaults. Configure the global graphics system settings. Not
                       all parameters are relevant for all graphics
                       commands; but all parameters will be set by this
                       command. So mind their values. Refer to Section
                       \"Graphics Settings\" for details. The parameters
                       are And, Or, Fill Flag, Extent, and Flip. Bit 0 of
                       flip sets the horizontal flip, Bit 1 sets the
                       vertical flip.

  5,2    Draw Line     Draw a line between the screen coordinates
                       specified in parameters:0,1,parameters:2,3 (begin
                       X,Y) and parameters:4,5,parameters:6,7 (end X,Y).

  5,3    Draw          Draw a rectangle spanning the screen coordinates
         Rectangle     specified in parameters:0,1,parameters:2,3 (corner
                       X,Y) and parameters:4,5,parameters:6,7 (opposite
                       corner X,Y).

  5,4    Draw Ellipse  Draw an ellipse spanning the screen coordinates
                       specified in parameters:0,1,parameters:2,3 (corner
                       X,Y) and parameters:4,5,parameters:6,7 (opposite
                       corner X,Y).

  5,5    Draw Pixel    Draw a single pixel at the screen coordinates
                       specified in parameters:0,1,parameters:2,3 (X,Y).

  5,6    Draw Text     Draw the length-prefixed string of text stored at
                       the memory location specified in parameters:4,5 at
                       the screen character cell specified in
                       parameters:0,1,parameters:2,3 (X,Y).

  5,7    Draw Image    Draw the image with image ID in Parameter:4 at the
                       screen coordinates parameters:0,1,parameters:2,3
                       (X,Y). The extent and flip settings influence this
                       command.

  5,8    Draw Tilemap  Draw the current tilemap at the screen coordinates
                       specified in parameters:0,1,parameters:2,3
                       (top-left X,Y) and parameters:4,5,parameters:6,7
                       (bottom-right X,Y) using current graphics settings.

  5,32   Set Palette   Set the palette colour at the index spcified in
                       Parameter:0 to the values in
                       Parameter:1,Parameter:2,Parameter:3 (RGB).

  5,33   Read Pixel    Read a single pixel at the screen coordinates
                       specified in parameters:0,1,parameters:2,3 (X,Y).
                       When the routine completes, the result will be in
                       Parameter:0. If sprites are in use, this will be
                       the background only (0..15), if sprites are not in
                       use it may return (0..255)

  5,34   Reset Palette Reset the palette to the defaults.

  5,35   Set Tilemap   Set the current tilemap. parameters:0,1 is the
                       memory address of the tilemap, and
                       parameters:2,3,parameters:4,5 (X,Y) specifies the
                       offset into the tilemap, in units of pixels, of the
                       top-left pixel of the tile.

  5,36   Read Sprite   Read Pixel from the sprite layer at the screen
         Pixel         coordinates specified in
                       parameters:0,1,parameters:2,3 (X,Y). When the
                       routine completes, the result will be in
                       Parameter:0. Refer to Section \"Pixel Colors\" for
                       details.

  5,37   Frame Count   Deposit into parameters:0..3, the number of
                       v-blanks (full screen redraws) which have occurred
                       since power-on. This is updated at the start of
                       each v-blank period.

  5,38   Get Palette   Get the palette colour at the index spcified in
                       Parameter:0. Values are returned in
                       Parameter:1,Parameter:2,Parameter:3 (RGB).

  5,39   Write Pixel   Write Pixel index Parameter:4 to the screen
                       coordinate specified in
                       parameters:0,1,parameters:2,3 (X,Y).

  5,64   Set Color     Set Color. Sets the current drawing colour to
                       Parameter:0

  5,65   Set Solid     Set Solid Flag. Sets the solid flag to Parameter:0,
         Flag          which indicates either solid fill (for shapes) or
                       solid background (for images and fonts)

  5,66   Set Draw Size Set Draw Size. Sets the drawing scale for images
                       and fonts to Parameter:0

  5,67   Set Flip Bits Set Flip Bits. Sets the flip bits for drawing
                       images. Bit 0 set causes a horizontal flip, bit 1
                       set causes a vertical flip.

  ------------------------------------------------------------------------

## Sprites

  -----------------------------------------------------------------------

  G,F   Function      Description and Example

  ----- ------------- ---------------------------------------------------

  6,1   Sprite Reset  Reset the sprite system.

  6,2   Sprite Set    Set or update the sprite specified in Parameter:0.
                      The parameters are : Sprite Number, X Low, X High,
                      Y Low, Y High, Image, Flip and Anchor and Flags.
                      Bit 0 of flags specifies 32 bit sprites. Values
                      that are \$80 or \$8080 are not updated.

  6,3   Sprite Hide.  Hide the sprite specified in Parameter:0.

  6,4   Sprite        Parameter:0 is non-zero if the distance is less
        Collision.    than or equal to Parameter:2 between the center of
                      the sprite with index specified in Parameter:0 and
                      the center of the sprite with index specified in
                      Parameter:1.

  6,5   Sprite        Deposit into parameters:1..4, the screen
        Position      coordinates of the sprite with the index specified
                      in Parameter:0.

  -----------------------------------------------------------------------

## Controller

  -----------------------------------------------------------------------

  G,F   Function      Description and Example

  ----- ------------- ---------------------------------------------------

  7,1   Read Default  This reads the status of the base controller into
        Controller    Parameter:0, and is a compatibility API call. The
                      base controller is the keyboard keys (these are
                      WASD+OPKL or Arrow Keys+ZXCV) or the gamepad
                      controller buttons. Either works. The 8 bits of the
                      returned byte are the following buttons, most
                      significant first : Y X B A Down Up Right Left

  7,2   Read          This returns the number of game controllers plugged
        Controller    in to the USB System into Parameter:0. This does
        Count         not include the keyboard based controller, only
                      physical controller hardware.

  7,3   Read          This returns a specific controller status.
        Controller    Controller 0 is the keyboard controller,
                      Controllers 1 upwards are those physical USB
                      devices.

  -----------------------------------------------------------------------

## Sound

  -----------------------------------------------------------------------

  G,F   Function      Description and Example

  ----- ------------- ---------------------------------------------------

  8,1   Reset Sound   Reset the sound system. This empties all channel
                      queues and silences all channels immediately.

  8,2   Reset Channel Reset the sound channel specified in Parameter:0.

  8,3   Beep          Play the startup beep immediately.

  8,4   Queue Sound   Queue a sound. Refer to Section #\\ref{sound}
                      \"Sound\" for details. The parameters are :
                      Channel, Frequency Low, Frequency High, Duration
                      Low, Duration High, Slide Low, Slide High and
                      Source.

  8,5   Play Sound    Play the sound effect specified in Parameter:1 on
                      the channel specified in Parameter:0 immediately,
                      clearing the channel queue.

  8,6   Sound Status  Deposit in Parameter:0 the number of notes
                      outstanding before silence in the queue of the
                      channel specified in Parameter:0, including the
                      current playing sound, if any.

  8,7   Queue Sound   Queue a sound. Refer to Section #\\ref{sound}
        Extended      \"Sound\" for details. This is an extension of call
                      4 to support different waveform types and volumes.
                      The source parameter is no longer used. The
                      parameters are : Channel, Frequency Low, Frequency
                      High, Duration Low, Duration High, Slide Low, Slide
                      High, Sound Type and Sound Volume. All these are 16
                      bit parameters except the sound type and volume,
                      and the channel number.

  8,8   Get Channel   This returns the number of channels in Parameter #0
        Count         

  -----------------------------------------------------------------------

## Turtle Graphics

+----+------------+---------------------------------------------------+
| G  | Function   | Description and Example                           |
| ,F |            |                                                   |
+====+============+===================================================+
| 9  | Turtle     | Initialize the turtle graphics system.            |
| ,1 | Initialize | Parameter:0 is the sprite number to use for the   |
|    |            | turtle, as the turtle graphics system "adopts"    |
|    |            | one of the sprites. The icon is not currently     |
|    |            | re-definable, and initially the turtle is hidden. |
+----+------------+---------------------------------------------------+
| 9  | Turtle     | Turn the turtle right by Parameter:0,1 degrees.   |
| ,2 | Turn       | Show if hidden. To turn left, turn by a negative  |
|    |            | amount.\                                          |
|    |            | \                                                 |
|    |            | API_TURTLE_LEFT = #\$010E; API parameter (turn    |
|    |            | -90 degrees)                                      |
|    |            |                                                   |
|    |            | API_TURTLE_RIGHT = #\$005A; API parameter (turn   |
|    |            | +90 degrees)                                      |
|    |            |                                                   |
|    |            | API_TURTLE_FLIP = #\$00B4; API parameter (turn    |
|    |            | 180 degrees)                                      |
+----+------------+---------------------------------------------------+
| 9  | Turtle     | Move the turtle forward by Parameter:0,1 degrees, |
| ,3 | Move       | drawing in colour Parameter:2 if Parameter:3 is   |
|    |            | non-zero.                                         |
+----+------------+---------------------------------------------------+
| 9  | Turtle     | Hide the turtle.                                  |
| ,4 | Hide       |                                                   |
+----+------------+---------------------------------------------------+
| 9  | Turtle     | Move the turtle to the home position (in the      |
| ,5 | Home       | center, pointing upward).                         |
+----+------------+---------------------------------------------------+
| 9  | Turtle     | Show the turtle.                                  |
| ,6 | Show       |                                                   |
+----+------------+---------------------------------------------------+

## UEXT port I/O

  -------------------------------------------------------------------------

  G,F     Function      Description and Example

  ------- ------------- ---------------------------------------------------

  10,1    UExt          Initialise the UExt I/O system. This resets the IO
          Initialize    system to its default state, where all UEXT pins
                        are I/O pins, inputs and enabled.

  10,2    Write GPIO    This copies the value Parameter:1 to the output
                        latch for UEXT pin Parameter:0. This will only
                        display on the output pin if it is enabled, and its
                        direction is set to \"Output\" direction.

  10,3    Read GPIO     If the pin is set to \"Input\" direction, reads the
                        level on pin on UEXT port Parameter:0. If it is set
                        to \"Output\" direction, reads the output latch for
                        pin on UEXT port Parameter:0. If the read is
                        successful, the result will be in Parameter:0.

  10,4    Set Port      Set the port direction for UEXT Port Parameter:0 to
          Direction     the value in Parameter:1. This can be \$01 (Input),
                        \$02 (Output), or \$03 (Analogue Input).

  10,5    Write I2C     Write to I2C Device Parameter:0, Register
                        Parameter:1, value Parameter:2. No error is flagged
                        if the device is not present.

  10,6    Read I2C      Read from I2C Device Parameter:0, Register
                        Parameter:1. If the read is successful, the result
                        will be in Parameter:0. If the device is not
                        present, this will flag an error. Use FUNCTION 10,2
                        first, to check for its presence.

  10,7    Read Analog   Read the analogue value on UEXT Pin Parameter:0.
                        This has to be set to analog type to work. Returns
                        a value from 0..4095 stored in parameters:0,1,
                        which represents an input value of 0 to 3.3 volts.

  10,8    I2C Status    Try to read from I2C Device Parameter:0. If
                        present, then Parameter:0 will contain a non-zero
                        value.

  10,9    Read I2C      Try to read a block of memory from I2C Device
          Block         Parameter:0 into memory at parameters:1,2, length
                        parameters:3,4.

  10,10   Write I2C     Try to write a block of memory to I2C Device
          Block         Parameter:0 from memory at parameters:1,2, length
                        parameters:3,4.

  10,11   Read SPI      Try to read a block of memory from SPI Device into
          Block         memory at parameters:1,2, length parameters:3,4.

  10,12   Write SPI     Try to write a block of memory to SPI Device from
          Block         memory at parameters:1,2, length parameters:3,4.

  -------------------------------------------------------------------------

**\
**

  -------------------------------------------------------------------------

  10,13   Read UART     Try to read a block of memory from UART into memory
          Block         at parameters:1,2, length parameters:3,4. This can
                        fail with a timeout.

  ------- ------------- ---------------------------------------------------

  10,14   Write UART    Try to write a block of memory to UART from memory
          Block         at parameters:1,2, length parameters:3,4.

  10,15   Set UART      Set the Baud Rate and Serial Protocol for the UART
          Speed and     interface. The baud rate is in parameters:0..3 and
          Protocol      the protocol number is Parameter:4. Currently only
                        8N1 is supported, this is protocol 0.

  10,16   Write byte to Write byte Parameter:0 to the UART
          UART          

  10,17   Read byte     Read a byte from the UART. It is returned in
          from UART     Parameter:0

  10,18   Check if Byte See if a byte is available in the UART input
          Available     buffer. If available Parameter:0 is non zero.

  -------------------------------------------------------------------------

## Mouse

  ------------------------------------------------------------------------

  G,F    Function      Description and Example

  ------ ------------- ---------------------------------------------------

  11,1   Move display  Positions the display cursor at
         cursor        parameters:0,1,parameters:2,3

  11,2   Set mouse     Shows or hides the mouse cursor depending on the
         display       Parameter:0
         cursor        
         on/off.       

  11,3   Get mouse     Returns the mouse position (screen pixel, unsigned)
         state         in x parameters:0,1 and y parameters:2,3, button
                       state in Parameter:4 (button 1 is 0x1, button 2 0x2
                       etc., set when pressed), scroll wheel state in
                       Parameter:5 as uint8 which changes according to
                       scrolls.

  11,4   Test mouse    Returns non zero if a mouse is plugged in in
         present       Parameter:0

  11,5   Select mouse  Select a mouse cursor in Parameter:0 ; returns
         Cursor        error status if the cursor is not available.

  ------------------------------------------------------------------------

## Blitter

+----+------------+---------------------------------------------------+
| G  | Function   | Description and Example                           |
| ,F |            |                                                   |
+====+============+===================================================+
| 12 | Blitter    | Returns a non zero value in Parameter:0 if the    |
| ,1 | Busy       | blitter/DMA system is currently transferring      |
|    |            | data, used to check availability and transfer     |
|    |            | completion.                                       |
+----+------------+---------------------------------------------------+
| 12 | Simple     | Copy parameters:6,7 bytes of internal memory from |
| ,2 | Blit Copy  | Parameter:0:parameters:1,2 to                     |
|    |            | Parameter:3:parameters:4,5.                       |
|    |            |                                                   |
|    |            | Sets error flag if the transfer is not possible   |
|    |            | (e.g. illegal write addresses).                   |
|    |            |                                                   |
|    |            | The upper 8 bits of the address are:\             |
|    |            | 6502 RAM (00)\                                    |
|    |            | Video RAM (80,81)\                                |
|    |            | Graphics RAM (90)                                 |
+----+------------+---------------------------------------------------+
| 12 | Complex    | Copy a source rectangular area to a destination   |
| ,3 | Blit Copy  | rectangular area. It\'s oriented toward copying   |
|    |            | graphics data, but can be used as a more          |
|    |            | general-purpose memory mover. The source and      |
|    |            | target areas may be different formats, and the    |
|    |            | copy will convert the data on the fly. For        |
|    |            | example, you can expand 4bpp source graphics (two |
|    |            | pixels per byte) into the 1 pixel per byte        |
|    |            | framebuffer. However, the blitting is             |
|    |            | byte-oriented. So the source width is always      |
|    |            | rounded down to the nearest full byte.            |
|    |            |                                                   |
|    |            | Parameter (0) is the blit action:                 |
|    |            |                                                   |
|    |            |   ----------------------------------------------  |
|    |            |    0  Copy                                        |
|    |            |   --- ------------------------------------------  |
|    |            |    1  Copy masked - copy, but only where src is   |
|    |            |       not the transparent value.                  |
|    |            |                                                   |
|    |            |    2  Solid masked - set target to constant       |
|    |            |       solid value, but only where src is not the  |
|    |            |       transparent value.                          |
|    |            |   ----------------------------------------------  |
|    |            |                                                   |
|    |            | *See below for transparent/solid values.*         |
|    |            |                                                   |
|    |            | parameters (1,2) address of the source rectangle  |
|    |            | data.                                             |
|    |            |                                                   |
|    |            | parameters (3,4) address of the target rectangle  |
|    |            | data.                                             |
|    |            |                                                   |
|    |            | The source and target rectangle data is laid out  |
|    |            | in memory as follows:                             |
|    |            |                                                   |
|    |            |   ----------------------------------------------  |
|    |            |     0-2   24 bit address to copy from/to          |
|    |            |           (address is address:page:0)             |
|    |            |   ------- --------------------------------------  |
|    |            |      3    pad byte (must be zero)                 |
|    |            |                                                   |
|    |            |     4-5   Stride, in bytes. This is the value to  |
|    |            |           add to the address to get from one      |
|    |            |           line to the next                        |
|    |            |   ----------------------------------------------  |
|    |            |                                                   |
|    |            | Used for both source and target.                  |
|    |            |                                                   |
|    |            | For example:                                      |
|    |            |                                                   |
|    |            | -   if blitting to the screen, a stride of screen |
|    |            |     width (320) would get to the next line.       |
|    |            |                                                   |
|    |            | -   A zero source stride would repeat a single    |
|    |            |     line for the whole copy.                      |
|    |            |                                                   |
|    |            | -   A negative target stride would draw from the  |
|    |            |     bottom upward.                                |
|    |            |                                                   |
|    |            | **6 data format**                                 |
|    |            |                                                   |
|    |            |   ----------------------------------------------  |
|    |            |      0    bytes. Supported for both source and    |
|    |            |           target.                                 |
|    |            |   ------- --------------------------------------  |
|    |            |      1    pairs of 4-bit values (nibbles).        |
|    |            |           Source only.                            |
|    |            |                                                   |
|    |            |      2    8 single-bit values. Source only.       |
|    |            |                                                   |
|    |            |      3    high nibble. Target only.               |
|    |            |                                                   |
|    |            |      4    low nibble. Target only.                |
|    |            |                                                   |
|    |            |      7    A constant to use as the                |
|    |            |           \"transparent\" value for BLTACT_MASK   |
|    |            |           and BLTACT_SOLID. Source only. Not      |
|    |            |           used in target.                         |
|    |            |                                                   |
|    |            |      8    A constant to use as the \"solid\"      |
|    |            |           value for BLTACT_SOLID. Source only.    |
|    |            |           Not used in target.                     |
|    |            |                                                   |
|    |            |      9    Height. The number of lines to copy.    |
|    |            |           Source only. Not used in target. The    |
|    |            |           copy is driven by the source height.    |
|    |            |                                                   |
|    |            |   10 - 11 Width. The number of values to copy     |
|    |            |           for each line. Source only. Not used    |
|    |            |           in target. The copy is driven by the    |
|    |            |           source width.                           |
|    |            |   ----------------------------------------------  |
+----+------------+---------------------------------------------------+
| 12 | Blit Image | Blits an image from memory onto the screen. The   |
| ,4 |            | image will be clipped, so it\'s safe to blit      |
|    |            | partly (or fully) offscreen-images.               |
|    |            |                                                   |
|    |            |   ---                                             |
|    |            | ------------------------------------------------- |
|    |            |    **Parameter**  **Description**                 |
|    |            |   ---                                             |
|    |            | ------------ ------------------------------------ |
|    |            |                                                   |
|    |            |         0        The blit action (see function 3, |
|    |            |                   Complex Blit)                   |
|    |            |                                                   |
|    |            |        1, 2       Address of the source rectangle |
|    |            |                   data.                           |
|    |            |                                                   |
|    |            |                                                   |
|    |            |   3, 4       X pixel coordinate on screen (signed |
|    |            |                   16 bit)                         |
|    |            |                                                   |
|    |            |                                                   |
|    |            |   5, 6       Y pixel coordinate on screen (signed |
|    |            |                   16 bit)                         |
|    |            |                                                   |
|    |            |                                                   |
|    |            |       7        Destination format, determines how |
|    |            |                   framebuffer will be written:    |
|    |            |   ---                                             |
|    |            | ------------------------------------------------- |
|    |            |                                                   |
|    |            |   ----------------------------------------------  |
|    |            |      0    write to whole byte.                    |
|    |            |   ------- --------------------------------------  |
|    |            |      1    unsupported                             |
|    |            |                                                   |
|    |            |      2    unsupported                             |
|    |            |                                                   |
|    |            |      3    write to high nibble only.              |
|    |            |                                                   |
|    |            |      4    write to low nibble only.               |
|    |            |   ----------------------------------------------  |
|    |            |                                                   |
|    |            | **NOTE:** Clipping operates at byte resolution on |
|    |            | the source data. So, for example, if you blit a   |
|    |            | 1-bit image (format 2) to an x-position of -2,    |
|    |            | then the whole first byte will be skipped leaving |
|    |            | 6 empty pixels on the left. Same happens on the   |
|    |            | right - either the whole source byte is used, or  |
|    |            | it\'ll be skipped.                                |
+----+------------+---------------------------------------------------+

## Editor

  ------------------------------------------------------------------------

  G,F    Function      Description and Example

  ------ ------------- ---------------------------------------------------

  13,1   Initialize    Initializes the editor
         Editor        

  13,2   Reenter the   Re-enters the system editor. Returns the function
         Editor        required for call out, the editors sort of \'call
                       backs\' - see editor specification.

  ------------------------------------------------------------------------

; Convenience macros for Neo6502 applications programming

; SPDX-License-Identifier: CC0-1.0

;\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--;

; Neo6502 Kernel jump vectors (see kernel/kernel.asm) ;

;\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--;

ReadLine = \$FFEB

ReadCharacter = \$FFEE

WriteCharacter = \$FFF1

WaitMessage = \$FFF4

SendMessage = \$FFF7

;\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--;

; Neo6502 Kernel API control addresses ;

;\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--;

ControlPort = \$FF00

API_COMMAND = ControlPort + 0 ; function group address

API_FUNCTION = ControlPort + 1 ; function address

API_ERROR = ControlPort + 2 ; function error codes

API_STATUS = ControlPort + 3 ; misc hardware status codes (bit-field)

API_PARAMETERS = ControlPort + 4 ; function parameters base address
(+0-7)

;\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--;

; Neo6502 Kernel API control codes (see api.pdf) ;

;\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--;

; Status Information

API_ERROR_NONE = #\$00 ; error code

API_STATUS_ESC = #\$07 ; flag

; System functions (Group 1)

API_GROUP_SYSTEM = #\$01 ; API function group

API_FN_TIMER = #\$01 ; API function

API_FN_KEY_STATUS = #\$02 ; API function

API_FN_BASIC = #\$03 ; API function

API_FN_CREDITS = #\$04 ; API function

API_FN_SERIAL_STATUS = #\$05 ; API function

API_FN_LOCALE = #\$06 ; API function

API_FN_RESET = #\$07 ; API function

; Console functions (Group 2)

API_GROUP_CONSOLE = #\$02 ; API function group

API_FN_READ_CHAR = #\$01 ; API function

API_FN_CONSOLE_STATUS = #\$02 ; API function

API_FN_READ_LINE = #\$03 ; API function

API_FN_DEFINE_HOTKEY = #\$04 ; API function

API_FN_DEFINE_CHAR = #\$05 ; API function

API_FN_WRITE_CHAR = #\$06 ; API function

API_FN_SET_CURSOR_POS = #\$07 ; API function

API_FN_LIST_HOTKEYS = #\$08 ; API function

API_FN_SCREEN_SIZE = #\$09 ; API function

API_FN_INSERT_LINE = #\$0A ; API function

API_FN_DELETE_LINE = #\$0B ; API function

API_FN_CLEAR_SCREEN = #\$0C ; API function

API_FN_CURSOR_POS = #\$0D ; API function

API_FN_CLEAR_REGIION = #\$0E ; API function

API_FN_SET_TEXT_COLOR = #\$0F ; API function

API_FN_CURSOR_INVERSE = #\$10 ; API function

; Console results (Group 2 Function 2)

API_QUEUE_EMPTY = #\$FF ; API result (status code)

; File I/O functions (Group 3)

API_GROUP_FILEIO = #\$03 ; API function group

API_FN_LIST_DIRECTORY = #\$01 ; API function

API_FN_LOAD_FILENAME = #\$02 ; API function

API_FN_STORE_FILENAME = #\$03 ; API function

API_FN_FILE_OPEN = #\$04 ; API function

API_FN_FILE_CLOSE = #\$05 ; API function

API_FN_FILE_SEEK = #\$06 ; API function

API_FN_FILE_TELL = #\$07 ; API function

API_FN_FILE_READ = #\$08 ; API function

API_FN_FILE_WRITE = #\$09 ; API function

API_FN_FILE_SIZE = #\$0A ; API function

API_FN_FILE_SET_SIZE = #\$0B ; API function

API_FN_FILE_RENAME = #\$0C ; API function

API_FN_FILE_DELETE = #\$0D ; API function

API_FN_DIR_CHDIR = #\$0E ; API function

API_FN_DIR_MKDIR = #\$0F ; API function

API_FN_FILE_STAT = #\$10 ; API function

API_FN_DIR_OPEN = #\$11 ; API function

API_FN_DIR_READ = #\$12 ; API function

API_FN_DIR_CLOSE = #\$13 ; API function

API_FN_FILE_COPY = #\$14 ; API function

API_FN_LIST_FILTERED = #\$20 ; API function

; File I/O parameters (Group 3 Function 2)

API_FILE_TO_SCREEN = #\$FFFF ; API parameter

; Mathematics functions (Group 4)

API_GROUP_MATH = #\$04 ; API function group

API_FN_ADD = #\$00 ; API function

API_FN_SUB = #\$01 ; API function

API_FN_MUL = #\$02 ; API function

API_FN_DIV_DEC = #\$03 ; API function

API_FN_DIV_INT = #\$04 ; API function

API_FN_MOD = #\$05 ; API function

API_FN_COMP = #\$06 ; API function

API_FN_NEG = #\$10 ; API function

API_FN_FLOOR = #\$11 ; API function

API_FN_SQRT = #\$12 ; API function

API_FN_SINE = #\$13 ; API function

API_FN_COS = #\$14 ; API function

API_FN_TAN = #\$15 ; API function

API_FN_ATAN = #\$16 ; API function

API_FN_EXP = #\$17 ; API function

API_FN_LOG = #\$18 ; API function

API_FN_ABS = #\$19 ; API function

API_FN_SIGN = #\$1A ; API function

API_FN_RND_DEC = #\$1B ; API function

API_FN_RND_INT = #\$1C ; API function

API_FN_INT_TO_DEC = #\$20 ; API function

API_FN_STR_TO_NUM = #\$21 ; API function

API_FN_NUM_TO_STR = #\$22 ; API function

; Graphics functions (Group 5)

API_GROUP_GRAPHICS = #\$05 ; API function group

API_FN_SET_GFX = #\$01 ; API function

API_FN_DRAW_LINE = #\$02 ; API function

API_FN_DRAW_RECT = #\$03 ; API function

API_FN_DRAW_ELLIPSE = #\$04 ; API function

API_FN_DRAW_PIXEL = #\$05 ; API function

API_FN_DRAW_TEXT = #\$06 ; API function

API_FN_DRAW_IMG = #\$07 ; API function

API_FN_DRAW_TILEMAP = #\$08 ; API function

API_FN_SET_PALETTE = #\$20 ; API function

API_FN_READ_PIXEL = #\$21 ; API function

API_FN_RESET_PALETTE = #\$22 ; API function

API_FN_SET_TILEMAP = #\$23 ; API function

API_FN_READ_SPRITE_PXL = #\$24 ; API function

API_FN_FRAME_COUNT = #\$25 ; API function

API_FN_SET_COLOR = #\$40 ; API function

API_FN_SET_SOLID = #\$41 ; API function

API_FN_SET_DRAW_SIZE = #\$42 ; API function

API_FN_SET_FLIP = #\$43 ; API function

; Graphics parameters (Group 5, Function 1 - Group 6, Function 2)

API_FLIP_HORZ = #\$00 ; API parameter (flag)

API_FLIP_VERT = #\$01 ; API parameter (flag)

; Graphics results (Group 5, Functions 33,36)

API_PIXEL_TRANSPARENT = #\$00 ; API result (flag)

; Sprites functions (Group 6)

API_GROUP_SPRITES = #\$06 ; API function group

API_FN_SPRITE_RESET = #\$01 ; API function

API_FN_SPRITE_SET = #\$02 ; API function

API_FN_SPRITE_HIDE = #\$03 ; API function

API_FN_SPRITE_COLLISION = #\$04 ; API function

API_FN_SPRITE_POS = #\$05 ; API function

; Sprites parameters (Group 6, Function 2)

API_SPRITE_TURTLE = #\$00 ; API parameter (sprite index)

API_SPRITE_32BIT = #\$40 ; API parameter (bit-mask)

API_SPRITE_CLEAR = #\$80 ; API parameter (bit-mask)

API_ANCHOR_BL = #\$01 ; API parameter (anchor position)

API_ANCHOR_B = #\$02 ; API parameter (anchor position)

API_ANCHOR_BR = #\$03 ; API parameter (anchor position)

API_ANCHOR_L = #\$04 ; API parameter (anchor position)

API_ANCHOR_C = #\$05 ; API parameter (anchor position)

API_ANCHOR_R = #\$06 ; API parameter (anchor position)

API_ANCHOR_TL = #\$07 ; API parameter (anchor position)

API_ANCHOR_T = #\$08 ; API parameter (anchor position)

API_ANCHOR_TR = #\$09 ; API parameter (anchor position)

; Sprites results (Group 6, Function 4)

API_COLLISION_NONE = #\$00 ; API result (flag)

; Controller functions (Group 7)

API_GROUP_CONTROLLER = #\$07 ; API function group

API_FN_READ_CONTROLLER = #\$01 ; API function

; Controller results (Group 7, Function 1)

API_CONTROLLER_LEFT = #\$01 ; API result (status bit-mask)

API_CONTROLLER_RIGHT = #\$02 ; API result (status bit-mask)

API_CONTROLLER_UP = #\$04 ; API result (status bit-mask)

API_CONTROLLER_DOWN = #\$08 ; API result (status bit-mask)

API_CONTROLLER_BTNA = #\$10 ; API result (status bit-mask)

API_CONTROLLER_BTNB = #\$20 ; API result (status bit-mask)

; Sound functions (Group 8)

API_GROUP_SOUND = #\$08 ; API function group

API_FN_RESET_SOUND = #\$01 ; API function

API_FN_RESET_CHANNEL = #\$02 ; API function

API_FN_BEEP = #\$03 ; API function

API_FN_QUEUE_SOUND = #\$04 ; API function

API_FN_PLAY_SOUND = #\$05 ; API function

API_FN_SOUND_STATUS = #\$06 ; API function

; Sound parameters (Group 8, Functions 2,4,5)

API_SOUND_CH_00 = #\$00 ; API parameter (channel index)

; Sound parameters (Group 8, Function 4)

API_NOTE_REST = #\$0000 ; API parameter (musical rest)

API_NOTE_C0 = #\$0010 ; API parameter (musical note)

API_NOTE_Cs0 = #\$0011 ; API parameter (musical note)

API_NOTE_Df0 = #\$0011 ; API parameter (musical note)

API_NOTE_D0 = #\$0012 ; API parameter (musical note)

API_NOTE_Ds0 = #\$0013 ; API parameter (musical note)

API_NOTE_Ef0 = #\$0013 ; API parameter (musical note)

API_NOTE_E0 = #\$0015 ; API parameter (musical note)

API_NOTE_F0 = #\$0016 ; API parameter (musical note)

API_NOTE_Fs0 = #\$0017 ; API parameter (musical note)

API_NOTE_Gf0 = #\$0017 ; API parameter (musical note)

API_NOTE_G0 = #\$0018 ; API parameter (musical note)

API_NOTE_Af0 = #\$001A ; API parameter (musical note)

API_NOTE_Gs0 = #\$001A ; API parameter (musical note)

API_NOTE_A0 = #\$001C ; API parameter (musical note)

API_NOTE_As0 = #\$001D ; API parameter (musical note)

API_NOTE_Bf0 = #\$001D ; API parameter (musical note)

API_NOTE_B0 = #\$001F ; API parameter (musical note)

API_NOTE_C1 = #\$0021 ; API parameter (musical note)

API_NOTE_Cs1 = #\$0023 ; API parameter (musical note)

API_NOTE_Df1 = #\$0023 ; API parameter (musical note)

API_NOTE_D1 = #\$0025 ; API parameter (musical note)

API_NOTE_Ds1 = #\$0027 ; API parameter (musical note)

API_NOTE_Ef1 = #\$0027 ; API parameter (musical note)

API_NOTE_E1 = #\$0029 ; API parameter (musical note)

API_NOTE_F1 = #\$002C ; API parameter (musical note)

API_NOTE_Fs1 = #\$002E ; API parameter (musical note)

API_NOTE_Gf1 = #\$002E ; API parameter (musical note)

API_NOTE_G1 = #\$0031 ; API parameter (musical note)

API_NOTE_Af1 = #\$0034 ; API parameter (musical note)

API_NOTE_Gs1 = #\$0034 ; API parameter (musical note)

API_NOTE_A1 = #\$0037 ; API parameter (musical note)

API_NOTE_As1 = #\$003A ; API parameter (musical note)

API_NOTE_Bf1 = #\$003A ; API parameter (musical note)

API_NOTE_B1 = #\$003E ; API parameter (musical note)

API_NOTE_C2 = #\$0041 ; API parameter (musical note)

API_NOTE_Cs2 = #\$0045 ; API parameter (musical note)

API_NOTE_Df2 = #\$0045 ; API parameter (musical note)

API_NOTE_D2 = #\$0049 ; API parameter (musical note)

API_NOTE_Ds2 = #\$004E ; API parameter (musical note)

API_NOTE_Ef2 = #\$004E ; API parameter (musical note)

API_NOTE_E2 = #\$0052 ; API parameter (musical note)

API_NOTE_F2 = #\$0057 ; API parameter (musical note)

API_NOTE_Fs2 = #\$005C ; API parameter (musical note)

API_NOTE_Gf2 = #\$005C ; API parameter (musical note)

API_NOTE_G2 = #\$0062 ; API parameter (musical note)

API_NOTE_Af2 = #\$0068 ; API parameter (musical note)

API_NOTE_Gs2 = #\$0068 ; API parameter (musical note)

API_NOTE_A2 = #\$006E ; API parameter (musical note)

API_NOTE_As2 = #\$0075 ; API parameter (musical note)

API_NOTE_Bf2 = #\$0075 ; API parameter (musical note)

API_NOTE_B2 = #\$007B ; API parameter (musical note)

API_NOTE_C3 = #\$0083 ; API parameter (musical note)

API_NOTE_Cs3 = #\$008B ; API parameter (musical note)

API_NOTE_Df3 = #\$008B ; API parameter (musical note)

API_NOTE_D3 = #\$0093 ; API parameter (musical note)

API_NOTE_Ds3 = #\$009C ; API parameter (musical note)

API_NOTE_Ef3 = #\$009C ; API parameter (musical note)

API_NOTE_E3 = #\$00A5 ; API parameter (musical note)

API_NOTE_F3 = #\$00AF ; API parameter (musical note)

API_NOTE_Fs3 = #\$00B9 ; API parameter (musical note)

API_NOTE_Gf3 = #\$00B9 ; API parameter (musical note)

API_NOTE_G3 = #\$00C4 ; API parameter (musical note)

API_NOTE_Af3 = #\$00D0 ; API parameter (musical note)

API_NOTE_Gs3 = #\$00D0 ; API parameter (musical note)

API_NOTE_A3 = #\$00DC ; API parameter (musical note)

API_NOTE_As3 = #\$00E9 ; API parameter (musical note)

API_NOTE_Bf3 = #\$00E9 ; API parameter (musical note)

API_NOTE_B3 = #\$00F7 ; API parameter (musical note)

API_NOTE_C4 = #\$0106 ; API parameter (musical note)

API_NOTE_Cs4 = #\$0115 ; API parameter (musical note)

API_NOTE_Df4 = #\$0115 ; API parameter (musical note)

API_NOTE_D4 = #\$0126 ; API parameter (musical note)

API_NOTE_Ds4 = #\$0137 ; API parameter (musical note)

API_NOTE_Ef4 = #\$0137 ; API parameter (musical note)

API_NOTE_E4 = #\$014A ; API parameter (musical note)

API_NOTE_F4 = #\$015D ; API parameter (musical note)

API_NOTE_Fs4 = #\$0172 ; API parameter (musical note)

API_NOTE_Gf4 = #\$0172 ; API parameter (musical note)

API_NOTE_G4 = #\$0188 ; API parameter (musical note)

API_NOTE_Af4 = #\$019F ; API parameter (musical note)

API_NOTE_Gs4 = #\$019F ; API parameter (musical note)

API_NOTE_A4 = #\$01B8 ; API parameter (musical note)

API_NOTE_As4 = #\$01D2 ; API parameter (musical note)

API_NOTE_Bf4 = #\$01D2 ; API parameter (musical note)

API_NOTE_B4 = #\$01EE ; API parameter (musical note)

API_NOTE_C5 = #\$020B ; API parameter (musical note)

API_NOTE_Cs5 = #\$022A ; API parameter (musical note)

API_NOTE_Df5 = #\$022A ; API parameter (musical note)

API_NOTE_D5 = #\$024B ; API parameter (musical note)

API_NOTE_Ds5 = #\$026E ; API parameter (musical note)

API_NOTE_Ef5 = #\$026E ; API parameter (musical note)

API_NOTE_E5 = #\$0293 ; API parameter (musical note)

API_NOTE_F5 = #\$02BA ; API parameter (musical note)

API_NOTE_Fs5 = #\$02E4 ; API parameter (musical note)

API_NOTE_Gf5 = #\$02E4 ; API parameter (musical note)

API_NOTE_G5 = #\$0310 ; API parameter (musical note)

API_NOTE_Af5 = #\$033F ; API parameter (musical note)

API_NOTE_Gs5 = #\$033F ; API parameter (musical note)

API_NOTE_A5 = #\$0370 ; API parameter (musical note)

API_NOTE_As5 = #\$03A4 ; API parameter (musical note)

API_NOTE_Bf5 = #\$03A4 ; API parameter (musical note)

API_NOTE_B5 = #\$03DC ; API parameter (musical note)

API_NOTE_C6 = #\$0417 ; API parameter (musical note)

API_NOTE_Cs6 = #\$0455 ; API parameter (musical note)

API_NOTE_Df6 = #\$0455 ; API parameter (musical note)

API_NOTE_D6 = #\$0497 ; API parameter (musical note)

API_NOTE_Ds6 = #\$04DD ; API parameter (musical note)

API_NOTE_Ef6 = #\$04DD ; API parameter (musical note)

API_NOTE_E6 = #\$0527 ; API parameter (musical note)

API_NOTE_F6 = #\$0575 ; API parameter (musical note)

API_NOTE_Fs6 = #\$05C8 ; API parameter (musical note)

API_NOTE_Gf6 = #\$05C8 ; API parameter (musical note)

API_NOTE_G6 = #\$0620 ; API parameter (musical note)

API_NOTE_Af6 = #\$067D ; API parameter (musical note)

API_NOTE_Gs6 = #\$067D ; API parameter (musical note)

API_NOTE_A6 = #\$06E0 ; API parameter (musical note)

API_NOTE_As6 = #\$0749 ; API parameter (musical note)

API_NOTE_Bf6 = #\$0749 ; API parameter (musical note)

API_NOTE_B6 = #\$07B8 ; API parameter (musical note)

API_NOTE_C7 = #\$082D ; API parameter (musical note)

API_NOTE_Cs7 = #\$08A9 ; API parameter (musical note)

API_NOTE_Df7 = #\$08A9 ; API parameter (musical note)

API_NOTE_D7 = #\$092D ; API parameter (musical note)

API_NOTE_Ds7 = #\$09B9 ; API parameter (musical note)

API_NOTE_Ef7 = #\$09B9 ; API parameter (musical note)

API_NOTE_E7 = #\$0A4D ; API parameter (musical note)

API_NOTE_F7 = #\$0AEA ; API parameter (musical note)

API_NOTE_Fs7 = #\$0B90 ; API parameter (musical note)

API_NOTE_Gf7 = #\$0B90 ; API parameter (musical note)

API_NOTE_G7 = #\$0C40 ; API parameter (musical note)

API_NOTE_Af7 = #\$0CFA ; API parameter (musical note)

API_NOTE_Gs7 = #\$0CFA ; API parameter (musical note)

API_NOTE_A7 = #\$0DC0 ; API parameter (musical note)

API_NOTE_As7 = #\$0E91 ; API parameter (musical note)

API_NOTE_Bf7 = #\$0E91 ; API parameter (musical note)

API_NOTE_B7 = #\$0F6F ; API parameter (musical note)

API_NOTE_C8 = #\$105A ; API parameter (musical note)

API_NOTE_Cs8 = #\$1153 ; API parameter (musical note)

API_NOTE_Df8 = #\$1153 ; API parameter (musical note)

API_NOTE_D8 = #\$125B ; API parameter (musical note)

API_NOTE_Ds8 = #\$1372 ; API parameter (musical note)

API_NOTE_Ef8 = #\$1372 ; API parameter (musical note)

API_NOTE_E8 = #\$149A ; API parameter (musical note)

API_NOTE_F8 = #\$15D4 ; API parameter (musical note)

API_NOTE_Fs8 = #\$1720 ; API parameter (musical note)

API_NOTE_Gf8 = #\$1720 ; API parameter (musical note)

API_NOTE_G8 = #\$1880 ; API parameter (musical note)

API_NOTE_Af8 = #\$19F5 ; API parameter (musical note)

API_NOTE_Gs8 = #\$19F5 ; API parameter (musical note)

API_NOTE_A8 = #\$1B80 ; API parameter (musical note)

API_NOTE_As8 = #\$1D23 ; API parameter (musical note)

API_NOTE_Bf8 = #\$1D23 ; API parameter (musical note)

API_NOTE_B8 = #\$1EDE ; API parameter (musical note)

API_NOTE_C9 = #\$20B4 ; API parameter (musical note)

API_NOTE_Cs9 = #\$22A6 ; API parameter (musical note)

API_NOTE_Df9 = #\$22A6 ; API parameter (musical note)

API_NOTE_D9 = #\$24B5 ; API parameter (musical note)

API_NOTE_Ds9 = #\$26E4 ; API parameter (musical note)

API_NOTE_Ef9 = #\$26E4 ; API parameter (musical note)

API_NOTE_E9 = #\$2934 ; API parameter (musical note)

API_NOTE_F9 = #\$2BA7 ; API parameter (musical note)

API_NOTE_Fs9 = #\$2E40 ; API parameter (musical note)

API_NOTE_Gf9 = #\$2E40 ; API parameter (musical note)

API_NOTE_G9 = #\$3100 ; API parameter (musical note)

API_NOTE_Af9 = #\$33EA ; API parameter (musical note)

API_NOTE_Gs9 = #\$33EA ; API parameter (musical note)

API_NOTE_A9 = #\$3700 ; API parameter (musical note)

API_NOTE_As9 = #\$3A45 ; API parameter (musical note)

API_NOTE_Bf9 = #\$3A45 ; API parameter (musical note)

API_NOTE_B9 = #\$3DBC ; API parameter (musical note)

API_NOTE_C10 = #\$4168 ; API parameter (musical note)

API_NOTE_Cs10 = #\$454C ; API parameter (musical note)

API_NOTE_Df10 = #\$454C ; API parameter (musical note)

API_NOTE_D10 = #\$496B ; API parameter (musical note)

API_NOTE_Ds10 = #\$4DC8 ; API parameter (musical note)

API_NOTE_Ef10 = #\$4DC8 ; API parameter (musical note)

API_TEMPO_60 = #\$0064 ; API parameter (musical note duration, 60BPM)

API_TEMPO_80 = #\$004B ; API parameter (musical note duration, 80BPM)

API_TEMPO_90 = #\$0042 ; API parameter (musical note duration, 90BPM)

API_TEMPO_120 = #\$0032 ; API parameter (musical note duration, 120BPM)

API_SLIDE_NONE = #\$0000 ; API parameter (slide value)

API_SLIDE_SLOW = #\$0004 ; API parameter (slide range)

API_SLIDE_MED = #\$0008 ; API parameter (slide range)

API_SLIDE_FAST = #\$0010 ; API parameter (slide range)

API_SOUND_SRC_BEEP = #\$00 ; API parameter (sound generator)

; Sound parameters (Group 8, Function 5)

API_SFX_POSITIVE = #\$00 ; API parameter (sound effect)

API_SFX_NEGATIVE = #\$01 ; API parameter (sound effect)

API_SFX_ERROR = #\$02 ; API parameter (sound effect)

API_SFX_CONFIRM = #\$03 ; API parameter (sound effect)

API_SFX_REJECT = #\$04 ; API parameter (sound effect)

API_SFX_SWEEP = #\$05 ; API parameter (sound effect)

API_SFX_COIN = #\$06 ; API parameter (sound effect)

API_SFX_LASER_LONG = #\$07 ; API parameter (sound effect)

API_SFX_POWERUP = #\$08 ; API parameter (sound effect)

API_SFX_VICTORY = #\$09 ; API parameter (sound effect)

API_SFX_DEFEAT = #\$0A ; API parameter (sound effect)

API_SFX_FANFARE = #\$0B ; API parameter (sound effect)

API_SFX_ALARM1 = #\$0C ; API parameter (sound effect)

API_SFX_ALARM2 = #\$0D ; API parameter (sound effect)

API_SFX_ALARM3 = #\$0E ; API parameter (sound effect)

API_SFX_RING1 = #\$0F ; API parameter (sound effect)

API_SFX_RING2 = #\$10 ; API parameter (sound effect)

API_SFX_RING3 = #\$11 ; API parameter (sound effect)

API_SFX_DANGER = #\$12 ; API parameter (sound effect)

API_SFX_EXPL_LONG = #\$13 ; API parameter (sound effect)

API_SFX_EXPL_MEDIUM = #\$14 ; API parameter (sound effect)

API_SFX_EXPL_SHORT = #\$15 ; API parameter (sound effect)

API_SFX_LASER_MEDIUM = #\$16 ; API parameter (sound effect)

API_SFX_LASER_SHORT = #\$17 ; API parameter (sound effect)

; Turtle Graphics functions (Group 9)

API_GROUP_TURTLE = #\$09 ; API function group

API_FN_TURTLE_INIT = #\$01 ; API function

API_FN_TURTLE_TURN = #\$02 ; API function

API_FN_TURTLE_MOVE = #\$03 ; API function

API_FN_TURTLE_HIDE = #\$04 ; API function

API_FN_TURTLE_HOME = #\$05 ; API function

; Turtle Graphics parameters (Group 9, Function 2)

API_TURTLE_LEFT = #\$010E ; API parameter (turn -90 degrees)

API_TURTLE_RIGHT = #\$005A ; API parameter (turn +90 degrees)

API_TURTLE_FLIP = #\$00B4 ; API parameter (turn 180 degrees)

; Turtle Graphics parameters (Group 9, Function 3)

API_PEN_UP = #\$00 ; API parameter (turtle tracks on)

API_PEN_DOWN = #\$01 ; API parameter (turtle tracks off)

; UExt functions (Group 10)

API_GROUP_UEXT = #\$09 ; API function group

API_FN_UEXT_INIT = #\$01 ; API function

API_FN_GPIO_WRITE = #\$02 ; API function

API_FN_GPIO_READ = #\$03 ; API function

API_FN_SET_PORT_DIR = #\$04 ; API function

API_FN_I2C_WRITE = #\$05 ; API function

API_FN_I2C_READ = #\$06 ; API function

API_FN_ANALOG_READ = #\$07 ; API function

;\-\-\-\-\-\-\--;

; colors ;

;\-\-\-\-\-\-\--;

COLOR_BLACK = #\$80

COLOR_RED = #\$81

COLOR_GREEN = #\$82

COLOR_YELLOW = #\$83

COLOR_BLUE = #\$84

COLOR_MAGENTA = #\$85

COLOR_CYAN = #\$86

COLOR_WHITE = #\$87

COLOR_ALT_BLACK = #\$88

COLOR_DARK_GREY = #\$89

COLOR_DARK_GREEN = #\$8A

COLOR_ORANGE = #\$8B

COLOR_DARK_ORANGE = #\$8C

COLOR_BROWN = #\$8D

COLOR_PINK = #\$8E

COLOR_LIGHT_GREY = #\$8F

**\
**

# Pascal for the Neo6502

**\
**

**\
**

**\
**

# Appendix A

This appendix offers the detailed specifications for each of the Neo6502
computers and the differences between them.

  ------------------------------------------------------------------------

  Feature                                        Neo6502\     Neo6502pc
                                                 Computer      Computer

  -------------------------------------------- ------------ --------------

  Physical W65C02 processor running at 6.25Mhz     Yes           Yes

  RP2040 SoC (System on a Chip) w/ 2MB Flash       Yes           Yes

  10-pin UEXT connector                             1x            4x

  6502 bus connector                               Yes           Yes

  Audio mini speaker                               Yes           Yes

  Audio 3.5mm connector                            Yes           Yes

  USB-C power supply connector                     Yes           Yes

  DVI/HDMI connector                               Yes          No[^7]

  USB-A Port[^8]                                  1x[^9]          3x

  4-position configuration slide switch            Yes           Yes

  Boot Button                                      Yes           Yes

  One 14-pin external 12 GPIO connector             No           Yes

  Programming slide switch                          No           Yes

  USB-C Programming Port                            No           Yes

  Build-in LCD display (320x240)\                   No           Yes
  w/ touch panel acting like mouse                          

  Build in LiPo battery w/ charger circuit and      No           Yes
  battery monitoring                                        

Power on off switch                               No           Yes
------------------------------------------------------------------------

## ![A red circuit board with black and red components Description automatically generated](media/image20.png){width="2.4180555555555556in" height="2.2631944444444443in"}Neo6502

The Olimex Neo6502 was the first model released. The main components are
a W65C02 and a Raspberry Pi RP2040. The W65C02 runs the machine code (at
about 6.3Mhz), with the RP2040 providing RAM, Video and other aspects.

![A close-up of a circuit board Description automatically
generated](media/image21.png){width="2.7736111111111112in"
height="2.3965277777777776in"}Early adopters had a revision A board
(purple), and later the revision B board (red) was released. Both are
almost identical, with the exception that the revision A board required
a couple of wires connecting pins of the UEXT to the 6502 bus to work
properly. Both models come as the board only, with cases available to
protect the board.

### Hardware Pictures

![A red circuit board with white text Description automatically
generated](media/image22.png){width="6.225517279090114in"
height="3.879032152230971in"}

## Neo6502pc

The Olimex Neo6502pc is an open-source hardware and software standalone
modern retro computer with a real W65C02 processor, RP2040 co-processor,
USB host (with 3x USB-A ports), Lithium polymer (Lipo) battery with
charging circuit and battery monitoring.

The design goal with Neo6502pc was to create a simple retro-computer
with a real 6502 processor that has modern features (provided by the
Raspberry RP2040 co-processor) such as HDMI video output, USB ports that
support USB keyboards, flash drives for storage, and USB game pad. The
RP2040 co-processor emulates the RAM for the W6502 processor.

More Information:
<https://www.olimex.com/Products/Retro-Computers/Neo6502pc/open-source-hardware>

## Features

- A real W65C02 processor clocked at 6.25Mhz

- Graphics co-processor RP2040 providing 320 x 240 resolution with
  256-color display on HDMI/DVI.

- 32k Graphics RAM for tiles and sprites

- 128 sprites up to 32x32 pixels.

- Multiple tile maps (16x16 tiles, can be double sized)

- High speed drawing features

- Turtle Graphics

- Blitter for high-speed graphics

- Four UEXT interface ports to access a wide range of hardware add
  ons.

- 1 channel \"beeper\" sound with SFX library (to be replaced by
  AY-3-8910 Emulation)

- USB flash drive support for storage w/ optional SD Card support.

- Supports standard USB keyboard.

- Fast structured BASIC with hardware support and inline assembler.

- BASIC can be edited on screen or using a text editor.

- High Speed Integer/Floating point arithmetic

- Huge open-source community that has written documentation, provided
  samples, and code including games.

- Cross development support

- Accurate cross platform emulator for Windows/Mac/Linux, only
  requires SDL2

- Serial link to PC for Cross-Development

- Program in PASCAL using Mad Pascal compiler

- Program in \'C\' using CC65 and LLVM

- USB Mouse and Gamepad support

- BASIC support for Serial, I2C and SPI hardware via UEXT Connector -
  64KB linear RAM space for code

- LCD display

- Internal battery backup power supply which allows it to operate up
  to 3 hours without external power supply

- Three external and one internal USB hosts (internal is connected to
  LCD touch panel)

- Audio output

- 12 GPIO extension connector

- USB-C for power and internal battery charging.

- Second USB-C for RP2040 firmware programming

- Dimensions 220 x 130 x 35 mm

### Neo6502pc -- Hardware Pictures

![A blue rectangular device with a screen and buttons Description
automatically generated](media/image23.png){width="5.0in"
height="2.6834700349956258in"}![A blue rectangular object with red text
Description automatically generated](media/image24.png){width="5.0in"
height="2.956722440944882in"}![A blue rectangular object with red text
Description automatically generated](media/image25.png){width="5.0in"
height="2.8098436132983378in"}

**Neo6502pc -- Hardware Pictures** *(continued)*

  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

  ![A red rectangular device with a screen Description          ![A black rectangular device with a screen Description        ![A yellow rectangular device with a screen Description
  automatically                                                 automatically                                                 automatically
  generated](media/image26.jpeg){width="1.7247714348206473in"   generated](media/image27.jpeg){width="1.7335115923009623in"   generated](media/image28.jpeg){width="1.9541283902012248in"
  height="0.96248687664042in"}                                  height="0.9171467629046369in"}                                height="1.0817147856517935in"}

  ------------------------------------------------------------- ------------------------------------------------------------- -------------------------------------------------------------

  ![A green rectangular device with a screen Description        ![A blue rectangular device with a screen Description         
  automatically                                                 automatically                                                 
  generated](media/image29.jpeg){width="1.7972222222222223in"   generated](media/image30.jpeg){width="1.9082567804024497in"   
  height="0.9722222222222222in"}                                height="1.0171062992125983in"}                                

  -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

![](media/image31.png){width="6.433333333333334in"
height="3.4535312773403324in"}

## Neo6502pc Specific Hardware Specifications

### Neo6502pc -- Schematic

The latest schematic for the Neo6502pc is available on GitHub using this
link: <https://github.com/OLIMEX/Neo6502pc>

### Neo6502pc -- 12 GPIO EXT1 Connector

Neo6502pc has a CH32V003 expander IC which is connected to RP2040 via
I2C and can monitor battery charge, the presence of the external power
supply and access to the 12 GPIOs via the EXT1 connector:

  -----------------------------------------------------------------------

            ![A diagram of an electrical wiring Description automatically
                            generated](media/image32.png){width="6.125in"
                                           height="2.4027777777777777in"}

  -----------------------------------------------------------------------

            Neo6502pc and Neo6502 -- 12-pin GPIO EXT1 connector schematic

  -----------------------------------------------------------------------

# Shared Hardware

## Neo6502pc and Neo6502 -- W6502 Bus Connector

All 6502 signals are available on BUS1 connector for attaching external
hardware on it. Signals available:

- 

- +5V

- 3.3V

- GND

- D0-D7

- A0-A15

- PHI2

- R/W

- RESB

- SOB

- MLB

- VPB

- SYNC

- NMIB

- IRQB

Two signals of RP2040 SWDIO and SWCLK are also present for RP2040
debugging, these should [not be connected]{.underline} on the external
6502 peripheral boards.

  -----------------------------------------------------------------------

                           ![A diagram of a bus Description automatically
                generated](media/image33.jpg){width="4.928632983377078in"
                                            height="4.903290682414698in"}

  -----------------------------------------------------------------------

                   Neo6502pc and Neo6502 -- W6502 bus connector schematic

  -----------------------------------------------------------------------

## Neo6502pc and Neo6502 -- UEXT Connectors

UEXT (Universal EXTension) connectors have the following signals
available. All signals are with 3.3V levels.

- +3.3V

- GND

- I2C

- SPI

- UART

UEXT connector can be found in many different shapes, however the
connector used on the Neo6502pv uses a UEXT connector that is 0.1"
2.54mm step boxed plastic connector.

  -----------------------------------------------------------------------

![](media/image34.png){width="4.96875in" height="2.359461942257218in"}
-----------------------------------------------------------------------

             Neo6502 and Neo6502pc UEXT Connector w/signal identification

  -----------------------------------------------------------------------

Olimex has developed number of
[[MODULES]{.underline}](https://www.olimex.com/Products/Modules/) with
this connector: temperature, humidity, pressure, magnetic field, light
sensors, LCDs, LED matrix, Relays, Bluetooth, Zigbee, WiFi, GSM, GPS,
RFID, RTC, EKG, sensors and etc.

Neo6502pc UEXT connector is wired to RP2040 GPIOs as follows:

  -----------------------------------------------------------------------

                       ![A diagram of a circuit Description automatically
                generated](media/image35.png){width="5.395160761154855in"
                                           height="1.8612674978127735in"}

  -----------------------------------------------------------------------

                                     Neo6502 and Neo6502pc UEXT schematic

  -----------------------------------------------------------------------

## Neo6502pc and Neo6502 -- Configuration Switch Block

The Slide configuration switch can enable/disable the Buzzer, also can
connect or disconnect RESB, NMIB and IRQB to RP2040 UEXT signals.

  -----------------------------------------------------------------------

                 ![A diagram of a circuit board Description automatically
                generated](media/image36.jpg){width="6.133345363079615in"
                                           height="1.3467410323709537in"}

  -----------------------------------------------------------------------

               Neo6502 and Neo6502pc configuration switch block schematic

  -----------------------------------------------------------------------

> ![A close-up of a computer Description automatically
> generated](media/image12.png){width="2.0558694225721785in"
> height="1.563675634295713in"}

By default, the Neo6502pc is shipped with all switches in the closed
state -- enabling the Piezoelectric Buzzer Speaker, and all signals
wired to the RP2040. With SW2, SW3 and SW4 enabled, the SPI on UEXT
[cannot]{.underline} be used.

# Appendix A -- ASCII Character Codes

## Non-Printable ASCII Codes

  ---------------------------------------------------------------------------------

   Code   Ctrl      Key      Function  

  ------ ------ ----------- ---------- --------------------------------------------

    1      A    Left Arrow             Cursor Left
    
    4      D    Right Arrow            Cursor Right
    
    5      E      Insert               Insertion Mode
    
    6      F     Page Down             Cursor Page Down
    
    7      G        End                Cursor Line End
    
    8      H     Backspace             Delete Character Left
    
    9      I        Tab                Tab Character
    
    10     J                           Line Feed
    
    12     L                           Clear Screen
    
    13     M       Enter               Carriage Return (Accept Line)
    
    18     R      Page Up              Cursor Page Up
    
    19     S       Down                Cursor Down
    
    20     T       Home                Cursor Line Begin
    
    22     V                           Cursor Down (8 Lines)
    
    23     W        Up                 Cursor Up
    
    24     X                           Cursor Color Inverse
    
    26     Z      Delete               Delete Character Right
    
    27     \[     Escape               Exit

  ---------------------------------------------------------------------------------

## Printable ASCII Characters Codes

  -----------------------------------------------------------------------

        Code       Key

  ---------------- ------------------------------------------------------

       20-7F       Standard set of ASCII Characters
    
       80-8F       Set Foreground Color
    
       90-9F       Set Background Color
    
       C0-FF       User-definable Characters

  -----------------------------------------------------------------------

# 

# Appendix V -- CREDITS and LICENSE

The Neo6502 board, schematics, and firmware are all part of an Open
Source project created by Mr. Tsvetan Usunov of Bulgaria.

This document is an open-source copy-left document that is a collection
and combination of various other open-source documents and is meant to
be a superset of these documents and replaces them. Permission is hereby
granted, free of charge, to any person obtaining a copy of these
documentation files (the \"Software\"), to deal in the Software without
restriction, including without limitation the rights to use, copy,
modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to
do so, subject to the following conditions:

\* All derivatives of this document must also carry the same open-source
copy-left license.

THE SOFTWARE IS PROVIDED \"AS IS\", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR

IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,

FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
THE

AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER

LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM,

OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
IN THE

SOFTWARE.

**This license can also be found in the Github repository:**\
https://github.com/jewettg/Neo6502-Documentation/blob/main/LICENSE.md

**This document initially created by**

**Greg Jewett**\
**Email:** greg~(at)~ejewett~(dot)~com\
**Website:** <https://sites.google.com/ejewett.com/gregjewett/home>

For a list of contributing authors and updates, please visit the Github
repository:

https://github.com/jewettg/Neo6502-Documentation/blob/main/CONTRIBUTING.md

# Appendix W -- Document Revision History

Please visit the GitHub repository file CHANGELOG.md

<https://github.com/jewettg/Neo6502-Documentation/blob/main/CHANGELOG.md>

# Appendix X -- About Olimex

![A red letter m and a white background Description automatically
generated](media/image5.png){width="2.7857141294838144in"
height="0.7686843832020998in"}

  -----------------------------------------------------------------------

  **OLIMEX Ltd.**\          **Contact:** Mr. Tsvetan Usunov**\
  2 Pravda St., P.O. Box    Email:** <info@olimex.com>\
  237,\                     **Voice:** +359-32-626259,
  Plovdiv 4000 BULGARIA     +359-32-267407, +359-32-621270

  ------------------------- ---------------------------------------------

  -----------------------------------------------------------------------

Olimex Ltd is a leading provider for development tools and programmers
for embedded market.

The company has 28 years of experience in designing, prototyping and
manufacturing printed circuit boards, sub-assemblies, and complete
electronic products.

We were established in 1991 in Plovdiv - the second largest city in
Bulgaria.

We have extensive knowledge in analog, digital, and microcontroller
design, and we offer our own-designed development boards, programmers
and emulators for rapid prototyping ARM, AVR, MSP430, MAXQ and PIC
microcontrollers.

Olimex is recognized as an approved third-party hardware developer by
Texas Instruments Inc., Maxim Integrated, Atmel Inc., NXP Inc., ST
Microelectronics Inc., IAR Systems AB, Cirrus Logic Inc., OKI
Semiconductor Inc, Energy Micro Inc., and Microchip Inc.

We have over 30,000 active customer accounts who regularly use our
services for electronic boards development and prototyping. Our design
capabilities are backed by our own PCB prototype production and assembly
facility, so all designs made by us are created with
design-for-manufacturing in mind - which guarantees that they are
optimized for reliability and provide cost-effective solutions for our
customers.

The company's 5,000 sq m. production buildings are situated on our
10,000 sq m. property.

# Appendix Z -- Online Resources

As an open-source project, there are a ton of resources already
available for the Neo6502! Below is a list (not exhaustive, and growing)
of various websites, repositories, and more.\
\
If you find a link that no longer works, please let us know.

+----------------+-----------------------------------------------------+
| ![Blue text on | -   [Tindie                                         |
| a black        |     (Australia)](https://www.tin                    |
| background     | die.com/products/agon/neo6502-rev-b-available-now/) |
| Description    |                                                     |
| automatically  | -   [The Pi Hut                                     |
| ge             |                                                     |
| nerated](media | (UK)](https://thepihut.com/products/olimex-neo6502) |
| /image37.png){ |                                                     |
| width="1.33027 | -   [Digikey                                        |
| 44969378828in" |     (US)](https://www.digikey.                      |
| he             | com/en/products/detail/olimex-ltd/NEO6502/22078296) |
| ight="0.389580 |                                                     |
| 0524934383in"} |                                                     |
+:==============:+=====================================================+
| ![GitHub logo  | **Neo6502:** [Olimex Neo6502 Github                 |
| PNG            | Repository](https://github.com/OLIMEX/Neo6502/)     |
| transparent    |                                                     |
| image          | **Documentation:**                                  |
| download,      | [Neo6502-Documentation                              |
| size:          | ](https://github.com/jewettg/Neo6502-Documentation) |
| 112            |                                                     |
| 5x417px](media |                                                     |
| /image38.png){ |                                                     |
| width="0.99044 |                                                     |
| 29133858268in" |                                                     |
| he             |                                                     |
| ight="0.366971 |                                                     |
| 7847769029in"} |                                                     |
+----------------+-----------------------------------------------------+
| ![A red letter | **Neo6502 Project:** [Official Product              |
| m and a white  | Website](https://www.olimex.com/Pro                 |
| background     | ducts/Retro-Computers/Neo6502/open-source-hardware) |
| Description    |                                                     |
| automatically  | **UEXT Modules:** [UEXT Modules available from      |
| ge             | Olimex](https://www.olimex.com/Products/Modules/)   |
| nerated](media |                                                     |
| /image39.png){ |                                                     |
| width="0.88990 |                                                     |
| 81364829396in" |                                                     |
| he             |                                                     |
| ight="0.245559 |                                                     |
| 9300087489in"} |                                                     |
+----------------+-----------------------------------------------------+
| ![A blue text  | [Facebook                                           |
| on a black     | Page                                                |
| background     | ](https://www.facebook.com/groups/745798620676673/) |
| Description    |                                                     |
| automatically  |                                                     |
| gene           |                                                     |
| rated](media/i |                                                     |
| mage40.png){wi |                                                     |
| dth="1.1375in" |                                                     |
| heigh          |                                                     |
| t="0.24375in"} |                                                     |
+----------------+-----------------------------------------------------+
| ![A blue text  | [Discord                                            |
| on a black     | Server](https://discord.com/invite/Z74ZJ7VMjD)      |
| background     |                                                     |
| Description    |                                                     |
| automatically  |                                                     |
| ge             |                                                     |
| nerated](media |                                                     |
| /image41.png){ |                                                     |
| width="1.13761 |                                                     |
| 48293963255in" |                                                     |
| hei            |                                                     |
| ght="0.2150043 |                                                     |
| 7445319334in"} |                                                     |
+----------------+-----------------------------------------------------+

[^1]:

[^2]: ^†^ Provided to use only for porting code to the NeoBASIC.
    Continued use is not recommended as line numbers can change. The
    renumber and library commands or use of the built-in editor will
    change line numbers and not update gosub and goto calls.

[^3]:

[^4]:

[^5]:

[^6]:

[^7]: The HDMI port is utilized by the built-in LCD display.

[^8]: The USB-A port can be used for various USB accessories (keyboards,
    flash drives, gamepad, etc..) and for the Neo6502 -- it serves as
    the RP2040 programming port (*requires a USB-A to USB-A cable*).

[^9]: Requires the USB-Neohub
    (https://www.olimex.com/Products/USB-Modules/USB-NeoHub/open-source-hardware)
    the expand the number of USB-A ports available. Additional ports are
    required to utilize NeoBasic, as you need at minimum a flash drive
    and keyboard. *Unlike the USB-Neohub, not all USB hubs are
    compatible or supported*.
