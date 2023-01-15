
# General information

## Which 6502 to buy?

* Especially when you are inexperienced, buy a real WDC W65C02S6TPG-14. This is still in production, has a static design (so it can be single stepped) and just works. It's not available cheaply on Aliexpress, so don't even try.
* R65C02 could be okay. When single stepping, the clock has to be HIGH, with short LOW pulses.
* Don't use NMOS versions like the original MOS6502 to avoid problems.
* Don't use any "UMC W65C02" (they don't exist) or other wannabe W65C02S from China. Rockwell stopped existing after 1999, so any newer Rockwell chips are fake.

## Connecting the 6502

* Supply voltage
  * Pin 8 / V<sub>dd</sub>: +5V
  * Pin 21 / V<sub>ss</sub>: GND
  * Pin 1 / V<sub>ss</sub> (NOT FOR WDC W65C02S. Leave open, since this is
    <span style="text-decoration:overline">`VP`</span>)

* Use a 3k3 pullup resistor (if not used) for
  * Pin 2 / `RDY` (essential, because this is bidirectional)
  * Pin 4 / <span style="text-decoration:overline">`IRQ`</span>
  * Pin 6 / <span style="text-decoration:overline">`NMI`</span>
  * Pin 36 / `BE` (WDC W65C02S only)

* Leave unconnected (if not used)
  * Pin 1 / <span style="text-decoration:overline">`VP`</span> (WDB W65C02S only)
  * Pin 3 / `PHI1O`
  * Pin 5 / `MLB`
  * Pin 7 / `SYNC`
  * Pin 35 / `NC`
  * Pin 36 / `NC` (this is `BE`/"bus enable" on the WDC W65C02S)
  * Pin 37 / `PHI2`
  * Pin 38 / <span style="text-decoration:overline">`SO`</span>

* Reset circuit to Pin 40 / <span style="text-decoration:overline">`RES`</span>
  * Cheap version (works with restrictions): Use a 3k3 pullup resistor to +5V and a push button to GND
  * Use a DS1813 (recommended, small and simple)
    * Pin 1 goes to <span style="text-decoration:overline">`RES`</span>
    * Pin 2 goes to +5V
    * Pin 3 goes to GND
    * Connect Pin 1 and Pin 3 with a push button
  * Use a combination of 74HC14s to create a debounced LOW-spike
  * Use a 555 timer to create a debounced LOW-spike

* Connect some external clock to Pin 39 (`PHI2C`)
  * Debounced push button for single stepping
  * A 555 timer for a slow clock
  * 1 MHz or 2 Mhz oscillator (not a crystal). Don't go higher, unless you eat timing diagrams for breakfast

* Signals used by other parts of the system:
  * Pin 9-20, Pin 22-25 / `A0`-`A15`
  * Pin 33-26 / `D0`-`D7`
  * Pin 34 / `R`/<span style="text-decoration:overline">`W`</span>
  * Pin 40 / <span style="text-decoration:overline">`RES`</span> (the reset signal coming from the reset circuit)

## Choosing ROM

* Avoid anything starting with `27C`, because this is UV erasable EPROM. You cannot delete them using sunlight 
(not in a reasonable amount of time), so you would need a UV erasing device. Erasing takes likes 15 minutes.
Also, UV light kills your eyes.
* EEPROMs (`28C`): up to 150ns
  * 28C256 (32 KiB, preferred)
  * 28C64 (8 KiB)
* Flash EEPROM (`29C`): up to 70ns
  * 29C010A (128 KiB)  
* NOR-Flash (`SST39SF`): 70ns 
  * SST39SF010A (128 KiB)
  * SST39SF020A (256 KiB)
  * SST39SF040 (512 KiB)

All ROMs work the same way when reading. Programmers need a different logic depending on the type.