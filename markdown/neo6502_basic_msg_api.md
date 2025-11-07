# Raspberry PI 2040 Messaging API

The Neo6502 uses a Raspberry PI 2040 to provide memory, graphics support, file I/O capabilities and external (UEXT) interface.   BASIC and many other programming languages for the Neo6502 computer implement many of the RP2040 capabilities utilizing the messaging API behind commands and functions.   The following section documents the native While the BASIC and other programming languages implement many of these capabilities as command and functions, many of the routines can be access natively via the RP2040 messaging API.

The Neo6502 API is a messaging system. There are no methods to access the hardware directly. Messages are passed via the block of memory from `$FF00` to `$FF0F`, as specified in the "API Messaging Addresses" table below.

## API Messaging Addresses

<table width="100%" cellpadding="5">
  <tr>
    <td width="20%"><b>Address</b></td>
    <td width="20%"><b>Type</b></td>
    <td width="60%"><b>Notes</b></td>
  </tr>
  <tr>
    <td><code>$FF00</code></td>
    <td>Group</td>
    <td>Group selector and status. Writing a non-zero value to this location triggers the routine specified in <code>$FF01</code>. The system will respond by setting the “error” and “parameters” values appropriately. Upon completion, this memory location will be will cleared.</td>
  </tr>
  <tr>
    <td><code>$FF01</code></td>
    <td>Function</td>
    <td>A command or function within the specified group.</td>
  </tr>
  <tr>
    <td><code>$FF02</code></td>
    <td>Error</td>
    <td>Return any error values, 0 = no error.</td>
  </tr>
  <tr>
    <td colspan=3><b>Note</b>:<i><code>FF03:0</code> through <code>FF03:6</code> are not used.</i></td>
  </tr>
  <tr>
    <td><code>$FF03:7</code></td>
    <td>Status</td>
    <td>Set (1) if the ESCape key has been pressed. This is not automatically reset.</td>
  </tr>
  <tr>
    <td><code>$FF04 … $FF0B</code></td>
    <td>Parameters</td>
    <td>This memory block is notated in this document as <code>Param[0]</code> (<code>$FF04</code>) through <code>Param[7]</code> (<code>$FF0B</code>), each a single byte, or combined (<code>Param[0,1]</code> is a word *representing a larger value*).  The addressing space is little-endian, least significant byte (LSB) at the lowest memory address. <br><br> Many functions require values be present in these memory locations (as parameters of the function).  Functions may also return values in these memory locations.</td>
  </tr>
</table>

The above address map and the tables describing the functions found in this section can be found in numerous sections of the firmware release download:

·       `examples/assembly/neo6502.inc`

·       `examples/C/neo6502.h`

## 

## Using RP2040 messaging API in NeoBASIC

### Procedure to make Messaging API call

<table width="100%" cellpadding="5">
  <tr>
    <td width="40%"><code>proc sendmsg(g,f)</code></td>
    <td width="60%">Define a procedure to “send a message” using parameters f (function), and p (parameters)</td>
  </tr>
  <tr>
    <td><code>while peek($FF00):wend</code></td>
    <td>Wait until the messaging API is ready to accept a message (all previous commands in queue have completed)</td>
  </tr>
  <tr>
    <td><code>poke $FF01,f:poke $FF00,g</code></td>
    <td>Poke the function and group.  If there are parameters to pass, you must have those entered into memory (poke) prior to performing “poke $ff00,g”, as this call the function immediately.</td>
  </tr>
  <tr>
    <td><code>while peek($FF00):wend</code></td>
    <td>Wait until for the function to complete.</td>
  </tr>
  <tr>
    <td><code>endproc</code></td>
    <td>End the procedure.</td>
  </tr>
</table>

The above code allows you to make a messaging API call.  Example: `sendmsg(2,12)` will clear the screen.

### Messaging API calls with parameters in NeoBASIC

Since most functions need parameters, you need to specify those in the appropriate memory addresses before making the messaging API call.

```basic
x1=100,y1=100,x2=200,y2=200
doke $FF04
```

## API Commands/Functions

*Grouped by functionality in the tables.*

**Note:**   That these are referring to a mapping to memory locations. The numbers [0..7] represent offsets from the parameters base address $FF04.  The actual bytes are not necessarily all distinct "parameters", depending on the function or routine, a parameter may be an individual byte; one or more bits of a byte interpreted as a composite or bit-field; or multiple adjacent bytes interpreted as 16 or 32 bit values.

**For example**:   The list Param[0,1] would indicate a single logical parameter, comprised of the two adjacent bytes FF04 and FF05. The range Params[4..7] would indicate a single logical parameter, spanning consecutive bytes between FF08 and FF0B.

An integer like “320” would be represented as FF04 = 40, FF05= 01.

**Note:   Strings referenced by parameters are not ASCIIZ, but are length-prefixed.**  The first byte represents the length of the string (not counting itself). The string begins at the second byte.  Consequently, strings must be 255 bytes or less (not counting the length header).

## System

| **G,F** | **Function**               | **Description and Example**                                                                                                                                                                                                                                                                             |
|:-------:| -------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1,0     | DSP Reset                  | Resets the messaging system and component systems.<br> Normally, should not be used.                                                                                                                                                                                                                    |
| 1,1     | Timer                      | Deposit the value (32-bits) of the 100Hz system timer into<br> parameters:0..3.                                                                                                                                                                                                                         |
| 1,2     | Key Status                 | Deposits the state of the specified keyboard key<br> into Parameter:0.<br><br> State of keyboard modifiers (Shift/Ctrl/Alt/Meta) is returned in Parameter:1. The key which to query is specified in Parameter:0.                                                                                        |
| 1,3     | Basic                      | Loads and allows the execution of BASIC via an indirect<br> jump through address zero.                                                                                                                                                                                                                  |
| 1,4     | Credits                    | Print the Neo6502 project contributors (stored in<br> flash memory).                                                                                                                                                                                                                                    |
| 1,5     | Serial Status              | Check the serial port to see if there is a data transmission.[[GJ1]](#_msocom_1)                                                                                                                                                                                                                        |
| 1,6     | Locale                     | Set the locale code specified in parameters:0,1 as<br> upper-case ASCII letters.  Parameter:0<br> takes the first letter and Parameter:1 takes the second letter.  <br> For example: French (FR) would require Parameter 0 being 46 and Parameter 1<br> being 52[[JG2]](#_msocom_2) [[GJ3]](#_msocom_3) |
| 1,7     | System Reset               | System Reset. This is a full hardware reset. It resets the<br> RP2040 using the Watchdog timer, and this also resets the 65C02.                                                                                                                                                                         |
| 1,8     | MOS                        | Perform a MOS command.[[GJ4]](#_msocom_4)                                                                                                                                                                                                                                                               |
| 1,10    | Write character to debug   | Writes a single character to the debug port (the UART on<br> the Pico, or stderr on the emulator). This allows maximum flexibility.                                                                                                                                                                     |
| 1,11    | Return Version Information | Reads the current version: major.minor.patch into parameters: 0-2.<br><br> These values are guaranteed to be in the range 0 – 255.                                                                                                                                                                      |

## Console

| **G,F** | **Function**                              | **Description and Example**                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ------- | ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 2,0     | Write Character                           | Console out. *This is a duplicate* *of* function 2,6 *for backward compatibility.*                                                                                                                                                                                                                                                                                                                                                                                                         |
| 2,1     | Read Character                            | Read and remove a key press from the keyboard queue into<br> Parameter:0.  This is the ASCII value<br> of the keystroke.  If there are no key<br> presses in the queue, Parameter:0 will be zero. **Note:** This function is better suited for text<br> input, but not for games.  See function<br> 7,1 (read default controller) is<br> better suited for games, as this only detects key presses. It does not<br> include the ability to check whether the key is currently down or not. |
| 2,2     | Console Status                            | Check to see if the keyboard queue is empty. If it<br> is, Parameter:0 will be FF, otherwise it will be 00                                                                                                                                                                                                                                                                                                                                                                                 |
| 2,3     | Read Line                                 | Input the current line below the cursor into parameters:0,1<br> as a length-prefixed string; and move the cursor to the line below. Handles<br> multiple-line input.                                                                                                                                                                                                                                                                                                                       |
| 2,4     | Define Hotkey                             | Define the function key F1..F10 specified in<br> Parameter:0 as 1..10 to emit the length-prefixed string stored at the memory<br> location specified in parameters:2,3. F11 and F12 cannot currently be defined.                                                                                                                                                                                                                                                                           |
| 2,5     | Define Character                          | Define a font character specified in Parameter:0 within<br> the range of 192..255.  Fill bits 0..5<br> (columns) of parameters:1..7 (rows) with the character bitmap.                                                                                                                                                                                                                                                                                                                      |
| 2,6     | Write Character                           | Write the character specified in Parameter:0 to the<br> console at the cursor position.  Refer to<br> Section "Console Codes" for details.[[GJ5]](#_msocom_5)                                                                                                                                                                                                                                                                                                                              |
| 2,7     | Set Cursor Pos                            | Move the cursor to the screen character cell Parameter:0<br> (X), Parameter:1 (Y).                                                                                                                                                                                                                                                                                                                                                                                                         |
| 2,8     | List Hotkeys                              | Display the current function key definitions                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| 2,9     | Screen Size                               | Returns the console size in characters, in Parameter:0<br> (height) and Parameter:1 (width).                                                                                                                                                                                                                                                                                                                                                                                               |
| 2,10    | Insert Line                               | This is an internal function (inserting a blank<br> line on the console) and is not officially supported.  *It is not recommended for use and maybe<br> obsoleted or the functionality may change.*                                                                                                                                                                                                                                                                                        |
| 2,11    | Delete<br> Line                           | This is<br> an internal function (deletes line from the console) and is not officially<br> supported.  *It is not recommended<br> for use and maybe obsoleted or the functionality may change.*                                                                                                                                                                                                                                                                                            |
| 2,12    | Clear Screen                              | Clears the screen.  Equivalent to the “cls” BASIC command.                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| 2,13    | Get Cursor Position                       | Returns the current screen character cell of the cursor in<br> Parameter:0 (X), Parameter:1 (Y).                                                                                                                                                                                                                                                                                                                                                                                           |
| 2,14    | Clear Text Region                         | Erase all characters within the rectangular region<br> specified in parameters:0,1 (begin X,Y) and parameters:2,3 (end X,Y).                                                                                                                                                                                                                                                                                                                                                               |
| 2,15    | Set Text Color                            | Sets the foreground color to Parameter:0 and the<br> background color to Parameter:1                                                                                                                                                                                                                                                                                                                                                                                                       |
| 2,16    | Cursor Inverse                            | This is an internal function (inverts/swaps the<br> foreground and background colors – normal -vs- inverse) and is not officially<br> supported.  *It is not recommended<br> for use and maybe obsoleted or the functionality may change.*                                                                                                                                                                                                                                                 |
| 2,17    | Tab                                       | Moves<br> the cursor to the right until it reaches the position in Parameter 0.  This is an internal helper function.  *It is not recommended for use and maybe<br> obsoleted or the functionality may change.*                                                                                                                                                                                                                                                                            |
| 2,18    | Read foreground and background<br> colors | Read the foreground and<br> background RGB colors into Param[0] and Param[1][[JG6]](#_msocom_6)                                                                                                                                                                                                                                                                                                                                                                                            |
| 2,19    | Show/Hide Cursor Reversing                | Set the cursor visibility to Param[0]. This is reset by<br> clearing the screen.                                                                                                                                                                                                                                                                                                                                                                                                           |

## File I/O

| **G,F** | **Function**            | **Description and Example**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ------- | ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 3,1     | List Directory          | Display the file listing of the present directory.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| 3,2     | Load File               | Load a file by name into memory. On input: parameters:0,1 points to the length-prefixed filename<br> string; parameters:2,3 contains the location to write the data to.<br> If the address is $FFFF, the file will instead be loaded into the graphics<br> working memory, used for sprites, tiles, images. On output: Error location contains an error/status code.                                                                                                                                                                                                                                                                                                                                                   |
| 3,3     | Store File              | Saves data in memory to a file. On input: parameters:0,1 points to the length-prefixed<br> filename string; parameters:2,3 contains the location to read data<br> from; parameters:4,5 specified the number of bytes to<br> store. On output: Error location contains an error/status code.                                                                                                                                                                                                                                                                                                                                                                                                                            |
| 3,4     | File Open               | Opens a file into a specific channel. On input: Parameter:0 contains the file channel to open; parameters:1,2 points to the length-prefixed filename<br> string; Parameter:3 contains the open mode. See below. Valid open modes are: 0 opens the file for read-only access; 1 opens the file for write-only access; 2 opens the file for read-write access; 3 creates the file if it doesn't already exist, truncates<br> it if it does, and opens the file for read-write access. Modes 0 to 2 will fail if the file does not already exist.<br> If the channel is already open, the call fails. Opening the same file more<br> than once on different channels has undefined behaviour, and is not<br> recommended. |
| 3,5     | File Close              | Closes a particular channel. On input: Parameter:0 contains the file channel to close. If<br> this is $FF this closes all open files.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| 3,6     | File Seek               | Seeks the file opened on a particular channel to a<br> location. On input: Parameter:0 contains the file channel to operate on; parameters:1..4 contains the file location. You can seek beyond the end of a file to extend the file.<br> However, whether the file size changes when the seek happens, or when you<br> perform the write is undefined behavior.                                                                                                                                                                                                                                                                                                                                                       |
| 3,7     | File Tell               | Returns the current seek location for the file<br> opened on a particular channel. On input: Parameter:0 contains the file channel to operate<br> on. On output: parameters:1..4 contains the seek location within<br> the file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 3,8     | File Read               | Reads data from an opened file. On input: Parameter:0 contains the file channel to operate on. parameters:1,2 points to the destination in memory, or $FFFF to read into graphics memory. parameters:3,4 contains the amount of data to read. On output: parameters:3,4 is updated to contain the amount of data<br> actually read. Data is read from the current seek position, which is<br> advanced after the read.                                                                                                                                                                                                                                                                                                 |
| 3,9     | File Write              | Writes data to an opened file. On input: Parameter:0 contains the file channel to operate<br> on; parameters:1,2 points to the data in memory; parameters:3,4 contains the amount of data to<br> write. On output: parameters:3,4 is updated to contain the amount of<br> data actually written. Data is written to the current seek position, which<br> is advanced after the write.                                                                                                                                                                                                                                                                                                                                  |
| 3,10    | File Size               | Returns the current size of an opened file. On input: Parameter:0 contains the file channel to operate on. On output: parameters:1..4 contains the size of the file. This call should be used on open files, and takes into<br> account any buffered data which has not yet been written to disk.<br> Consequently, this may return a different size than Function 3,16 "File<br> Stat".                                                                                                                                                                                                                                                                                                                               |
| 3,11    | File Set Size           | Extends or truncates an opened file to a particular<br> size. On input: Parameter:0 contains the file channel to operate<br> on; parameters:1..4 contains the new size of the file.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| 3,12    | File Rename             | Renames a file. On input: parameters:0,1 points to the length-prefixed string for<br> the old name; parameters:2,3 points to the length-prefixed string for<br> the new name. Files may be renamed across directories.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| 3,13    | File Delete             | Deletes a file or directory. On input: parameters:0,1 points to the length-prefixed<br> filename string. Deleting a file which is open has undefined<br> behavior. Directories may only be deleted if they are empty.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| 3,14    | Create Directory        | Creates a new directory. On input: parameters:0,1 points to the length-prefixed filename<br> string.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| 3,15    | Change Directory        | Changes the current working directory. On input: parameters:0,1 points to the length-prefixed path<br> string.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 3,16    | File Stat               | Retrieves information about a file by name. On input: parameters:0,1 points to the length-prefixed filename<br> string. parameters:0..3 contains the length of the file; Parameter:4 contains the attributes bit-field of the file. If the file is open for writing, this may not return the<br> correct size due to buffered data not having been flushed to disk. File attributes are a bitfield as follows: 0,0,0,Hidden,<br> Read Only, Archive, System, Directory.                                                                                                                                                                                                                                                |
| 3,17    | Open Directory          | Opens a directory for enumeration. On input: parameters:0,1 points to the length-prefixed<br> filename string. Only one directory at a time may be opened. If a<br> directory is already open when this call is made, it is automatically closed.<br> However, an open directory may make it impossible to delete the directory; so<br> closing the directory after use is good practice.                                                                                                                                                                                                                                                                                                                              |
| 3,18    | Read Directory          | Reads an item from the currently open directory. On input: parameters:0,1 points to a length-prefixed buffer for<br> returning the filename. parameters:0,1 is unchanged, but the buffer is updated to<br> contain the length-prefixed filename (without any leading path); parameters:2..5 contains the length of the file; Parameter:6 contains the file attributes, as described by<br> Function 3,16 "File Stat". If there are no more items to read, this call fails and an<br> error is flagged.                                                                                                                                                                                                                 |
| 3,19    | Close Directory         | Closes any directory opened previously by Function<br> 3,17 "Open Directory".                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| 3,20    | Copy File               | Copies a file. On input: parameters:0,1 points to the length-prefixed old filename; parameters:2,3 points to the length-prefixed new filename. Only single files may be copied, not directories.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 3,21    | Set File Attributes     | Sets the attributes for a file. On input: parameters:0,1 points to the length-prefixed<br> filename; Parameter:2 is the attribute bitfield. (See Stat<br> File for details.) The directory bit cannot be changed. Obviously.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 3,22    | Check End of File (EOF) | Returns the end of file status of an opened file. On<br> input: Parameter:0 contains the file channel to operate on. On output: Parameter:0 is non-zero if the file is at the end of the<br> file. This call should be used on open files and may return an<br> error if the file is closed.                                                                                                                                                                                                                                                                                                                                                                                                                           |
| 3,32    | List Filtered           | Prints a filtered file listing of the current<br> directory to the console. On input: parameters:0,1 points to the filename search<br> string. Files will only be shown if the name contains the<br> search string (ie: a substring match).                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

## Mathematics

The
mathematical interface of the API functions largely as a helper system for the
BASIC interpreted, but it is open to any developer who wishes to avail
themselves of the functionality.

The interface is
used in a stack environment but is designed so it could be used in either a
stack environment or a fixed location environment. The Neo6502 BASIC stack is
also 'split', so elements are not consecutive, though they can be.

Parameter 0 and 1
specify the address of the registers 1 and 2. Register 1 starts at this
address, Register 2 starts at the next address. Parameter 2 specifies the step
to the next register. Therefore they are interleaved by default at present.

So if parameters 0
and 1 are 8100 and Parameter 2 is 4, the 5 byte registers are

Register 1:
8100,8104,8108,810C,8110

Register 2:
8101,8105,8109,810D,8111

Bytes 1-4 of the
'register' are the number, which can be either an integer (32 bit signed) or a
standard 'C' float (e.g. the IEEE Single Precision Float format). Bit 0 is the
type byte, and the relevant bit is bit 6, which is set to indicate bytes 1-4 are
a float value, and is set on return.

Binary functions
that use int and float combined (one is int and one is float) normally return a
float.[[GJ7]](#_msocom_7) 

| **G,F** | **Function**                     | **Description and Example**                                                                         |
| ------- | -------------------------------- | --------------------------------------------------------------------------------------------------- |
| 4,0     | Addition                         | Register1<br> := Register 1 + Register2                                                             |
| 4,1     | Subtraction                      | Register1 := Register 1 -<br> Register2                                                             |
| 4,2     | Multiplication                   | Register1<br> := Register 1 * Register2                                                             |
| 4,3     | Decimal Division                 | Register1 := Register 1 /<br> Register2 (floating point)                                            |
| 4,4     | Integer<br> Division             | Register1<br> := Register 1 / Register2 (integer result)                                            |
| 4,5     | Integer Modulus                  | Register1 := Register 1 mod<br> Register2                                                           |
| 4,6     | Compare                          | Parameter:0<br> := Register 1 compare Register2 : returns $FF, 0, 1 for less equal and<br> greater. |
| 4,7     | Power                            | Register1 := Register 1 to the<br> power of Register2 (floating point result whatever)              |
| 4,8     | Distance<br> (counter-rectangle) | Register1<br> := Square root of (Register1 * Register1) + (Register2 * Register2)                   |
| 4,9     | Angle calculation (arctangent2)  | Register1 := arctangent2(Register<br> 1,Register 2) - angle in degrees/radians                      |
| 4,16    | Negate                           | Register1<br> := -Register 1                                                                        |
| 4,17    | Floor                            | Register1 := floor(Register 1)                                                                      |
| 4,18    | Square<br> Root                  | Register1<br> := square root(Register 1)                                                            |
| 4,19    | Sine                             | Register1 := sine(Register 1)<br> angles in degrees/radians                                         |
| 4,20    | Cosine                           | Register1<br> := cosine(Register 1) angles in degrees/radians                                       |
| 4,21    | Tangent                          | Register1 := tangent(Register 1)<br> angles in degrees/radians                                      |
| 4,22    | Arctangent                       | Register1<br> := arctangent(Register 1) angles in degrees/radians                                   |
| 4,23    | Exponent                         | Register1 := e to the power of<br> Register 1                                                       |
| 4,24    | Logarithm                        | Register1<br> := log(Register 1) natural logarithm                                                  |
| 4,25    | Absolute Value                   | Register1 := absolute<br> value(Register 1)                                                         |
| 4,26    | Sign                             | Register1<br> := sign(Register 1), returns -1 0 or 1                                                |
| 4,27    | Random Decimal                   | Register1 := random float from<br> 0-1                                                              |
| 4,28    | Random<br> Integer               | Register1<br> := random integer from 0 to (Register 1-1)                                            |
| 4,32    | Number to Decimal                | Helper function for tokenizer,<br> do not use.                                                      |
| 4,33    | String<br> to Number             | Convert<br> the length prefixed string at parameters:4,5 to a constant in Register1.                |
| 4,34    | Number to String                 | Convert the constant in<br> Register1 to a length prefixed string which is stored at parameters:4,5 |
| 4,35    | Set<br> Degree/Radian Mode       | Sets<br> the use of degrees (the default) when non zero, radians when zero.                         |

## Graphics

| **G,F** | **Function**      | **Description and Example**                                                                                                                                                                                                                                                                                                                                                       |
| ------- | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 5,1     | Set Defaults.     | Configure the global graphics system settings. Not<br> all parameters are relevant for all graphics commands; but all parameters<br> will be set by this command. So mind their values.  Refer to Section "Graphics Settings"<br> for details. The parameters are And, Or, Fill Flag, Extent, and Flip. Bit 0<br> of flip sets the horizontal flip, Bit 1 sets the vertical flip. |
| 5,2     | Draw Line         | Draw a line between the screen coordinates specified in parameters:0,1,parameters:2,3<br> (begin X,Y) and parameters:4,5,parameters:6,7 (end X,Y).                                                                                                                                                                                                                                |
| 5,3     | Draw Rectangle    | Draw a rectangle spanning the screen coordinates<br> specified in parameters:0,1,parameters:2,3 (corner X,Y) and parameters:4,5,parameters:6,7<br> (opposite corner X,Y).                                                                                                                                                                                                         |
| 5,4     | Draw Ellipse      | Draw an ellipse spanning the screen coordinates specified<br> in parameters:0,1,parameters:2,3 (corner X,Y) and parameters:4,5,parameters:6,7<br> (opposite corner X,Y).                                                                                                                                                                                                          |
| 5,5     | Draw Pixel        | Draw a single pixel at the screen coordinates<br> specified in parameters:0,1,parameters:2,3 (X,Y).                                                                                                                                                                                                                                                                               |
| 5,6     | Draw Text         | Draw the length-prefixed string of text stored at the<br> memory location specified in parameters:4,5 at the screen character cell<br> specified in parameters:0,1,parameters:2,3 (X,Y).                                                                                                                                                                                          |
| 5,7     | Draw Image        | Draw the image with image ID in Parameter:4 at the<br> screen coordinates parameters:0,1,parameters:2,3 (X,Y). The extent and flip<br> settings influence this command.                                                                                                                                                                                                           |
| 5,8     | Draw Tilemap      | Draw the current tilemap at the screen coordinates<br> specified in parameters:0,1,parameters:2,3 (top-left X,Y) and parameters:4,5,parameters:6,7<br> (bottom-right X,Y) using current graphics settings.                                                                                                                                                                        |
| 5,32    | Set Palette       | Set the palette colour at the index spcified in<br> Parameter:0 to the values in Parameter:1,Parameter:2,Parameter:3 (RGB).                                                                                                                                                                                                                                                       |
| 5,33    | Read Pixel        | Read a single pixel at the screen coordinates specified in<br> parameters:0,1,parameters:2,3 (X,Y). When the routine completes, the result<br> will be in Parameter:0. If sprites are in use, this will be the background<br> only (0..15), if sprites are not in use it may return (0..255)                                                                                      |
| 5,34    | Reset Palette     | Reset the palette to the defaults.                                                                                                                                                                                                                                                                                                                                                |
| 5,35    | Set Tilemap       | Set the current tilemap. parameters:0,1 is the memory<br> address of the tilemap, and parameters:2,3,parameters:4,5 (X,Y) specifies the<br> offset into the tilemap, in units of pixels, of the top-left pixel of the<br> tile.                                                                                                                                                   |
| 5,36    | Read Sprite Pixel | Read Pixel from the sprite layer at the screen<br> coordinates specified in parameters:0,1,parameters:2,3 (X,Y).  When the routine completes, the result will<br> be in Parameter:0.  Refer to Section<br> "Pixel Colors" for details.                                                                                                                                            |
| 5,37    | Frame Count       | Deposit into parameters:0..3, the number of v-blanks (full<br> screen redraws) which have occurred since power-on. This is updated at the<br> start of each v-blank period.                                                                                                                                                                                                       |
| 5,38    | Get Palette       | Get the palette colour at the index spcified in<br> Parameter:0. Values are returned in Parameter:1,Parameter:2,Parameter:3<br> (RGB).                                                                                                                                                                                                                                            |
| 5,39    | Write Pixel       | Write Pixel index Parameter:4 to the screen coordinate<br> specified in parameters:0,1,parameters:2,3 (X,Y).                                                                                                                                                                                                                                                                      |
| 5,64    | Set Color         | Set Color. Sets the current drawing colour to<br> Parameter:0                                                                                                                                                                                                                                                                                                                     |
| 5,65    | Set Solid Flag    | Set Solid Flag. Sets the solid flag to Parameter:0, which<br> indicates either solid fill (for shapes) or solid background (for images and<br> fonts)                                                                                                                                                                                                                             |
| 5,66    | Set Draw Size     | Set Draw Size. Sets the drawing scale for images<br> and fonts to Parameter:0                                                                                                                                                                                                                                                                                                     |
| 5,67    | Set Flip Bits     | Set Flip Bits.  Sets<br> the flip bits for drawing images. Bit 0 set causes a horizontal flip, bit 1<br> set causes a vertical flip.                                                                                                                                                                                                                                              |

## Sprites

| **G,F** | **Function**      | **Description and Example**                                                                                                                                                                                                                            |
| ------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 6,1     | Sprite Reset      | Reset the sprite system.                                                                                                                                                                                                                               |
| 6,2     | Sprite Set        | Set or update the sprite specified in Parameter:0.  The parameters are : Sprite Number, X Low,<br> X High, Y Low, Y High, Image, Flip and Anchor and Flags.  Bit 0 of flags specifies 32 bit sprites.  Values that are 80 or 8080 are not<br> updated. |
| 6,3     | Sprite Hide.      | Hide the sprite specified in Parameter:0.                                                                                                                                                                                                              |
| 6,4     | Sprite Collision. | Parameter:0 is non-zero if the distance is less than or<br> equal to Parameter:2 between the center of the sprite with index specified in<br> Parameter:0 and the center of the sprite with index specified in Parameter:1.                            |
| 6,5     | Sprite Position   | Deposit into parameters:1..4, the screen<br> coordinates of the sprite with the index specified in Parameter:0.                                                                                                                                        |

## Controller

| **G,F** | **Function**            | **Description and Example**                                                                                                                                                                                                                                                                                                                                     |
| ------- | ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 7,1     | Read Default Controller | This reads the status of the base controller into<br> Parameter:0, and is a compatibility API call.  The base controller is the keyboard keys (these are WASD+OPKL or Arrow<br> Keys+ZXCV) or the gamepad controller buttons. Either works.  The 8 bits of the returned byte are the<br> following buttons, most significant first : Y X B A Down Up Right Left |
| 7,2     | Read Controller Count   | This returns the number of game controllers plugged in to<br> the USB System into Parameter:0. This does not include the keyboard based<br> controller, only physical controller hardware.                                                                                                                                                                      |
| 7,3     | Read Controller         | This returns a specific controller status. Controller<br> 0 is the keyboard controller, Controllers 1 upwards are those physical USB<br> devices.                                                                                                                                                                                                               |

## Sound

| **G,F** | **Function**         | **Description and Example**                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ------- | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 8,1     | Reset Sound          | Reset the sound system. This empties all channel<br> queues and silences all channels immediately.                                                                                                                                                                                                                                                                                                                                                    |
| 8,2     | Reset Channel        | Reset the sound channel specified in Parameter:0.                                                                                                                                                                                                                                                                                                                                                                                                     |
| 8,3     | Beep                 | Play the startup beep immediately.                                                                                                                                                                                                                                                                                                                                                                                                                    |
| 8,4     | Queue Sound          | Queue a sound. Refer to Section #\ref{sound}<br> "Sound" for details.  The<br> parameters are : Channel, Frequency Low, Frequency High, Duration Low,<br> Duration High, Slide Low, Slide High and Source.                                                                                                                                                                                                                                            |
| 8,5     | Play Sound           | Play the sound effect specified in Parameter:1 on<br> the channel specified in Parameter:0 immediately, clearing the channel queue.                                                                                                                                                                                                                                                                                                                   |
| 8,6     | Sound Status         | Deposit in Parameter:0 the number of notes outstanding<br> before silence in the queue of the channel specified in Parameter:0,<br> including the current playing sound, if any.                                                                                                                                                                                                                                                                      |
| 8,7     | Queue Sound Extended | Queue a sound. Refer to Section #\ref{sound}<br> "Sound" for details. This is an extension of call 4 to support<br> different waveform types and volumes. The source parameter is no longer used.<br> The parameters are : Channel, Frequency Low, Frequency High, Duration Low,<br> Duration High, Slide Low, Slide High, Sound Type and Sound Volume. All these are<br> 16 bit parameters except the sound type and volume, and the channel number. |
| 8,8     | Get Channel Count    | This returns the number of channels in Parameter #0                                                                                                                                                                                                                                                                                                                                                                                                   |

## Turtle Graphics

| **G,F** | **Function**      | **Description and Example**                                                                                                                                                                                                                                                                                         |
| ------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 9,1     | Turtle Initialize | Initialize the turtle graphics system.  Parameter:0 is the sprite number to use for<br> the turtle, as the turtle graphics system “adopts” one of the sprites.  The icon is not currently re-definable, and<br> initially the turtle is hidden.                                                                     |
| 9,2     | Turtle Turn       | Turn the turtle right by Parameter:0,1 degrees. Show if<br> hidden. To turn left, turn by a negative amount.<br><br> <br><br> API_TURTLE_LEFT  = #010E; API parameter (turn -90 degrees) API_TURTLE_RIGHT<br> = #005A; API parameter (turn +90 degrees) API_TURTLE_FLIP  = #$00B4; API parameter (turn 180 degrees) |
| 9,3     | Turtle Move       | Move the turtle forward by Parameter:0,1 degrees,<br> drawing in colour Parameter:2 if Parameter:3 is non-zero.                                                                                                                                                                                                     |
| 9,4     | Turtle Hide       | Hide the turtle.                                                                                                                                                                                                                                                                                                    |
| 9,5     | Turtle Home       | Move the turtle to the home position (in the<br> center, pointing upward).                                                                                                                                                                                                                                          |
| 9,6     | Turtle Show       | Show the turtle.                                                                                                                                                                                                                                                                                                    |

## UEXT port I/O

| **G,F** | **Function**       | **Description and Example**                                                                                                                                                                                                                                       |
| ------- | ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 10,1    | UExt Initialize    | Initialise the UExt I/O system. This resets the IO<br> system to its default state, where all UEXT pins are I/O pins, inputs and<br> enabled.                                                                                                                     |
| 10,2    | Write GPIO         | This copies the value Parameter:1 to the output latch for<br> UEXT pin Parameter:0. This will only display on the output pin if it is<br> enabled, and its direction is set to "Output" direction.                                                                |
| 10,3    | Read GPIO          | If the pin is set to "Input" direction,<br> reads the level on pin on UEXT port Parameter:0.  If it is set to "Output"<br> direction, reads the output latch for pin on UEXT port Parameter:0.  If the read is successful, the result will<br> be in Parameter:0. |
| 10,4    | Set Port Direction | Set the port direction for UEXT Port Parameter:0 to the<br> value in Parameter:1.  This can be 01<br> (Input), 02 (Output), or $03 (Analogue Input).                                                                                                              |
| 10,5    | Write I2C          | Write to I2C Device Parameter:0, Register<br> Parameter:1, value Parameter:2. No error is flagged if the device is not<br> present.                                                                                                                               |
| 10,6    | Read I2C           | Read from I2C Device Parameter:0, Register<br> Parameter:1.  If the read is<br> successful, the result will be in Parameter:0.  If the device is not present, this will<br> flag an error. Use FUNCTION 10,2 first, to check for its presence.                    |
| 10,7    | Read Analog        | Read the analogue value on UEXT Pin<br> Parameter:0.  This has to be set to<br> analog type to work.  Returns a value<br> from 0..4095 stored in parameters:0,1, which represents an input value of 0<br> to 3.3 volts.                                           |
| 10,8    | I2C Status         | Try to read from I2C Device Parameter:0.  If present, then Parameter:0 will contain a<br> non-zero value.                                                                                                                                                         |
| 10,9    | Read I2C Block     | Try to read a block of memory from I2C Device<br> Parameter:0 into memory at parameters:1,2, length parameters:3,4.                                                                                                                                               |
| 10,10   | Write I2C Block    | Try to write a block of memory to I2C Device Parameter:0<br> from memory at parameters:1,2, length parameters:3,4.                                                                                                                                                |
| 10,11   | Read SPI Block     | Try to read a block of memory from SPI Device into<br> memory at parameters:1,2, length parameters:3,4.                                                                                                                                                           |
| 10,12   | Write SPI Block    | Try to write a block of memory to SPI Device from memory<br> at parameters:1,2, length parameters:3,4.                                                                                                                                                            |

| 10,13 | Read UART Block             | Try to read a block of memory from  <br>UART into memory at parameters:1,2, length parameters:3,4. This can fail with  <br>a timeout.                                                                  |
| ----- | --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 10,14 | Write UART Block            | Try to write a block of memory to UART from memory<br> at parameters:1,2, length parameters:3,4.                                                                                                       |
| 10,15 | Set UART Speed and Protocol | Set the Baud Rate and Serial Protocol for the UART<br> interface. The baud rate is in parameters:0..3 and the protocol number is<br> Parameter:4. Currently only 8N1 is supported, this is protocol 0. |
| 10,16 | Write byte to UART          | Write byte Parameter:0 to the UART                                                                                                                                                                     |
| 10,17 | Read byte from UART         | Read a byte from the UART. It is returned in Parameter:0                                                                                                                                               |
| 10,18 | Check if Byte Available     | See if a byte is available in the UART input<br> buffer. If available Parameter:0 is non zero.                                                                                                         |

## Mouse

| **G,F** | **Function**                     | **Description and Example**                                                                                                                                                                                                                                                  |
| ------- | -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 11,1    | Move display cursor              | Positions the display cursor at parameters:0,1,parameters:2,3                                                                                                                                                                                                                |
| 11,2    | Set mouse display cursor on/off. | Shows or hides the mouse cursor depending on the<br> Parameter:0                                                                                                                                                                                                             |
| 11,3    | Get mouse state                  | Returns the mouse position (screen pixel, unsigned)<br> in x parameters:0,1 and y parameters:2,3, button state in Parameter:4 (button<br> 1 is 0x1, button 2 0x2 etc., set when pressed), scroll wheel state in<br> Parameter:5 as uint8 which changes according to scrolls. |
| 11,4    | Test mouse present               | Returns non zero if a mouse is plugged in in Parameter:0                                                                                                                                                                                                                     |
| 11,5    | Select mouse Cursor              | Select a mouse cursor in Parameter:0 ; returns<br> error status if the cursor is not available.                                                                                                                                                                              |

## Blitter

<table width="100%" cellpadding="5">
  <tr>
    <th width="10%">G,F</th>
    <th width="20%">Function</th>
    <th width="70%">Description and Example</th>
  </tr>
  <tr>
    <td>12,1</td>
    <td>Blitter Busy</td>
    <td>
      Returns a non zero value in Parameter:0 if the blitter/DMA system is currently 
      transferring data, used to check availability and transfer completion.
    </td>
  </tr>
  <tr>
    <td>12,2</td>
    <td>Simple Blit Copy</td>
    <td>
      Copy parameters:6,7 bytes of internal memory from
      Parameter:0:parameters:1,2 to Parameter:3:parameters:4,5. Sets error flag if the transfer is 
      not possible (e.g. illegal write addresses). The upper 8 bits of the address are: <ul>
      <li>6502 RAM (00)</li><li>Video RAM (80,81)</li><li>Graphics RAM (90)</li></ul>
    </td>
  </tr>
  <tr>
    <td>12,3</td>
    <td>Complex Blit Copy</td>
    <td>
        Copy a source rectangular area to a destination rectangular area.  It's oriented toward copying 
        graphics data, but can be used as a more general-purpose memory mover.  The source and target 
        areas may be different formats, and the copy will convert the data on the fly.  For example, 
        you can expand 4bpp source graphics (two pixels per byte) into the 1 pixel per byte framebuffer.
        However, the blitting is byte-oriented. So the source width is always rounded down to the 
        nearest full byte.

        Parameter (0) is the blit action:  
          <table width="100%">
                 <tr>
                     <td>0</td><td>Copy</td>
                 </tr>
                 <tr>
                     <td>1</td><td>Copy masked - copy, but only where src is not the transparent value.</td>
                 </tr>
                 <tr>
                     <td>2</td><td>Solid masked - set target to constant solid value, but only where src is not the transparent value.</td>
                 </tr>
            </table>    
    
    
    </td>

</tr>
</table>
