# Agon 24-bit eZ80 FORTH

(C) Copyright 2023, L.C. Benschop
Released under GPL v3
This program contains Z80 code originally copyrighted 1994, 1995 by
Brad Rodriguez.

## Introduction

This FORTH system is designed to run on the Agon Light computer and it runs in 24-bit ADL mode.

A useful introduction to FORTH can be found here. 
http://galileo.phys.virginia.edu/classes/551.jvn.fall01/primer.htm
The FORTH system does not have all features mentioned in that document, such
as local variables and floating point.

### Package contents

The `forth24` subdirectory contains the following files:

* readme.txt (this file).
* forth24.bin (the FORTH binary to run on Agon).
* kernel24.bin (the bare FORTH binary generated by the metacompiler. Requires to load extend24.4th later).
* glossary.txt (a list of all words in FORTH with a short explanation).
* testcode24.4th (example for code definitions).
* cross24.4th Can be run from gforth to generate Agon eZ80 FORTH from
  source.
* dometa24.4th Can be run from Agon forth to generate Agon eZ80 FORTH from
  source (kernel80.bin).
* metaez80.4th. The metacompiler, used to generate Agon eZ80 FORTH from
  source.
* asmez80.4th. Source of the FORTH assembler.
* kernl24a.4th, kernl24b.4th, kernl24c.4th, and extend80.4th: sourcees of
  Agon Z80 FORTH.

### Prerequisites

You need Agon MOS of at least version 1.03. 
You need a text editor to edit FORTH source files on Agon. The following repository contains the Nano.bin file that you can install on the
micro-SD card in the MOS subdirectory.
https://github.com/lennart-benschop/agon-utilities

## Getting started

## Starting up.

Make sure you have the files on the SD-card, in particular
the forth24.bin file. When your autoexec.txt causes your machine to start up in BASIC, make sure to leave BASIC by typing the
following command: `*BYE`

At the MOS command prompt type:

load forth24.bin

Then type the command:

run

Alternatively you can automatically load a Forth source file on startup. This is especially useful if you run Forth 
in autoexec.txt. Example
```
run &40000 tetris.4th
```

You should now see a message like this:
```
Agon 24-bit eZ80 Forth, 2023-07-02 GPLv3
Copyright 2023 L.C. Benschop, Brad Rodriguez
```

You are now at the FORTH command prompt. You can type a FORTH command
like this:

```
12 23 * .
```

You should see the the line change to
```
12 23 * . 276 OK 
```
Forth is a stack-based language and formulas you type use Reverse
Polish Notation. What you just typed:

`12` puts the number 12 on the stack,
`13` puts the number 13 on the stack.
`*` takes two numbers from the stack, multiplies them and puts the result
  on the stack.
`.` takes the top number from the stack and prints it.

What FORTH replied to you:

`276` is the result of the `.` command the printed result of the multiplication.
`OK` is a response to tell that there were no errors.

You can add new words to the FORTH language as follows. Type the following
commands:
`: SHOW-NUMBERS 20 0 DO I . LOOP ;`
Start with a colon (`:`) and end with a semicolon (`;`).
After typing this, you should see OK

Then type:
`SHOW-NUMBERS`

You should now see:
`SHOW-NUMBERS 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 OK`

The colon starts the definition of a new FORTH word. In this case it is
SHOW-NUMBERS. DO and LOOP create a loop (start at 0, run up to but not
including 20).  The word I puts the loop index onto the stack. The word
`.` prints this number.  The semicolon ends the currently defined word.

You can looad a source file by typing the following comand:

`FLOAD squares.4th`
Then Type:
`20 SQUARES`
You should see:
```
    1          1
    2          4
    ...
   20        400OK
``` 
You can now view and/or edit the loaded source file with the ED
command. Press the ESC key to leave the editor.

You can go to the MOS command prompt using the `BYE` command or by pressing Ctrl-Alt-Del.

From there you can restart FORTH simply by typing the run command again.
Your changes to the dictionary and the loaded source file in memory will
stay intact.

If your FORTH program gets stuck in an endless loop, you can go back
to the command prompt by pressing Ctrl-Alt-Del and go back to the FORTH prompt
by entering the 'run' command.

## Helpful commands

`.S`

This command shows you the contents of the stack, the word Empty when
the stack is empty.
```
.S  Empty OK
1 2 3 4 .S 1 2 3 4 OK
DUP .S 1 2 3 4 4 OK
+ SWAP - .S 1 2 5 OK
DROP DROP DROP .S Empty
```

`WORDS`

This command shows all available words in the FORTH (there may be
additional words in separate word lists). You can stop the command
halfway by pressing the ESC key or you can pause it by pressing any
other key (pressing one further key resumes it).

`FORGET <word>`

This removes the specified word and any later definitions from the FORTH
dictionary. When you look at the squares.4th file, you see that it starts
with FORGET SQUARES. You can edit and reload that file many times and each
time it will forget the old definition of SQUARES before compiling the new
one.

## File related commands


OPEN <name>

Select the specified source file to edit and load.
`OPEN squares.4th`


`OK`

Run (compile and/or interpret) the source file that is selected by OPEN.


`ED`

Edit the source file selected by OPEN. Leave the editor by pressing the ESC
key. In fact the editor can edit any ASCII text files, not just FORTH
sources. In principle you can edit this README.md file.

`FLOAD <name>`

Load the spacified file into memory and immediately run it. This is equivalent
to `OPEN <name> OK`

The commands FLOAD, OPEN, OK and ED are the same as in the F-PC forth
for MS-DOS, introduced in 1988.

`CAT`

Show all files on disk.

`DELETE <name>`

Delete the specified disk file.

`addr len BLOAD <name>`
Load a file into a specified memory region. For example:
`$F000 $800 BLOAD chardefs.bin `

`addr len BSAVE <name>`
Save a memory region into the specified file. For example
`$F000 $800 BSAVE mychars.bin`

`SAVE-SYSTEM <name>`

Save the current FORTH dictionary into the specified file. You can later
load this file at the MOS prompt and run it.
Example:
`SAVE-SYSTEM myapp.bin`

## Glossary file

The file `glossary.txt` gives a short explanation for every FDRTH word.
THis file is generated from the source files.

Each word is accompanied by a stack diagram, like this:

`+ ( n1 n2 --- n3)`
The numbers shown left of the --- are the numbers that the word expects
as inputs, the numbers shown to the right are the numbers produced by the
word as results. So in this case, '+" expects two numbers n1 and n2 as
inputs and produces one number n3 as a result.

## The assembler

This is a prefix assembler, like F-PC used to have. The assembler instructions
look the same as you learned them, as opposed to more traditional FORTH
assemblers, where instructions have a postfix notation. Postfix makes the
implementation of the assembler simpler. In particular for the Z80, most
FORTH assemblers either only implement the 8080 subset of instructions or use a strange mix of 8080 and Z80 mnemonics that are completely nonstandard.
I wanted my Z80 code to look like Z80 code in other assemblers.

However, there are a few small diferences:
* You need to attact a comma to each source operand and separate it from
  the destination operand by a space, so we write:
  ```
   ADD HL, BC
   ADD A, E
  ```
  It will not work if you write `ADD HL,BC` or `ADD HL , BC`
* Comments to end-of-line are backslashes, not semicolons.
* Direct loads use (), and ()
  LD A, $1234 ()     instead if LD A,(1234h)
  LD $4567 (), HL    instead of LD (4567h),HL
* Indexed uses (IX+), and (IY+)
  `ADD A, 4 (IX+)`     instead of `ADD A,(IX+4)`
  `LD -5 (IY+), B`    instead of `LD (IY-5),B`
* Support for labels is very limited. It is supported only in the cross
  assembler and only for backward references.
  Instead you use `BEGIN..UNTIL`,
  `BEGIN..WHILE..REPEATE` and `IF..ELSE..THEN` constructs.
  Jumps and calls to constant addresses are fine.

  There is support for numeric labels 1 to 9. Use 1F..9F for forward references
  and 1B..9B to backward references to the nearest numeric label. Define with
  1:..9:
  Example
```
    JR NZ, 1F  \ Conditional forward jump to 1: label.
    ADD HL, DE
1:  ADD HL, DE    
    JR NC, 1B  \ conditional backward jump to 1: label.
```

Example code definition:
```
CODE 3* ( n1 --- n2)
\G Multiply a number by 3
  LD HL, 0
  ADD HL, DE
  ADD HL, HL   \ Times 2
  ADD HL, DE   \ Add original value, so times 3
  EX  DE, HL    \ Move result to TOS
  NEXT
END-CODE
```
A code definition starts with CODE and ends with END-CODE.
The word NEXT expands to the instruction sequence to execute the next
threaded code definition. This should be the last thing executed by
(almost) every code definition.

You can use constructs like `IF..THEN` and `BEGIN..UNTIL` like this:
```
CODE CLZ ( u --- n)
\G Count leading zeros.
   LD HL, 0     
   AND A
   ADC HL, DE   \ Test if TOS = 0 (and move to HL).
   0= IF
     LD DE, $18 \ If zero we have 24 leading zeros.
   ELSE
     LD DE, $FFFFFF \ Start at -1 as we increment 1 more than leading zeros.
     BEGIN
	ADD HL, HL \ Shift left
     	INC DE     \ Count one more
     U< UNTIL      \ Until the last bit shifted out is 1.
   THEN
   NEXT
END-CODE   
```
The IF, ELSE and UNTIL words expand to (conditional) JR instructions.
For example 0= IF expands to a forward JR NZ to the point at the corresponding
ELSE (or THEN when there is no ELSE).
The U< UNTIL corresponds to a backware JR NC to the point at the correspioding
BEGIN. Only 0=, 0<>, U< and U>= are allowed as conditions.
Plus the weird looking B--0=, which expands to DJNZ.

There are @IF, @ELSE, @UNTIL etc. words that are exactly like IF, ELSE
and UNTIL, but they use absolute jumps instead.

## Internals

### Memory Map
```
$40000      Startup code.
$40040      MOS header.
$50050      Forth executable+dictionary
HERE..PAD  word buffer, numeric conversion buffer
PAD..PAD+80 temporary workspace
    -$8FE00 Free space
$8FE00-$8FFFF Return stack
$BF000-$BFFFF Data stack (shared with MOS).
```

## Threading model

Agon eZ80 FORTTH uses the eZ80 registers as follows:
* IY is the instruction pointer, it points to a list of word addresses inside
   a colon definition.
* DE is the top of stack
* SP is the (data) stack pointer.
* IX is the return stack pointer.
All other registers (including the shadow registers) are free to use.

It is a direct threaded FORTH, which means that the NEXT routine jumps
to the Code Field address of the code word. The code field of machine code
words (like + or !) contains machine code. The code field of all other words
contains a CALL instruction to the handler of that type of word.

## Dictionary structure 

Agon eZ80 FORTH uses hashing to speed up dictionary searches. A hash
value is computed from the name by XOR and shift operations on the length,
the first and second (if applicable) characters of the name. The number of
hash chains is a power of 2 (typically it is set to 16) and the hash values
are then 0..15. Each name is stored in the hash chain corresponding to
its hash value, therefore it needs to be looked up in that same chain.
Splitting the dictionary into multiple chains speads up searching.

Each wordlist data structure has the following fields.
* `VOC-LINK` 1 CELL, link to previously defined wordlist. FORGET traverses them
                 all.
* `#THREADS` 1 CELL, the number of hash chains used by this wordlist,
heads    #THREADS cells: the heads of the linked lists for each hash chain.

The VOCABULARY defining word has the wordlist data structure in its
parameter field.

Each word has a header that contains the following fields.
Link field 1 CELL contains the address of the name field of the
                  previous definition in the same hash chain.
Name field Count byte (bits 4..0 specify name length), bit 7 always set.
                       bit 6 set for IMMEDIATE words, bit 5 unused
            followed by as many ASCII characters as specified in count byte.
Code field. directly follows the last byte of the name.
            For code words, machine code starts here.
	    For all other words: contains a CALL to the handler of the word
	    (call is 4 bytes).
Parameter field: data belonging to the word. For colon definitions this
            is a list of code addresses

## How to recompile

### introduction

Agon eZ80 FORTH is cross-compiled (meta-compiled) from
FORTH. Originally it was cross-compiled from gforth (a FORTH running
under Linux), but the tools are designed such that they can also run
from Agon eZ80 FORTH itself. It can be cross-compiled in both ways.

Compiling occurs in two stages:
- The cross-compile stage will produce a bare-bones FORTH system called
  kernel24.bin. This system misses many features, but it is powerful
  enough to load and compile its own extensions.
- The extension stage will ALWAYS run on Agon,
  even if kernel24.bin was generated from another FORTH version.
  The extension stage is started by running kernel24.bin and loading
  several FORTH source files from it. Those source files compile
  several additional important FORTH words and the assembler.
  After this you can save the extended system to the SD-card using the
  SAVE-SYSTEM command.


### Cross compile from gforth

Run the following command:

`gforth cross24.4th`

This file will load asmez80.4th, metaez80.4th, kernl24a.4th, kernl24b.4th and
kernl24c.4th adnd will save kernel24.bin afterwards.

The file kernel24.bin must be copied to an SD-card together with the
source files used in the extension stage.

### Cross compile from Agon eZ80 Forth.

First run forth24.bin. The standard version of forth24.bin has the
eZ80 assembler already loaded. Form a customized version without the assembler
you may need to load the assembler asmez80.4th first.

From Agon Z80 FORTH, run the following commands:

`FLOAD dometa24.4th'

This will load the files metaez80.4th, kernl24a.4th, kernl24b.4th and kernl24c.4th and then save the rebuilt kernel24.bin image.
Finally it will exit FORTH and return to the MOS prompt.

###Extension stage.

On the Agon MOS prompt, load and run kernel24.bin

load kernel80.bin
run

This starts a minimal FORTH system. You can now load the extensions.

FLOAD extend24.4th

When extend24.4th and asmez80.4th are loaded a lot of Redefining
messages are shown. This is normal.  extend24.4th will also load the assembler and save the rebuilt forth24.bin file and exit FORTH.

## Origin

The machine-independent code and the metacompiler were based on SOD32
FORTH that I wrote in 1994.  SOD32 was a virtual 32-bit stack machine.

Much of the design of that FORTH was based on public domain FORTH
implementations of the 1980s, like F-83 by Laxen & Perry and F-PC by
Tom Zimmer and Robert L. Smith. 

Later in 1994-1995 I wrote a FORTH for the 6809, based on SOD32
FORTH. This one used a prefix assembler, just like the one used in
F-PC and like the one I now wrote for the Z-80.

When I wanted to create a FORTH for the Cerberus-2080, I wanted to have it
for the Z80 as that is the CPU that I am most familiar with and that IMHO
is more suitable for running FORTH. Besides, Alexandre Dumont was already
working on a FORTH for the 65C02.

I had a few loose requirements:
* It would be a metacompiled FORTH, generated from an existing FORTH system.
  Once it would be complete, it would be able to recompile itself
  (be self-hosting). F-83 and F-PC also had this feature.
  * The alternative to metacompiling is building the FORTH system using
    a traditonal assembler. For example Camel Forth is written using a
    macro assembler. The problem with this is that you are not self-hosting
    and that most FORTH implementations depend on very specific macro features
    found in one specific cross assembler that may not be available or only
    runs under DOS.
* Like F-PC it would load source code from text files. I also considered a
  block-based system, but I decided against it for the following reasons.
  * BIOS would only allow to read and write files in one go. It would not
    support the random access that allowed you to maintain a very large
    disk file and have only a few blocks in memory at once.
  * Blocks have traditionally 16 lines of 64 characters each. This did not
    work well with the 40 column screen of Cerberus. 40 columns is not
    ideal for text files, but one can write text files with short lines.
* It would contain a text file editor, like F-PC.    
* It had to be loosely ANS Forth based.
* It had to have a prefix assembler, like F-PC.

For Agon I wanted to stick with mnost features from Cerberus Z80
FORTH, except that I decided not to include a text editor in Forth
itself. A buffer to hold a large source file would take up too much
addressing space. The good news is that it is easy for Agon to run the
editor as an external program, even from within Forth.

I ended up using most of the primitives of Camel Forth, a FORTH
written for the Z80 by Brad Radriguez in 1994 and 1995. Cerberus Z80
Forth and Agon Forth are NOT Camel Forth, it just contains code from
it. I could have picked the Z80 primitives from my old ZX-Spectrum
FORTH instead, but CamelForth promised to be faster.

The main thing missing was a Z80 assembler. I could not find any good
ones written in FORTH. The Z80 assembler I used for my ZX Spectrum
Forth was a postfix assembler (I think I could even live with that),
but it also had weird mnemonics unlike the well-known Z80 mnemonics
and more like the 8080 mnemonics, but not quite that. So I decided to
write my own Z80 assembler in FORTH that was a prefix assembler. This
was more of a challenge than I thought at first, but I got it done.

Others who read my source code can now at least recognize the
assembler instructions.



