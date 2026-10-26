# Mini Piano: 555 Timer PCB

A five-key electronic piano designed in KiCad for Rice University's ELEC 240.
An LMC555 runs in astable mode, and each key selects a different timing
resistance, setting the frequency of the square wave that drives a piezo buzzer.

![3D render](PCB%20Design.png)

[View the schematic (PDF)](PCB%20Design.pdf)

[Front copper (PDF)](PCB%20Design-F_Cu.pdf) · [Back copper (PDF)](PCB%20Design-B_Cu.pdf) · [Board outline (PDF)](PCB%20Design-Edge_Cuts.pdf)

Instead of giving each key its own resistor, R2 through R6 form a single series
ladder, and each key taps a different point on it. Each note's R_B is the
running sum of the resistors above its tap:

| Note | Key | R_B | Theoretical f | Target |
|---|---|---|---|---|
| A5 | SW1 | 5.6k | 907 Hz | 880 Hz |
| G5 | SW2 | 6.6k | 806 Hz | 784 Hz |
| E5 | SW3 | 8.4k | 671 Hz | 659 Hz |
| D5 | SW4 | 9.6k | 604 Hz | 587 Hz |
| C5 | SW5 | 11.1k | 536 Hz | 523 Hz |

With f = 1 / (ln2 * (R_A + 2R_B) * C), R_A = 4.7k, and C = 100 nF.

What this buys:
- **Five standard resistor values make five notes.** No odd values or trimmers.
- **High-note priority for free.** If two keys are pressed, the tap closest to
  the 555 shorts out the ladder below it, so the higher note wins, like a
  monophonic synth.
- **A pentatonic scale.** C, D, E, G, A is the C major pentatonic, so there is
  no wrong note.

## Other details
- 1N5817 Schottky diode for reverse-polarity protection, with lower drop than a
  standard silicon diode
- 10 uF bulk and 100 nF decoupling capacitors, plus 10 nF on the 555 control pin
- Power LED, screw terminal input, and a 1k series resistor on the buzzer
- Two-layer board with ground pours on both layers and zero vias. Through-hole
  pads handle every layer change.
- Keys and ladder resistors are laid out in pitch order, like a keyboard.

## Tuning notes and next revision
Every note comes out 30 to 53 cents sharp in theory. Because the error is
roughly uniform, the whole scale can be pulled down together with one part
change: adding about 2.2 nF in parallel with C1 lowers every note by roughly
the same amount.

## Status
The design is complete. I'm soldering and testing the board this semester, and
I'll add photos and measured frequencies once it's built.
