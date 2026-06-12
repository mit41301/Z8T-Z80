The floppy disk which comes with the Southern Cross EPROM emulator contains
examples each with a .Z8T, .HEX and .PRN file. What does this mean? Let us
review the procedure of assembling source files into object files using a
PC.

When you write a program in mnemonic form (with LD, OUT etc. as we 
have been doing) you are writing source code. You will usually use 
a PC and text editor program. (Do not use word processing programs
like Word, Wordperfect, etc. You must use a text editor programs
which enters pure ASCII codes without hidden or embedded formatting 
commands. We use Qedit v2.07.) You will normally use a suffix for 
the file name which indicates the type of file it is. Mnemonic files 
generally have the .ASM suffix.

The computer does not understand the mnemonics you have written. They
must be translated or assembled into the binary pattern that the
computer understands (object code.) However, it has been found in 
practice that  dealing with pure binary object code ( .bin files) 
presents some problems. When they are transmitted by modem or from 
computer to computer any electrical spikes can change one or more 
bytes and the receiving computer has absolutely no way of knowing 
that this change has happened. It has been found that an intermediate 
file (a formatted hex file) which contains checksums, end-of-file 
markers and which is formatted in a defined way is a better way to 
proceed. The most common formatted hex file format is now the Intel 
hex format. There are other formats (Motorola, Tektronix) but all
assemblers will have the Intel hex file option. A complete definition of
the formats can be found in the UK journal Elektor, September, 1987, page
57. It is a simple matter for the receiving computer or uC system to check
that the received file conforms to the Intel hex format, and to do the
final extraction of the object code from it (that is, strip of the checksum
bytes and other additional information which is used to check for for file
integrity) and to put the object code in the appropriate memory locations.
The program in the 8748/9 microcontroller does this.

We have provided a public domain Z80 assembler for DOS, Z8T.COM. To 
more easily identify source code we have used the .Z8T suffix. To 
do the assembly enter 

Z8T myfile.z8t

Z8T produces an Intel formatted hex file as its output. It has the
.hex suffix. The assembler also produces a print files (.prn) which is a
combined listing of the original source code and the assembled code. Use a
text editor to look at the files and you will soon understand what is going
on much better than if we spend paragraphs here explaining it.

The assembler also looks after many other things besides just converting 
mnemonics. It allows you to write your source code using easy to remember 
name and labels. The assembler does all the difficult mathematical 
work of calculating the numbers and absolute address locations for 
the object code, calculating the relative offset required in branch
instructions etc.

Computers must handle text information as well as numbers. The standard 
code for text information on PC computers is ASCII. Alphanumeric characters 
have a defined binary pattern which all manufacturers and software 
writens accept and program to.

Sufficient examples are provided that the rules of writing programs for the
Z8T assembler will rapidly be known to you.

To prepare a program for the assembler it must contain some directions for
it. For example, at what address to start assembly, END to know when to
stop assembly, what label names to use for various absolute addresses.
These assembler directives (also called pseudo op codes) which have to be
added to the source code before assembly. (And they must be at the
beginning of the program so they are read before the actual code is
encountered.)

The EQUate is used to give a specific memory location or binary number 
an easily remembered name rather than an absolute address location. 
On the Southern Cross Single Board Computer all this information is
contained in a Header File. The Header file provides a
complete listing of all EQUates used in the Monitor so you can use this
code for your own programs. It also means that code written for this
Monitor will be valid for future Monitor versions even though the absolute
address of subroutines in the new version may be completely different from
this one. All you have to change is the header file, reassemble and the new
absolute address locations are added automatically.

The ORiGin assembler directive tells the assembler where you want 
the program to start. It can be used more than once in a program to 
put different parts of a program in different positions in memory, 
eg, the message program used with the 8x8 display has 5 ORiGin statements.

Space for tables is reserved using the db and dw (define byte & define
word) pseudo op dodes. Examples can be found in some of the programs on
the floppy disk.

Some other Z8T syntax rules are:

1. put an h or H at the end of hex numbers to indicate they are hex
numbers, eg 2Eh

2. place a 0 (number zero) in front of numbers if you want them to be
treated as numbers and not memory locations, eg LD HL, 0FFFFh

3. a '$' as or in the argument of a mnemonic code will substitute the
address the current line is on, eg 'LOOP   DJNZ $' will keep looping on
that line until B (set earlier) equals zero

4. labels must start in column 1. Non label fields must begin in column 2
or greater.

Lines with errors in assembly are shown with a character in the first 
position of the line in the .prn file. Garbage characters after the 
2 arrows at the end of the .hex and .prn files can be disregarded. 
Other limitations of z8t will gradually make themselves known to you. 


No More Hand Assembly
---------------------

With a PC and an assembler it is very easy to forget about hand assembly 
altogether. Even for just looking up a couple of codes you can run 
z8t on the code fragment instead of looking up a book (or trying to 
find the Listing somewhere buried on your desk!) Here is an actual example
about how we developed a code examples for the Southern Cross 8x8 addon
card. We made a quick text file out of our head which we though would work
as follows:


    org 2100h
    ld hl,2000h
    ld c,16h
    rst 30h
    jp 2100h
    end


We ran 'z8t 8x8test.z8t' and looked at the prn file to check for
errors. It ran OK but with garbage on the displays.  We entered in 
the 8 byte buffer at 2000h to 2007h by hand having calculated the 
bytes on a piece of paper for the letter 'A',  entered the object
code by hand at 2100h and did a Fn 0 at 2100h. It worked so there 
was our example. Comments were the last thing to add.

This may not be the way that the text books say code should be developed. 
However, it is one actual way in the real world that code is produced 
which is why the example has been given here.

            - - - - - - - - - - - - - - - - - - - -

