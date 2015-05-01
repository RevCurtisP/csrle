
# CompuServe standard for RLE file format

CompuServe RLE file format standard was formed in the 80's and defines 
the compression for 1-bit images.

Header sequence in RLE file represents Graphic Mode Control, control 
is initiated when program runs onto a sequence of three characters, 
those characters are ASCII ESC (HEX 1B), ASCII G(HEX 47) and the 
third character is ASCII H (HEX 48) or M (HEX 4D). Third character 
represents resolution, there are two possible graphics modes, and 
those are high resolution graphic mode (256 x 192 pixels) represented 
by sequence <ESC><G><H> and medium resolution graphic mode 
(128 x 96 pixels) which is represented by <ESC><G><M> sequence.

After header sequence, data sequence starts, basic data sequence 
consists of a pair of run length encoded ASCII characters. The first 
number represents number of the background (turned off) pixels and 
the second character is the number of foreground (turned on) pixels. 
Each number of a pair represents the count number of pixels plus 32 
decimal, i.e. from each number 32 is substracted and that number 
represents how many next pixels will be turned on or turned off 
depending on what number of pair we observe. Usually it is used 
ASCII ~(HEX 7E, DEC 126) as highest possible value, because some 
terminals interpret ASCII character 7F HEX as <RUBOUT>, because RLE 
file format was used as file which was to show graphic on terminals. 
Previous facts lead to conlussion that in each byte we can denote 
repetition of 94 pixels (126 - 32). For example pair <D><'> 
(HEX: 44 27, DECIMAL 68 39) means next 68 (decimal) pixels are turned 
off and then 39 (decimal) pixels are turned on.

Data in file is written in such a way that if the last pixel set was 
on position 254 then the next pixel will be on the first position in 
next line i.e. pictures are being drawn from up to down. Let's 
illustrate this with an example; if the last pixel set on line was on 
position 252 and data sequence consists of pair 21 hex, 27hex i.e one 
background pixel and seven foreground pixels then following pixel is 
turned off, then the following two pixels of current line are turned 
on, and then the rest of five pixels turned on, on the beginning of 
the next line.

The ending sequence for RLE standard consists of three characters 
<ESC><G><N>, <ESC> is a control character which ends the graphic 
display. Basic convention is that control character shouldn't effect 
the display. All control characters should be ignored besides <ESC> 
and <BEL> characters, <BEL> can be optionally used, so in some cases 
RLE file ending sequence consists of <BEL><ESC><G><N>. In other words 
end of RLE file according to standard is <ESC><G><H> or 
<BEL><ESC><G><N>.



Example - RLE file (all numbers are in ASCII HEX format):
1B 47 48 7E 20 7E 20 7E 20 7E 20 ....
 .     .    .    .    .    .    .    .    .     .    .
 .    41 36  .    .   .    .    .    .     .    .
 .     .    .    .    .   .    .    .    .     .    .
 .     .    .   07 1B 47 4E  

1B 47 48 - is header <ESC><G><H> and represents high resolution, first data sequence pair 20hex, 7Ehex means that first 94dec pixels are all turned on, the second data sequence is the same so second 94d pixels are also turned on (the first 188d pixels are turned on so far), and so on. Then somewhere in the file pairs 41h 36h occurs which means that next 33d pixels are turned off and after that 22d pixels are turned off, etc. Last four character are the ending sequence which was described above.
