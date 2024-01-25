# Wildcard

The Wildcard is a 1982 Apple ][ card used to make an in-memory copy of copy-protected software. It basically hijacks program execution, substituting its own 2k EPROM which has some routines for saving what's in memory to disk.

Button pressed -> /NMI low.

This causes the processor to start fetching the NMI vector at $FFFA. $FFFA is decoded by U4A, U5 and U6 and U1D pin 13 goes low. This causes the output at U1D pin 11 to go high.

phi0 -> RDY low, this places the CPU in halt.
phi0 -> /INH, /DMA low, $C081 on the bus, turns Language Card on

The first clock received clocks the 1 from U1D pin 11 into register 4, causing Q4 to go high and /Q4 to go low. Q1 is still low, since the 74175 just came out of reset. This pulls RDY low, halting the CPU (before it can actually fetch the NMI vector).
Q4 is fed to register 2, so at the next clock Q2 goes high and /Q2 low. This pulls /DMA low and puts $C081 on the bus, which turns the Language Card on. It also pulls /INH low, this turns off the on-board ROMs.

phi0 -> /DMA high, /INH low, EPROM active

The next clock sets Q3, which turns off /DMA and the $C081 that was forced onto the bus, and enables the EPROM via /OE.

phi0 -> RDY low, CPU comes out of halt

And the next clock gets the CPU running again (still trying to fetch the NMI vector from $FFFA, but now the Wildcard EPROM is mapped at $E000 to $FFFF.

Access to /IOSTB switches the EPROM off again

Like /RESET, /IOSTB will reset the Wildcard.

![Shield](Wildcard_info.png?raw=true)

---------------------------------------------------------------------

![Shield](s-l1600-11.jpg?raw=true)
![Shield](s-l1600-13.jpg?raw=true)
![Shield](s-l1600-14.jpg?raw=true)
![Shield](s-l1600-15.jpg?raw=true)

