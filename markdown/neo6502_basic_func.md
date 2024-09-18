<img title="" src="file:///Users/jewettg/GitHub/Neo6502-Documentation/markdown/images/neobasic/NEO%20BASIC_logo_final.png" alt="">

*Written by Paul Robson*

NeoBASIC runs very fast compared to most 6502-based home computers back in the day that were 2 Mhz or less.  Compared to the FPGA-assisted Commander X16 that runs at 8 Mhz, it underperforms versus the Neo6502 in this BASIC line draw demo, partially due to the inefficient BASIC interpreter.

You can program right on the Neo6502 using NeoBASIC or use the emulators that will run on Windows, Linux, and MacOS desktop operating systems. Cross-development is possible even with NeoBASIC by using the Python script to tokenize (text to BASIC binary) or detokenize (BASIC binary to text) your basic listing ready for execution.

There are some NeoBASIC based games that have been written that really show off some of the very powerful features:

<table width="100%">
  <tr>
    <td>
      <li><b>Galaxians</b> arcade game</li>
      <li><b>Robotron 2048</b></li>
      <li><b>Tetris</b></li>
      <li><b>Space Invaders</b> arcade game</li>
    </td>
    <td>
        <li><b>Frogger</b> arcade game</li>
        <li><b>PacMad</b> (PacMan like game)</li>
        <li><b>Solitaire Suite</b> of card games<br>
          <div align="right"><i>...and some many more!</i></div></li>
    </td>
  </tr>
</table>

----

# Programming Reference and Technical Documentation

The information that follows describes the NeoBASIC interpreter.  It is for experienced programmers and includes explanations of all BASIC statements and functions, memory tables, sprite usage, and file formats.

This version of BASIC was written with concepts brought over from more modern BASIC interpreters and other programming languages that make it more powerful and create a lot less spaghetti code[<sup>1</sup>](#footnotes) that BASIC programs generally tended to create.

## MOS -vs- mos

There are several locations in the guide where the acronym “mos” is used.  To help ensure that the correct acronym expansion is interpreted correctly, here is a key:

- **MOS** (all uppercase letters) = Machine Operating System.

- **mos** (all lowercase letters) = metal oxide semiconductor.

Also note, that the command “mos” can be both “MOS” and “mos” and the command comes from the uppercase “MOS” (Machine Operating System).

## Immediate -vs- Deferred Execution Commands

Many, but not all commands can be executed immediately by just typing the command at the beginning of a line and pressing return.

All commands (with a few exceptions) can be used within a program and can be preceded with a line number to add it to a program.   Execute those commands as part of the program by typing "run ↩︎”, and to see a listing of your program type “list ↩︎ “.

## Line Numbers

In NeoBASIC, line numbers are only used to provide an order to the program where declaration order is important.   Line numbers should not be used as a reference within the program, and while GOSUB and GOTO are valid BASIC commands, their use is highly discouraged. It is recommended that a main program exist with functions and procedures used along with proper use of “for”, “while” and “repeat” flow control programming syntax.

<table border="0" cellpadding="5" width="100%">
<tr>
<td width="10%" align="center" vertical-align="middle">
   <img src="images/icons/eye-watch.png" width="30">
</td>
<td width="90%">
<b>Note:</b> If a program is imported or created and contains the use of GOSUB or GOTO with reference to line numbers, the program will stop working properly should the program get renumbered.   A program will get renumbered if the built-in commands "renumber" or "library" are used; or the result of editing a program with the built-in editor.  
</td>
</tr>
</table>

## Procedure Placement

```
10 print “Say Something”
20 print “-----------------------” 
25 a$=”Something to say!” 
30 call saysomething(a$) **
40 end
50 proc saysomething(a$) 
60    print a$ 
70 endproc
```

Procedure declarations should be at the end of a program, with a preceding “end” statement before the first “proc” statement.  Should NeoBASIC encounter a proc statement during execution, the program will halt and present a syntax error.

## Comments

- A code should include comments.   Good comments don't repeat the code or explain it. They clarify its intent. Comments should explain, at a higher level of abstraction than the code, what you're trying to do.

- Comments can be added to code as a line by itself or added to the end of an existing line by using the single quote following by your comment.   Comments should be limited to alphanumeric, and caution should be used when using quotes and other special characters.

- Note, after entering a comment, the BASIC tokenizer will display it as a single quote followed by the comment within double quotes.   A comment is basically an ignored string at execution.

----

## Binary Operators

If you remember “PEMDAS” (parenthesis, exponents, multiplication, division, addition, subtraction) or "Please Excuse My Dear Aunt Sally" from grade school, this table is an expansion of that.

| **Precedence** | **Operator** | **Notes**                                                |
| -------------- | ------------ | -------------------------------------------------------- |
| 4              | *            | Multiplication operator                                  |
| 4              | /            | Forward slash is floating point divide. 22/7 is 3.142857 |
| 4              | \            | Backward slash is integer divide, 22\7 is 3              |
| 4              | %            | Modulus of integer division ignoring signs               |
| 4              | >>           | Logical shift right, highest bit zero                    |
| 4              | <<           | Logical shift left                                       |
| 3              | +            | Addition operator                                        |
| 3              | -            | Subtraction operator                                     |
| 2              | <            | Less than                                                |
| 2              | <=           | Less than or equal                                       |
| 2              | >            | Great than                                               |
| 2              | >=           | Greater than or equal                                    |
| 2              | <>           | Not equal to                                             |
| 2              | =            | Equal to                                                 |
| 1              | &            | Binary AND operator on integers                          |
| 1              | \|           | Binary OR operator on integers                           |
| 1              | ^            | Binary XOR operator on integers                          |

In the above table, an expression using the above operators will evaluate  as -1 for true, and 0 for false.

**Example:**

`print (1+2) > (1+1)`

**Result:** `-1` *(true)*

`print (1*3) = (2\4)`

**Result:** `0` *(false)*

### Flow Control Commands

| **Control Structure** <img src="images/empty.gif" width="300" height="1">                                                         | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| --------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `do`  ... <br>  <br>`loop`                                                                                                        | Provides an infinite loop structure, with the ability to use the exit command to break out of the loop at any point.  <br> **Example:** <pre>10 index = 0 <br/>20 **do** <br/>30   print “Hello World!” <br/>40   index = index + 1 <br/>50   if index > 10 then **exit** <br/>60 **loop**</pre><br/>An optional `exit` keyword can be used, allowing you to exit or break out of the loop.                                                                                                                                                                                                                                            |
| `end`                                                                                                                             | End the current running program  <br> *When defining procedures, ensure that the “end” precedes any of the procedure definitions so program execution to not continue into the procedure declaration section, else a syntax error will result.*                                                                                                                                                                                                                                                                                                                                                                                        |
| `for` *var*` = `*start* `to`*end* ... <br/>`next` *var*<br/><br/><br/>`for` *var*` = `*start* `downto` *end* ... <br/>`next` *var | Provides a controlled loop (for / next) using a range of values.<br><br> *Note this is non-standard for/loop control, as there are limitations.<br>Limitations:<br/><br/><ul><li>The index must be an integer.</li> <br/><li>The step is controlled using “to” (1) or “downto” (-1). </li> <br> next cannot specify an index and cannot be used to terminate loops (*using the wrong index).* Execution must operate in order and flow cannot be stopped arbitrarily.  *The variable after next is ignored.*  <br> **Example:**<pre>10 for count = 10 downto 1 <br/>20   print count<br/>30 next count<br/>40 print “Blast off!”</pre> |
| `gosub` *expr*[<sup>2</sup>](#footnotes)                                                                                          | Call subroutine at line number.  A return command will return control to the line after the gosub call.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `goto` *expr*[<sup>2</sup>](#footnotes)                                                                                           | Transfer execution to a line number.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `return`[<sup>2</sup>](#footnotes)                                                                                                | *See* *gosub.*                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| `if` *expr* `then` *nn*[<sup>2</sup>](#footnotes)                                                                                 | Provide a single conditional, where execution is transferred to line number nn if the {expr} is true. **<br><br> NOTE:** `if` {expr} `goto` *nn* does not work.                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `on error` *nn*[<sup>2</sup>](#footnotes)                                                                                         | Install an error handler, when execution is transferred to line number *nn* when an error occurs (*effectively a goto command*).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

---

## Functions

### Arithmetic and Boolean Functions

| **Function**<img src="images/empty.gif" width="200" height="1"> | **Description** (*function and return value*)                                                                                                                                                                                          |
| --------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `atan(`*n*`)`                                                   | Calculate the arctangent (the inverse tangent function) of n in degrees  <br><br/> **Example****:** <br>`print atan(90)`<br/>`89.363411`                                                                                               |
| `atan2(`*y,x*`)`                                                | Calculates the arctangent (the inverse tangent function) of y,x <br> if x equals 0, atan2 returns π/2<br> if y is positive, -π/2 if y is negative, or 0 if y is 0.<br/><br/>**Example****:** <br>`print atan2(-1, 0)`<br/>`-90.000000` |
| `cos(`*n*`)`                                                    | Calculate the cosine of n (n must be in degrees)<br/><br/>**Example:** <br>`print cos(180)`<br/>`-1.000000`<br/>`print cos(90)`<br/>`-0.000000`                                                                                        |
| `exp(`*n*`)`                                                    | Calculates the exponential value of a floating-point argument n (en, where e equals 2.17128128...)<br/><br/>**Example:**<br/>`print exp(0)`<br/>`1.000000`<br/>`print exp(2)`<br/>`7.389056`                                           |
| `FALSE`                                                         | Return a constant of 0, a known falsey value. The value zero is always consider false.                                                                                                                                                 |
| `int(`*n*`)`                                                    | Return the whole part of the float value n. Integers are unchanged.<br/><br> **Example:** <br>`print int(22/7)` <br/>`3`                                                                                                               |
| `log(`*n*`)`                                                    | Calculate the natural logarithm  of *n*.   <br/><br/>**Example:** <br>`print log(0)`<br/>`-inf`                                                                                                                                        |
| `max(`*a,b*`)`                                                  | Return the largest of a and b (numbers or strings)<br> <br/>**Example:** <br>`print max(50,100)`<br/>`100`<br>`print max(“Hello”, “World”)`<br/>`World`                                                                                |
| `min(`*a,b*`)`                                                  | Return the smallest of a and b (numbers or strings)<br><br>**Example:** <br>`print min(50,100)`<br/>`50`<br/>`print max(“Hello”, “World”)`<br/>`Hello`                                                                                 |
| `pow(`*a,b*`)`                                                  | Returns a raised to the power b; the result is always floating point.<br/><br/>**Example:** <br>`print pow(2,0)`<br/>`1.000000`<br/>`print pow(2,8)`<br/>`256.000000`                                                                  |
| `rand(`*n*`)`                                                   | Returns a random integer, where 0 < x < n.<br> *The value returned will be between* *0 and* *n-1.*<br> **Example:** <br>`print rand(50)`<br/>`33`                                                                                      |
| `rnd(0)`                                                        | Returns a random number where 0 < x < 1.<br>*The value (**zero) passed is ignored.*<br><br/> **Example:** <br>`print rand(0)`<br/>`0.179750`                                                                                           |
| `sqr(`*n*`)`                                                    | Calculates the square root of *n*<br/><br/>**Example****:** <br>`print sqr(9)`<br/>`3.000000` <br/>`print sqr(64)`<br/>`8.000000`                                                                                                      |
| `tan(`*n*`)`                                                    | Calculates the tangent of n (n must be in degrees)   <br/><br/>**Example****:** <br>`print tan(45)`<br/>`1.000000` <br/>`print tan(135)` <br/>`-1.000000`                                                                              |
| `TRUE`                                                          | Return constant -1, a known truthy value. *<br/>Any value greater than 0 or less than 0 is considered true.*                                                                                                                           |

### File System and I/O Functions

| **Function**<img src="images/empty.gif" width="200" height="1"> | **Description** (*function and return value*)                                                                                                                                                                                                           |
| --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `eof(`*filename*`)`                                             | Returns non-zero value if at end of file *filename*.                                                                                                                                                                                                    |
| `exists(`*filename*`)`                                          | Returns true (-1) if the file *filename* exists, false (0) otherwise.                                                                                                                                                                                   |
| `locale` *string*                                               | Sets the locale to the ISO 3166-1 alpha-2 country code *string*<br/> e.g. locale "`de`" for Germany, “`us`” for United States of America.<br><br> Country code list: [ISO 3166-1 alpha-2 - Wikipedia](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) |
| `mos(`*command*`)`                                              | Returns zero if the command was successful, and non-zero error if it was unsuccessful or generated an error.<br/>**See**: MOS Commands.                                                                                                                 |

### BASIC Interpreter Functions

| **Function**<img src="images/empty.gif" width="200" height="1"> | **Description** (*function and return value*) |
| --------------------------------------------------------------- | --------------------------------------------- |
| err                                                             | Current error number                          |
| erl                                                             | Current error line number                     |

### String Functions

| **Function**<img src="images/empty.gif" width="200" height="1"> | **Description** (*function and return value*)                                                                                                                                                                       |
| --------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `asc(`*s$*`)`                                                   | Return ASCII value of first character or zero for empty string.<br/><br/>**Example****:** <br>`print asc(“A”)`<br/>`65` <br/>`print asc(“Z”)` <br/>`90`                                                             |
| `chr$(`*n*`)`                                                   | Convert ASCII decimal value to a character.  <br><br/> **Example:** <br>`print chr$(65)`<br/>`A` <br/>`print chr$(90)` <br/>`Z`                                                                                     |
| `instr(`*str\$,search$*`)`                                      | Returns the first position of search$ in str$, indexed from 1. Returns zero if not found.   <br/><br/>**Example:** <br>`print instr$(“xyzzy”,”y”)`<br/>`2` <br/>`print instr$(“xyzzy”,”a”)`<br/>`0`                 |
| `isval(`*s$*`)`                                                 | Converts string to number, returns -1 if okay, 0 if fails.                                                                                                                                                          |
| `left$(`*a$,n*`)`                                               | Left most number of characters (n) of a\$.   <br/><br/>**Example:** <br>`print left$(“heart”, 4)` <br/>`hear`                                                                                                       |
| `len(`*a$*`)`                                                   | Return length of string in characters.  <br><br/> **Example****:** <br>`print len(“xyzzy”)` <br/>`5`                                                                                                                |
| `lower$(`*a$*`)`                                                | Convert a string to lower case.   **Example****:** <br><br> print lower$(“ABCXYZ”) abcxyz                                                                                                                           |
| `mid$(`*a$,f*[*,s*]`)`                                          | Characters from a\$ starting at f (1 indexed), s characters, s is optional and defaults to the rest of the line.<br><br>**Example:** <br>`print mid$(“xyzzy”,3,2)` <br/>`zz`                                        |
| `right$(`*a\$,n*`)`                                             | Rightmost *n* characters of a$.<br/><br/>**Example**:  <br>`print right\$(“smart”, 3)`<br/>`art`                                                                                                                    |
| `spc(`*n*`)`                                                    | Returns<br> a string containing the number (n) spaces.<br/>**Example:** <br><br>`print “Start”+spc(20)+”End”`<br/>` Start                    End`                                                                   |
| `str$(`*n*`)`                                                   | Convert a number (*n*) to a string.<br/><br> **Example:** <br>`print str$(100)` <br/>`100` <br/>*Not really a good example, except that it returns a string of three characters 1, 0, 0, or “100”.*                 |
| `upper$(`*a$*`)`                                                | Convert a string to upper case.  <br><br/> **Example****:** <br>`print lower$(“abcxyz”)` <br/>`ABCXYZ`                                                                                                              |
| `val(`*s$*`)`                                                   | Convert string *s$* to number, error if bad number.   <br/><br/>**Example****:** <br>`print val(“150”)` `150`<br> *Not really a good example, except that it returns an integer of the string “150”, which is 150.* |

### Hardware Information Functions

| **Function**<img src="images/empty.gif" width="400" height="1"> | **Description (*function and return value*)**                                                                                                                                                                                                                                                                                                                                                                                           |
| --------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `alloc(`*n*`)`                                                  | Allocate n bytes of memory, return address                                                                                                                                                                                                                                                                                                                                                                                              |
| `analog(`*n*`)`                                                 | Read voltage level on pin n -- returns a value from 0 to 4095                                                                                                                                                                                                                                                                                                                                                                           |
| `deek(`*a*`)`                                                   | Read word value at a                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `event(`*v,r*`)`                                                | event takes an integer variable and a fire rate (r) in 1/100 seconds, and uses the integer variable to return -1 at that rate. If the value in v is zero, it resets (if you pause say), if the value in v is -1 the timer will not fire -- to unfreeze, set it to zero and it will resynchronize.                                                                                                                                       |
| `havemouse()`                                                   | Return non-zero if a mouse is connected.  <br> **Example:** <br/>`print havemouse()` <br/>`1`                                                                                                                                                                                                                                                                                                                                           |
| `himem`                                                         | First byte after end of memory -- the stack is allocated below here, and string memory below that.                                                                                                                                                                                                                                                                                                                                      |
| `inkey$()`                                                      | Return the key stroke if one is in the keyboard buffer, otherwise returns a n empty string.  <br><br/> **Example:**<pre>10 do<br/>20 a\$ = inkey\$()<br/>30 if a\$ <> “”:<br/>40   print a\$;<br/>50 endif<br/>60 loop</pre>Program will output the key pressed to the screen.<br/>                                                                                                                                                     |
| `idevice(`*device*`)`                                           | Returns true if i2c device present.                                                                                                                                                                                                                                                                                                                                                                                                     |
| `iread(`*device,register*`)`                                    | Read byte from I2C Device Register                                                                                                                                                                                                                                                                                                                                                                                                      |
| `joycount()`                                                    | Read the number of attached joypads, not including keyboard emulation of one.                                                                                                                                                                                                                                                                                                                                                           |
| `joypad(`[*index*],*dx,dy*`)`                                   | Reads the current joypad. The return value has bit 0 set if A is pressed, bit 1 set if B is pressed. Values -1,0 or 1 are placed into dx,dy representing movement on the D-Pad. If there is no gamepad plugged in (*at the time of writing it doesn't work*) the key equivalents are WASDOP and the cursor keys. If [index] is provided it is a specific joypad (from 1,0 is the keyboard), otherwise it is a composite of all of them. |
| `key(`*n*`)`                                                    | Return the state of the given key. The key is the USB HID key scan code *n.*                                                                                                                                                                                                                                                                                                                                                            |
| `mouse(`*x,y*[,*scroll*]`)`                                     | Reads the mouse. The return value indicates button state (bit 0 left, bit 1 right), and the mouse position and the scrolling wheel position are updated into the given variables.                                                                                                                                                                                                                                                       |
| `notes(`*c*`)`                                                  | Return the number of notes outstanding on channel c including the one currently playing -- so will be zero when the channel goes silent.                                                                                                                                                                                                                                                                                                |
| `page`                                                          | Return the address of the program base (e.g. the variable table)                                                                                                                                                                                                                                                                                                                                                                        |
| `peek(`*a*`)`                                                   | Read byte value at a                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `pin(`*n*`)`                                                    | Return value on UEXT pin n if input, output latch value if output.                                                                                                                                                                                                                                                                                                                                                                      |
| `point(`*x,y*`)`                                                | Read the screen pixel at coordinates x,y. This is graphics data only.                                                                                                                                                                                                                                                                                                                                                                   |
| `spoint(`*x,y*`)`                                               | Reads the color index on the sprite layer. 0 is transparency                                                                                                                                                                                                                                                                                                                                                                            |
| `tab(`*n*`)`                                                    | Advance to screen column n if not past it already.                                                                                                                                                                                                                                                                                                                                                                                      |
| `time()`                                                        | Return time since power on in 1/100 seconds.  <br><br/> **Example:**<br/>`print time()`<br/>`930150` *or 9301.5 seconds.*                                                                                                                                                                                                                                                                                                               |
| `uhasdata()`                                                    | Return true if there is data in the UART Receive buffer.                                                                                                                                                                                                                                                                                                                                                                                |
| `vblanks()`                                                     | Return the number of vertical-blanks since power on. This is updated at the start of the vertical-blank period.                                                                                                                                                                                                                                                                                                                         |

----

# Footnotes

<sup>1</sup> **Spaghetti code** is a pejorative phrase for difficult-to-maintain and unstructured computer source code. See: [Spaghetti_code](https://en.wikipedia.org/wiki/Spaghetti_code)

<sup>2</sup> Provided to use only for porting code to the NeoBASIC.  Continued use is not recommended as line numbers can change.  The renumber and library commands
or use of the built-in editor will change line numbers and not update gosub and goto calls.
