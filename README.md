
# universal_8bit_bus (U8B)
Universal 8 bit bus for variety of platforms.

This repository is created as a response to facebook comments: https://www.facebook.com/groups/396408204443613/404848536932913/?comment_id=405397260211374

# Bus signals
* (CE)    - card enable
* D0-D7   - input output data bus
* (RD)    - read from FIFO
* (WR)    - write to FIFO
* (EXE)   - start execution
* (IRQ)   - interuppt request
* RST     - reset
* VCC     - +5V
* VDD     - ground

# How it should work
Every expansion card should communicate with system via FIFO type memory located on card (AL422B for example or emulated with microcontroller). There should be separate FIFO memories for reading from card and for writing to card.

Command with according data placed in FIFO memory will be executed upon request signaled from system with EXE pin. End of execution of command or any other event can be signaled by expansion card to system with IRQ line.

# Logical structure
Expansion cards are devices that can implement variety of functions that may even be completly unrelated, for example card can support video out and mouse.

# Bus at host system
Every card slot should be mapped to system memory and represented as separate addresses of two bytes. Addresses with offset +0 are reserved to invoke command execution on card (EXE pin for is set to LOW while card enabled), where as addresses with offset +1 are meant for reading/writing cards FIFO.

# Standardized interfaces
To avoid fragmentation of expansions cards market (and necessity of drivers development for each of them) standardized set of commands should be prepared for card functions like: video out, video in, audio out, audio in, keyboard, joystick, mouse, network, storage (full list below)

# Commands
List of standardized command constants:
* 00h - getExpansionCardId - returns to buffer expansion card ID
* 01h - getExpansionCardName - returns number of characters of expansion card name followed by name encoded in ASCII
* 02h - getFunctions - returns number of functions that expansion card have implemented, followed by list of functions (single card may have many functions, for example audio in and out, video in and out on one board):
  - 00h - video out
  - 01h - video in (camera, grabber)
  - 02h - audio out
  - 03h - audio in
  - 04h - image out (printers, plotters, braile bars)
  - 05h - image in (scanners)
  - 06h - keyboard, button
  - 07h - joystick
  - 08h - mouse
  - 09h - touch
  - 0Ah - network adapter (TCP/UDP/IP/Encryption/Decryption, LAN, WIFI)
  - 0Bh - fan, motor (speed, direction)
  - 0Ch - light or diode (intensity)
  - 0Dh - light sensor
  - 0Eh - temperature sensor
  - 0Fh - accelerometer sensor
  - 10h - humidity sensor
  - 11h - gyroscope sensor
  - 12h - preasure sensor
  - 13h - voltage meter
  - 14h - amperage meter
  - 15h - power meter
  - 16h - oscilloscope
  - 17h - global positioning sensor
  - 18h - robot (CNC, 3D printer)
  - 19h - universal I/O
  - 1Ah - data storage
  - 1Bh - compute accelerator
  - 1Ch - real time clock
  - FDh - firmware
  - FEh - user function

# Commands specific to functions
Commands above 0Fh are specific to functions, their structure looks like that:

```<1 byte command constant> <1 byte function address> <n bytes of parameters>```
  
  where function address is an occurence on list of functions returned by 'getFunctions'

# IDC socket
Mainboard should use 20 pin two row female IDC socket.

# TODO
Pinouts and detailed commands specification for each function
