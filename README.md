# agon-forth
Forth for Agon computer

## forth16

The forth16 directory contains a 16-bit FORTH for Agon, to be run in Z-80 mode.

## forth24

The forth24 directory contains a 24-bit FORTH for Agon, to be run in
eZ80 ADL mode. This can use all available RAM. This has slightly more words
than 16-bit FORTH.

For the VIEW command to work, it is necessary to store all files from
the forth24 subdirectory into the /forth24 directory on the SD card
and to store the see.4th file into the /forthlib directory on the SD
card.

## forthlib

The forthlib directory contains library files, intended to be loaded via
the require command.
* see.4th (a simple decompiler)
* asmz80.4th (the 16-bit Z80 assembler, the 24-bit FORTH always contains
  the eZ80 assemlber as a standard).
* misc.4th (miscellaneous words)
* files.4th (file extensions, only for forth16. DBLOAD and DBSAVE to load
  and save files into the entire RAM. Forth24 can use BLOAD and BSAVE with
  24-bit addresses. Also contains READ-FILE, WRITE-FILE,
  REPOSITION-FILE. Forth24 has these standard).
* agon.4th (Graphics, sound, joystick commands for Agon).
* vload.4th (command to load VDU sequences like bitmaps or sound samples from
  files).

## examples

The examples directory contains FORTH example programs that are not specific to Agon FORTH, but that can run under it (and under both versions).
* tetris.4th is a Tetris-like program for text terminals. I used to run it a lot in the early days of Linux.
* tester.4th and core.4th is a small test suite for Forth.
* glosgen.4th is a program to generate glossary files.
* squares.4th is a small example program to run on Agon FORTH.
* sunrise.4th is a program to print calendars with sunrise and sunset times.
  It requires floating point, so it only runs on forth24 on Agon (and on other
  FORTH systems like gforth). On Agon it can plot sunries/sunset graphs too.

## examples_agon

The examples_agon directory contains FORTH example programs and utilities
that are specific to Agon FORTH, but that run under both the 16-bit and the
24-bit versions of it.
* serpent.4th Snake-type game originally submitted to the Olimex WPC June 2023.
  This is a slight adaptation of this game.
* restit.4th Tetris-type game originally submitted to the Olimex WPS July 2023.
  Slight modifications.
* spacer.4th 2D horizontal shooter game inspired by TI Parsec. Uses character
  graphics only.
* graphics.4th Graphics library, including turtle graphics.
* grpdemo.4th Graphics demo, runs on top of graphics.4th
* dodemo.4th. Load graphics.4th and grpdemo.4th and then runs the demo. Put forth.bin (or forth24.bin), graphics.4th, grpdemo.4th and dodemo.4th all in one directory, then the following
  commands run the demo.
```
load forth.bin
run . dodemo.4th
```
## games

Three ready-to run binaries of the games serpent, restit and spacer. You
can run them as MOS commands or just load them as binaries. 
These are 'turnkey' versions of the corresponding FORTH sources.
