# NEOBASIC Commands

## File System and I/O Commands

| **Command**<img src="images/empty.gif" width="600" height="1"> | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `close` *handle*                                               | Close a file by handle.   The handle is optional, and if not provided, all files will be closed.                                                                                                                                                                                                                                                                                                                                                                            |
| `input` #*channel*,*var*,*var*                                 | Reads a sequence of variables from the open file.                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `ireceive` *d*,*a,s*                                           | Receive bytes starting at a, count s to or from device d.                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `itransmit` *d*,*a,s*                                          | Send bytes starting at *a*, count *s* to or from device *d*.                                                                                                                                                                                                                                                                                                                                                                                                                |
| `isend` *device*,*data*                                        | Send data to i2c {device}; this is comma separated data, numbers or strings. If a semicolon is used as a separator e.g. 4137; then the constant is sent as a 16-bit value.                                                                                                                                                                                                                                                                                                  |
| `iwrite` *dev*,*reg*,*b*                                       | Write byte to I2C Device Register                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `load` "*file*[",*address*]                                    | Load file to BASIC space or given address.  The last quote is optional if the parameter *address* *is not used.*                                                                                                                                                                                                                                                                                                                                                            |
| `mos` "command"                                                | Execute Machine Operating System (MOS) command within the quotes.   Commands include cat, cd, md, copy, del, and more.   **<br>See**: MOS Commands                                                                                                                                                                                                                                                                                                                          |
| `open` [ `input` \| `output` ] *channel*,*file*                | Open a file for input or output on the given channel, using the given file name. Output erases the current file. This gives an error if the file does not exist; rather than trap this error it is recommended to use the exists() function if you think the file may not be present.                                                                                                                                                                                       |
| `print` [*string* \| *var*]                                    | This will output the contents that following the command at the current cursor position.  A string is encapsulated in double-quotes.  A variable can be a string or number variable.<br/><br/>You can concatenate strings and vars together with the `+` symbol. If you end a print statement with a semi-colon, then the next print will follow at the end of this print, effectively appending to the output.  Use `str$(var)` to concatenate an integer with the string. |
| `print #`*channel*,*expr*,*expr*                               | Writes a sequence of expressions to the open file.                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `print line #`*channel*.*var*.*var*                            | Prints a line to an output channel as an ASCII file, using LF (linefeed) line-break format (e.g. lines are separated by character code 10). This can be mixed with the above format, *but* the sequence has to be the same; you can't write a string using print line and read it back with input and vice versa. All variables must be strings.                                                                                                                            |
| `run` "*program*"                                              | Load & Run program.   *The last quotation mark is optional.*                                                                                                                                                                                                                                                                                                                                                                                                                |
| `save` "*file*[",*adr*,*sz*]                                   | Save BASIC program or memory from *adr* length *sz*.  The last quote is option if the *adr* *or* *sz* *parameters are not used.*                                                                                                                                                                                                                                                                                                                                            |
| `sreceive` *a,s*                                               | Receive bytes starting at a, count s to SPI device                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `stransmit` *a,s*                                              | Send bytes starting at a, count s to SPI device                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `ssend` *data*                                                 | Send data to SPI device; this is comma separated data, numbers or strings. If a semicolon is used as a separator e.g. 4137; then the constant is sent as a 16-bit value.                                                                                                                                                                                                                                                                                                    |
| `ureceive` *d*,*a,s*                                           | Receive bytes to/from the UART starting at a, count s                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `utransmit` *d*,*a,s*                                          | Send *d* bytes to/from the UART starting at *a*, count *s*                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `usend` *device*, *data*                                       | ~~Send data to UART; this is comma separated data, numbers or strings. If a semicolon is used~~                                                                                                                                                                                                                                                                                                                                                                             |

## BASIC Commands

| **Command**<img src="images/empty.gif" width="400" height="1"> | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `'` string                                                     | Comment. <br/>This is a string for syntactic consistency. If you type in the comment without the speech marks, which is easier, the speech marks will be added automatically.  See comments (page 13).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `assert` *expr*[,*msg*]                                        | Error generated if {expr} is zero, with optional message.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `cls`                                                          | Clear the graphics screen to current background color. This does not clear sprites.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `cursor` *x*,*y*                                               | Set the text cursor position to position x,y on the screen.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `data` *const*,*const*,…                                       | DATA statement. Strings must be enclosed in quote marks. *A read statement will read the data found in the data statements.**  **See**:  `read` and `restore`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `defchr` ch,....                                               | Define UDG ch (192-255) as a 6x7 font -- should be followed by 7 values from 0-63 representing the bit pattern of the graphic, so if these numbers are converted to a 6 digit binary numbera '1' represents a pixel that is 'on', and a '0' represents a pixel that is off.                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `delete`                                                       | Delete a line or range of lines                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `dim` *array*(*n*,[*m*]), $...                                 | Dimension a one or two dimension string or number array, up to 255 items.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `edit`                                                         | Basic Screen Editor                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| `fkey`                                                         | Lists the defined function keys                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `fkey` *key*,*string*                                          | Define the behavior of F1..F10 -- the characters in the string                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `ink` *fgr*[,*bgr*]                                            | Set the ink foreground and optionally background for the console.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `input` *var*                                                  | Will wait for input that ends when hitting return. The content entered will be stored in the variable *var*.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `let` *var*`=`*expr*                                           | Assignment statement. The let is optional.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `library` *from, to*                                           | The library command allows you to hide a section of code from the program listing.  It can be performed multiple times with different ranges to hide discontinuous sections of code.  The hidden “library” code persists in files saved, and will remain hidden even when the program is loaded from storage.                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `library`                                                      | Entering the command without any parameters will unhide all sections of code and renumber the program starting at 1000.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `list`                                                         | List an entire program                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `list` *from*                                                  | List a program starting at line number *from*                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `list` *from, to*                                              | List a program starting at line number *from* to the line number *to*.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `list` *procedure*`()`                                         | List the lines associated with a particular procedure.  *Must have the* () *following the name of the procedure.*                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `local` *var*, *var*                                           | Define local variables within a procedure scope.  Variables with the same name as local variables will be restored and then conclusion of the procedure execution, and local values will be lost if not assigned to global variables or passed out via the “def” parameter modifier.                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `mouse cursor` *n*                                             | Changes the mouse cursor to the shape found in the table below ("mouse cursor values")                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `mouse show`                                                   | Show the mouse on the screen                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `mouse` `TO` *x*,*y*                                           | Position mouse cursor                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| `new`                                                          | Erase any program in memory.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `old`                                                          | Reverses the “new” statement.  <br/>*Please note, that this could fail, depending on what actions or changes to memory has taken place since “new” was executed*.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `palette` c,r,g,b                                              | Set color c to r,g,b values.  Values are all 0 – 255, however it is actually 3:2:3 color, so the result will be approximations.   An example would be palette 7,255,128,0 which sets color 7 (normally white) to R = 255, G = 128, B = 0 which is orange.                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `palette clear`                                                | Reset palette to default                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `proc` *name*([p1,p2,…]) <br/>… <br/>`endproc`                 | Creates a procedure, that can optionally have parameters.   If parameters are used, when the procedure is called, the parameters must match (number and types) exactly.<br/><br/>Parameters can be defined as reference parameters and will return values. proc *name*( [ref] *p1*, [ref] *p2*,[ref]...) Any change to the variables will be passed back at the end of the procedure in the same variables that were in the parameter section.<br/><br/>Parameters cannot be arrays.   <br/><br/>**NOTE:**  Procedures should be defined at the end of a basic program and an “end” statement need to precede the declaration section.  If the BASIC program execution hits a proc declaration, a syntax error will be presented. |
| `call` *name* ([*p1,p2,…*])                                    | Call named procedure with optional parameters. <br><br> If the procedure is defined with reference variables, then values are returned at the end of the procedure call.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `renumber` [*start*]                                           | **Renumber<br> the program in memory starting at 1000, or from the optional parameter** ***start*.**      ![*](file:////Users/jewettg/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)**The** **renumber command will not<br> change any line numbers used with** **goto or** **gosub****.  *These are commands are not recommended<br> for use and should be used at your own risk.***                                                                                                                                                                                                                                                                                                 |
| `read` *var*, …                                                | Read variables from data statements. Variable type must match the data (string or integer) in the data statements.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `restore`                                                      | Restore data pointer to the beginning of the data statements.  Performing a read will read from the very first data constant.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `restore` *line*                                               | Restore data pointer to line number[[GJ1]](#_msocom_1)  *line.*      ![*](file:////Users/jewettg/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)The restore command uses line numbers, which are not guaranteed to remain the same in a program. *These are commands are not recommended for use and should be used at your own risk.*                                                                                                                                                                                                                                                                                                                                                 |
| `tilemap` *addr,x,y*                                           | Define a tilemap.   The tilemap data format is in the API. The tilemap is stored in memory at addr, and the offset into the                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

### Mouse Cursor Values

| Code | Cursor Shown            | Code | Cursor Shown        | Code | Cursor Shown       | Code | Cursor Shown   |
| ---- | ----------------------- | ---- | ------------------- | ---- | ------------------ | ---- | -------------- |
| 0    | White Arrow (*default*) | 10   | Hourglass           | 20   | White arrow        | 30   | Dinosaur       |
| 1    | Fly                     | 11   | Paint bucket (fill) | 21   | Black arrow        | 31   | Crown          |
| 2    | *Unknown*               | 12   | Right-angle tool    | 22   | Small arrow        | 32   | Dice           |
| 3    | *Target*                | 13   | Right-angle tool    | 23   | Very small arrow   | 33   | Globe          |
| 4    | *Unknown*               | 14   | *Unknown*           | 24   | Very small pointer | 34   | Christmas tree |
| 5    | *Unknown*               | 15   | Pencil              | 25   | Arrow              | 35   | Olympic rings  |
| 6    | *Unknown*               | 16   | Paintbrush          | 26   | Finger pointing    | 36   | Mountain peek  |
| 7    | *Unknown*               | 17   | Unknown             | 27   | Watermelon         | 37   | Flower         |
| 8    | Dartboard               | 18   | Yin-yang            | 28   | Lime               | 38   | Floppy disk    |
| 9    | Magnifying glass        | 19   | Paint brush         | 29   | Lemon              |      |                |

*The above list was accurate as of firmware version 0.99.1, and could change.* 

### Interfacing with hardware

| **Command**<img src="images/empty.gif" width="250" height="1"> | **Description**                                                                                                                                                                                                                                                                                   |
| -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `clear` [*address*]                                            | Clear out stack, strings, reset all variables. If an address is provided, then memory above that will not be touched by BASIC.  Note because this resets the stack, it cannot be done in a loop, subroutine or procedure -- they will be forgotten. Also clears the sprites and the sprite layer. |
| `doke` *addr*,*data*                                           | Write word to address                                                                                                                                                                                                                                                                             |
| `mon`                                                          | Enter the machine code monitor                                                                                                                                                                                                                                                                    |
| `pin`*pin*,*value*                                             | Set UEXT {pin} to given value.                                                                                                                                                                                                                                                                    |
| `pin` *pin* INPUT                                              | output                                                                                                                                                                                                                                                                                            |
| `poke` addr*,*data*                                            | Write byte to address                                                                                                                                                                                                                                                                             |
| `sys` *address*                                                | Call 65C02 machine code at given address. Passes contents of variables A,X,Y in those registers.                                                                                                                                                                                                  |
| `uconfig` *baud*[,*prt*]                                       | Set the baud rate and protocol for the UART. Currently only 8 data-bits, no parity and 1 stop-bit are supported.                                                                                                                                                                                  |

---

## MOS Commands

Using the MOS commands to access OS disk/file functionality. MOS commands can be used in three (3) different ways:

- On the NeoBASIC command line, prefixing the command with an asterisk.  
  **Example**:  `*del myfile.txt`

- On the NeoBASIC command line, use the mos BASIC command. Surround the MOS command in quotes.  
  **Example**:  `mos "del myfile.txt"` 

- Within a NeoBASIC program, you can use the mos BASIC function.  Surround the MOS command in quotes.  
  **Example**:  `if mos("del myfile.txt") > 0 then …` 

### Available MOS Commands

| Command | Description of function                                                                                                                                                                                                                                                                                                                      |
| ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `cat`   | Prints out the file listing of the current directory.                                                                                                                                                                                                                                                                                        |
| `del`   | Delete the specified file. <br> Can be used to delete a directory, only if it is empty.  *If a directory is not empty, the commandgenerates an error.*                                                                                                                                                                                       |
| `copy`  | Copy on path to another.   Requires two parameters, the first is the source path and file, and the second is the destination path and file.   If no path is given, but different names are given, then you get a copy of the source file specified in the first parameter in the current directory.   You cannot make copies of directories. |
| `file`  | Will verify that a file (*not a directory*) exists.  Return zero if it exists, and non-zero if it does not exist.                                                                                                                                                                                                                            |
| `ren`   | Will rename a file or directory.  Works identically as the copy command.                                                                                                                                                                                                                                                                     |
| `md`    | Make a directory.  Specify the name of the directory you want to create.                                                                                                                                                                                                                                                                     |
| `cd`    | Change Directory. Specify the name of the directory that you wish to traverse.                                                                                                                                                                                                                                                               |

**Standard Unix POSIX paths are used with the MOS commands.** 

| Path | Meaning, change made to current directory                                                                      |
| ---- | -------------------------------------------------------------------------------------------------------------- |
| ./   | Current directory                                                                                              |
| ../  | Go up in the hierarchy, relative to the current directory (*sometime referred as the going back a directory*). |
| /    | The root or top-level directory.                                                                               |

### MOS Error Codes

Most of the functions will return an error/status code to indicate whether the operation succeeded or not.

| **Name**                     | **Value** | **Meaning**                                                            |
| ---------------------------- | --------- | ---------------------------------------------------------------------- |
| `FIOERROR_OK`                | `0x00`    | Operation succeeded (not an error)                                     |
| `FIOERROR_UNKNOWN`           | `0x01`    | Something went wrong, but we don't know what                           |
| `FIOERROR_EOF`               | `0x02`    | A read or directory enumeration operation reached the end of the file. |
| `FIOERROR_UNIMPLEMENTED`     | `0x03`    | Operation is not implemented                                           |
| `FIOERROR_NO_FILE`           | `0x11`    | Could not find the file                                                |
| `FIOERROR_NO_PATH`           | `0x12`    | Could not find the path                                                |
| `FIOERROR_INVALID_DRIVE`     | `0x13`    | The logical drive number is invalid                                    |
| `FIOERROR_INVALID_NAME`      | `0x14`    | The path name format is invalid                                        |
| `FIOERROR_INVALID_PARAMETER` | `0x15`    | Given parameter is invalid                                             |
| `FIOERROR_DENIED`            | `0x21`    | Access denied due to prohibited access or disk or directory full       |
| `FIOERROR_EXIST`             | `0x22`    | Access denied due to prohibited access                                 |
| `FIOERROR_INVALID_OBJECT`    | `0x23`    | The file/directory object is invalid                                   |
| `FIOERROR_WRITE_PROTECTED`   | `0x24`    | The physical drive is write-protected                                  |
| `FIOERROR_LOCKED`            | `0x25`    | File is in use                                                         |
| `FIOERROR_DISK_ERR`          | `0x31`    | A hard error occurred in the low-level disk I/O layer                  |
| `FIOERROR_INT_ERR`           | `0x32`    | Assertion failed                                                       |
| `FIOERROR_NOT_READY`         | `0x33`    | The physical drive cannot work                                         |
| `FIOERROR_NOT_ENABLED`       | `0x34`    | The volume has no work area                                            |
| `FIOERROR_NO_FILESYSTEM`     | `0x35`    | The filesystem is invalid                                              |

### File

| **Name**           | **Value** | **Meaning**                                                       |
| ------------------ | --------- | ----------------------------------------------------------------- |
| `FIOATTR_DIR`      | `0x01`    | This is a directory (may not be modified)                         |
| `FIOATTR_SYSTEM`   | `0x02`    | This is a system file and will be hidden from directory listings  |
| `FIOATTR_ARCHIVE`  | `0x04`    | File is archived; automatically cleared when the file is modified |
| `FIOATTR_READONLY` | `0x08`    | File is read only and may not be overwritten or modified          |
| `FIOATTR_HIDDEN`   | `0x10`    | This will be hidden from directory listings                       |

----

## Graphics Commands

| **Command** <img src="images/empty.gif" width="325" height="1"> | **Description**                                                                                                                                                                                                                                                                                                     |
| --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `gload` *filename*                                              | Load filename into graphics memory.  <br> **Example:** gload “mygraphics.gfx”                                                                                                                                                                                                                                       |
| `from` x,y                                                      | Sets<br> the origin position, can be repeated and optional.                                                                                                                                                                                                                                                         |
| `to` x,y                                                        | Draw the element at x,y or between the current<br> position and x,y depending on the command. So you could have **text "Hello" to 10,10** or **rect 0,0 to<br> 100,50**                                                                                                                                             |
| `by` x,y                                                        | Same<br> as to but x and y are an offset from the current position                                                                                                                                                                                                                                                  |
| *x,y*                                                           | Set the current position without doing the action                                                                                                                                                                                                                                                                   |
| `ink` *c*                                                       | Modifier<br> to a graphic command to change what color c the command will use.                                                                                                                                                                                                                                      |
| `ink` *a,x*                                                     | Modify the color that is used in the graphics<br> commands by using the screen color and performing a binary AND with parameter<br> a, and performing a binary OR with parameter x.                                                                                                                                 |
| `solid`                                                         | Fill in rectangles and ellipses. For images and text, forces black background.[[GJ1]](#_msocom_1)                                                                                                                                                                                                                   |
| `text` {str} to x,y                                             | Draw/place text at t c0dc0he specified *x,y* coordinates.<br> `text “Hello World!” to 20,20`                                                                                                                                                                                                                        |
| `rect` {*`solid`* \| *`frame`*} *x1,y1* `to` *x2,y2*            | <br/>Will draw a rectangle with the upper-left-corner (x1,y1) to the bottom-right-corner (x2,y2).   **Note:** Once used, rect and ellipse commands will continue using the<br> solid or frame state until changed.   **See** frame,<br> solid                                                                       |
| ellipse                                                         | ellipse {*solid* \| *frame*} x1,y1 to x2,y2                                                                                                                                                                                                                                                                         |
| `frame`                                                         | A modifier for rectangles and ellipses, where it will only draw the outline, not filling it in.<br>`rect frame x1,y1 to x2,y2` will draw an empty rectangle.                                                                                                                                                        |
| `solid`                                                         | A modifier for rectangles and ellipses, where it will only draw a filled in rectangle or ellipse. <br/>`rect solid *x1,y1* to *x2,y2`* will draw a filled in rectangle.                                                                                                                                             |
| `dim` n                                                         | Set the scaling to *n* (for text, image, and tilemap only), and must be an integer.   <br/>`text "Hello" dim 2 to 10,10 to 10,100` will draw the word “Hello” at double its size!    <br/>Tiles can only be scaled at 1 or 2 (when scaling at 2, tiles are drawn at a size of 32x32, versus the scale 1 of 16 x16). |
| `move`                                                          |                                                                                                                                                                                                                                                                                                                     |
| `plot`                                                          |                                                                                                                                                                                                                                                                                                                     |
| `line`                                                          |                                                                                                                                                                                                                                                                                                                     |
| `flip`                                                          | flip is an optional modifier for the sprite command.<br><br> 0 = no flip; 1= horizontal flip; 2 = vertical flip; or 3 = both vertical and horizontal.                                                                                                                                                               |
| `anchor`                                                        |                                                                                                                                                                                                                                                                                                                     |
| `image`                                                         |                                                                                                                                                                                                                                                                                                                     |
| `sprite`                                                        |                                                                                                                                                                                                                                                                                                                     |

### Pixel Colors

<style>
.box {
  height: 20px;
  width: 20px;
  margin-bottom: 15px;
  border: 1px solid black;
}
.black { background-color: #000000;}
.red { background-color: #ff0044;}
.green { background-color: #00ee33;}
.yellow { background-color: #ffee22;}
.blue { background-color: #112255;}
.magenta { background-color: #772255;}
.cyan { background-color: #22aaff;}
.white { background-color: #ffffee;}
.black { background-color: #000000;}
.dark-gray { background-color: #555544;}
.dark-green { background-color: #008855;}
.orange { background-color: #ffaa00;}
.dark-orange { background-color: #aa5533;}
.brown { background-color: #887799;}
.pink { background-color: #ffccaa;}
.light-gray {  background-color: #cccccc;}
</style>

These colors are approximations and will vary depending on the type and image adjustments of the display.

<table width="100%">
   <tr>
      <td>0</td><td><code>$80</code></td><td>Black<br><i>Transperant</i></td><td><code>#000000</code></td><td><div class="box red"></div></td>
      <td>8</td><td><code>$88</code></td><td>Black</td><td><code>#000000</code></td><td><div class='box black'></div></td>
   </tr>
   <tr>
      <td>1</td><td><code>$81</code></td><td>Red</td><td><code>#ff0044</code></td><td><div class='box red'></div></td>
      <td>9</td><td><code>$89</code></td><td>Dark Grey</td><td><code>#555544</code></td><td><div class='box dark-grey'></div></td>
   </tr>
   <tr>
      <td>2</td><td><code>$82</code></td><td>Green</td><td><code>#00ee33</code></td><td><div class='box green'></div></td>
      <td>10</td><td><code>$8A</code></td><td>Dark Green</td><td><code>#008855</code></td><td><div class='box dark-green'></td>
   </tr>
   <tr>
      <td>3</td><td><code>$83</code></td><td>Yellow</td><td><code>#ffee22</code></td><td><div class='box yellow'></div></td>
      <td>11</td><td><code>$8B</code></td><td>Orange</td><td><code>#ffaa00</code></td><td><div class='box orange'></td>
   </tr>
   <tr>
      <td>4</td><td><code>$84</code></td><td>Blue</td><td><code>#112255</code></td><td><div class='box blue'></div></td>
      <td>12</td><td><code>$8C</code></td><td>Dark Orange</td><td><code>#aa5533</code></td><td><div class='box dark-orange'></td>
   </tr>
   <tr>
      <td>5</td><td><code>$85</code></td><td>Magenta</td><td><code>#772255</code></td><td><div class='box magenta'></div></td>
      <td>13</td><td><code>$8D</code></td><td>Brown</td><td><code>#887799</code></td><td><div class='box brown'></td>
   </tr>
   <tr>
      <td>6</td><td><code>$86</code></td><td>Cyan</td><td><code>#22aaff</code></td><td><div class='box cyan'></div></td>
      <td>14</td><td><code>$8E</code></td><td>Pink</td><td><code>#ffccaa</code></td><td><div class='box pink'></td>
   </tr>
   <tr>
      <td>7</td><td><code>$87</code></td><td>White</td><td><code>#ffffee</code></td><td><div class='box white'></div></td>
      <td>15</td><td><code>$8F</code></td><td>Light Grey</td><td><code>#cccccc</code></td><td><div class='box light-grey'></td>
   </tr>
</table>

<table width="100%">
  <tr>
    <td>0</td> <td><tt>$80</tt></td> <td>Black<br><i>Transperant</i></td> <td><tt>#000000</tt></td> <td><div class="box red"></div></td>        
  </tr>
  <tr>
    <td>1</td> <td><tt>$81</tt></td> <td>Red</td> <td><tt>#ff0044</tt></td><td><div class='box red'></div></td>
  </tr>
  <tr>
    <td>2</td> <td><tt>$82</tt></td> <td>Green</td>  <td><tt>#00ee33</tt></td><td><div class='box green'></div></td>        
  </tr>
  <tr>
    <td>3</td> <td><tt>$83</tt></td> <td>Yellow</td> <td><tt>#ffee22</tt></td><td><div class='box yellow'></div></td>
  </tr>
  <tr>
    <td>4</td> <td><tt>$84</tt></td> <td>Blue</td> <td><tt>#112255</tt></td> <td><div class='box blue'></div></td>        
  </tr>
   <tr>
    <td>5</td> <td><tt>$85</tt></td> <td>Magenta</td> <td><tt>#772255</tt></td> <td><div class='box magenta'></div></td>
  </tr>
 <tr>
    <td>6</td> <td><tt>$86</tt></td> <td>Cyan</td> <td><tt>#22aaff</tt></td> <td><div class='box cyan'></div></td>        
  </tr>
  <tr>
    <td>7</td> <td><tt>$87</tt></td> <td>White</td> <td><tt>#ffffee</tt></td> <td><div class='box white'></div></td>
  </tr>
  <tr>
    <td>8</td> <td><tt>$88</tt></td> <td>Black</td> <td><tt>#000000</tt></td> <td><div class='box black'></div></td>        
  </tr>
  <tr>
    <td>9</td> <td><tt>$89</tt></td> <td>Dark Grey</td> <td><tt>#555544</tt></td> <td><div class='box dark-grey'></div></td>
  </tr>
  <tr>
    <td>10</td> <td><tt>$8A</tt></td> <td>Dark Green</td> <td><tt>#008855</tt></td> <td><div class='box dark-green'></td>        
  </tr>
  <tr>
     <td>11</td> <td><tt>$8B</tt></td> <td>Orange</td> <td><tt>#ffaa00</tt></td> <td><div class='box orange'></td>
  </tr>
  <tr>
     <td>12</td> <td><tt>$8C</tt></td> <td>Dark Orange</td> <td><tt>#aa5533</tt></td> <td><div class='box dark-orange'></td>       
  </tr>
  <tr>
     <td>13</td> <td><tt>$8D</tt></td> <td>Brown</td> <td><tt>#887799</tt></td> <td><div class='box brown'></td>
  </tr>
  <tr>
     <td>14</td> <td><tt>$8E</tt></td> <td>Pink</td> <td><tt>#ffccaa</tt></td> <td><div class='box pink'></td>        
  </tr>
  <tr>
    <td>15</td> <td><tt>$8F</tt></td> <td>Light Grey</td> <td><tt>#cccccc</tt></td> <td><div class='box light-grey'></td>
  </tr>
</table>

| Pixel | **Hex** | Color                   |         |                                 | Pixel | **Hex** | Color       |         |                                     |
| ----- | ------- | ----------------------- | ------- | ------------------------------- | ----- | ------- | ----------- | ------- | ----------------------------------- |
| 0     | $80     | Black<br> *Transparent* | #000000 | <div class='box black'></div>   | 8     | $88     | Black       | #000000 | <div class='box black'></div>       |
| 1     | $81     | Red                     | #ff0044 | <div class='box red'></div>     | 9     | $89     | Dark Grey   | #555544 | <div class='box dark-grey'></div>   |
| 2     | $82     | Green                   | #00ee33 | <div class='box green'></div>   | 10    | $8A     | Dark Green  | #008855 | <div class='box dark-green'></div>  |
| 3     | $83     | Yellow                  | #ffee22 | <div class='box yellow'></div>  | 11    | $8B     | Orange      | #ffaa00 | <div class='box orange'></div>      |
| 4     | $84     | Blue                    | #112255 | <div class='box blue'></div>    | 12    | $8C     | Dark Orange | #aa5533 | <div class='box dark-orange'></div> |
| 5     | $85     | Magenta                 | #772255 | <div class='box magenta'></div> | 13    | $8D     | Brown       | #887799 | <div class='box brown'></div>       |
| 6     | $86     | Cyan                    | #22aaff | <div class='box cyan'></div>    | 14    | $8E     | Pink        | #ffccaa | <div class='box pink'></div>        |
| 7     | $87     | White                   | #ffffee | <div class='box white'></div>   | 15    | $8F     | Light Grey  | #cccccc | <div class='box light-grey'></div>  |

### Sprite Commands

Sprites are two-dimensional bitmaps that is integrated into a larger scene.  Sprites are loaded from graphics file that holds sprites, tiles and other objects.   Sprites can be drawn and moved without disturbing the current screen background.  For example, a flagpole can be an image drawn on the screen, but a flag moving up or down the flagpole is usually a sprite so it can be easily displayed and moved without disturbing the drawn flagpole.

The Neo6502 graphics system has one sprite layer (z-plane) in the conventional sense, however technically, there is no "sprite layer". The system uses palette manipulation to create, what is in practice, a pair of 4-bit bit-planes. The sprite graphics are in the upper nibble, the background is in the lower nibble, and the background is drawn only if the sprite graphic layer is zero.

**NOTE**:  There can be a total of 128 (0–127) sprites defined in memory, 127 user sprites, and a single sprite 128 which is used to show the “turtle” in turtle graphics mode, which can be overwritten if turtle graphics is not going to be used.

A typical sprite command is:

`SPRITE *n* [image *i*] [TO *x*,*y*] [FLIP *f*] [BY *x*,*y*] [ANCHOR *a*] [CLEAR]`

As with graphics commands not all options are required, as they are options, which applies modifiers to the command.  
You can simply use `SPRITE 1 IMAGE 3`.

*Modifiers include:*

<table width="100%">
      <tr>
         <th>Modifier<img src="images/empty.gif" width="250" height="1"></th>
         <th>Description of what action the modifier performs</th>
      </tr>
      <tr>
         <td><code>IMAGE</code> <em>i</em></td>
         <td>which sets the image for sprite <em>n</em> to image <em>i</em>.   <em>n</em> is the sprite number</td>
      </tr>
      <tr>
         <td><code>TO</code><em>x</em>,<em>y</em></td>
         <td>which sets the position of the sprite to coordinates <em>x</em>,<em>y</em></td>
      </tr>
      <tr>
         <td><code>FLIP</code> <em>f</em></td>
         <td>which sets the orientation of sprite <em>f</em>.   <br/><strong>Values:*</strong> <em>0</em> <em>= no flip;</em> <em>1**= horizontal flip;</em> <em>2</em> <em>= vertical flip; or</em> <em>3</em> <em>= both vertical and horizontal.</em></td>
      </tr>
      <tr>
         <td><code>ANCHOR</code><em>a</em></td>
         <td>which sets the anchor point.  <em>See</em></td>
      </tr>
      <tr>
         <td><code>BY</code> <em>x</em>,<em>y</em></td>
         <td>which sets the position by offset</td>
      </tr>
      <tr>
         <td><code>CLEAR</code></td>
         <td>This will reset all sprites and removed them from the display.</td>
      </tr>
   </tbody>
</table>

Example:

`SPRITE 1 IMAGE 2 TO 200,200 SPRITE 2 IMAGE 3 BY 10,10`

The above command will set sprite 1 to image 2 and then display it at coordinate 200,200 and set sprite 2 to image 3 and move sprite 2 to an offset position of 10,10 from its current position.

**Implementation notes**

- Up to 128 sprites are supported. Sprites are drawn by the RP2040 processor, so keep in mind displaying a large number of sprites on the screen will reduce overall system performance.

- Sprites are currently done with XOR drawing, which causes some flickering effects when they overlap. This should not be relied on (it may be replaced by clear/invalidate system at some point), but the actual implementation should not change.

## Sprite Support Functions

| Function <img src="images/empty.gif" width="250" height="1"> | Description                                                                                                                                                                        |
| ------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `spritex`(*n*)                                               | Will return the *x* coordinate of sprite *n*                                                                                                                                       |
| `spritey`(*n*)                                               | Will return the *y* coordinate of sprite *n*                                                                                                                                       |
| `hit`(*s1, s2, d*)                                           | Detect a sprites collision.   It returns true if the pixel distance<br> between the center of sprite s1 and the center of sprite s2 is less than or equal to the<br> distance *d*. |

**Example:**

If you wanted to move a sprite until it collided with another sprite, assuming both are 32x32, the collision distance would be 32 (the distance from the center to the edge of both sprites added together). 

```
x = 0
repeat
  x = x + 1: sprite 1 to x,40
until hit(1,2,32)
```

**Game Design**

Using the hit function with various distance values should be tested with the various applications to improve the “feel” of game play.   Experimenting with different distance values based on the shape and the size of the sprites can greatly improve the experience, where near exact collision detection would make the experience better.

**Sprite Drawing Anchor Points**

| 7   | 8   | 9   |
| --- | --- | --- |
| 4   | 0/5 | 6   |
| 1   | 2   | 3   |

The table below shows the valid anchor alignments for a sprite. The anchor position is the origin of the relative coordinate 0,0 of the sprite.  Based on the anchor point value provided, coordinate 0,0 will coincide with one of the positions shown in the table below. *The default anchor alignment is zero (middle-center).*

----

# Sounds and Music

Queued sounds are played sequentially, each after the previous has completed, such that sounds within a channel queue will not conflict, interrupt, or overlap.  Frequency is in units of hertz. Duration is in units of 100ths of a second. Slide is a gradual linear change in frequency, in units of Hz per 100th of a second. Sound target type 0 is the beeper. Currently, the beeper is the only available sound target.

| Function <img src="images/empty.gif" width="250" height="1"> | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sound clear`                                                | Resets the entire sound system, silences all channels, empties all queues.                                                                                                                                                                                                                                                                                                                                                                                            |
| `sound` *c* clear                                            | Resets a single channel *c*; silences it and empties its queue.                                                                                                                                                                                                                                                                                                                                                                                                       |
| `sound` *c, f, t* [, *s*]                                    | Queues a note on the given channel *c* of the given frequency *f* (in Hz) and time *t* (*in centiseconds*). These will be played in the background as other notes finish so you can 'queue up' an entire phrase and let it play by itself.   The slide *s* value adds that much to the frequency *f* every centisecond allowing some additional effects (note, done in 50Hz ticks)  <br/><br/>A mixture of the two syntaxes `SOUND 0 CLEAR 440,200` is now supported. |
| noise                                                        | White noise feature.  Use this command in lieu of sound.                                                                                                                                                                                                                                                                                                                                                                                                              |
| sfx *c, e*                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

**Sound effects**

These will be synthesized to the best ability of the available hardware, so the actual sound may vary slightly. 

| **ID** | **Sound** | **ID** | **Sound**  | **ID** | **Sound**  |
|:------:| --------- |:------:| ---------- |:------:| ---------- |
| 0      | positive  | 8      | powerup    | 16     | ringtone 2 |
| 1      | negative  | 9      | victory    | 17     | ringtone 3 |
| 2      | error     | 10     | defeat     | 18     | danger     |
| 3      | confirm   | 11     | fanfare    | 19     | expl100    |
| 4      | reject    | 12     | alarm 1    | 20     | expl50     |
| 5      | sweep     | 13     | alarm 2    | 21     | expl20     |
| 6      | coin      | 14     | alarm 3    | 22     | las30      |
| 7      | las70     | 15     | ringtone 1 | 23     | las10      |
