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

This version of BASIC was written with concepts brought over from more modern BASIC interpreters and other programming languages that make it more powerful and create a lot less spaghetti code[[1]](#footnotes) that BASIC programs generally tended to create.

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

<table border="0 cellpadding="5" width="100%">
<tr>
<td width="10%" align="center" vertical-align="middle">
   <img src="images/icons/eye-watch.png" width="30" ali>
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

A code should include comments.   Good comments don't repeat the code or explain it. They clarify its intent. Comments should explain, at a higher level of abstraction than the code, what you're trying to do.

Comments can be added to code as a line by itself or added to the end of an existing line by using the single quote following by your comment.   Comments should be limited to alphanumeric, and caution should be used when using quotes and other special characters.

Note, after entering a comment, the BASIC tokenizer will display it as a single quote followed by the comment within double quotes.   A comment is basically an ignored string at execution.

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



























----

# Footnotes

<sup>1</sup> **Spaghetti code** is a pejorative phrase for difficult-to-maintain and unstructured computer source code. See: [Spaghetti_code](https://en.wikipedia.org/wiki/Spaghetti_code)
