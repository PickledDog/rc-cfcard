# rc-cfcard
 Buffered CompactFlash card for RC2014
![Assembled PD105](/img/assembled.jpg)

## Overview
This is my take on an RC2014 CompactFlash adapter card. It incorporates fixes gleaned from discussions within the retrocomputing community, aiming to maximize compatibility with CompactFlash cards old and new.

## Description
This board allows one to add a CompactFlash card to their RC2014 system. Adding non-volatile flash storage like this allows one to run CP/M, via Grant Searle's [CP/M Loader](http://searle.x10host.com/cpm/index.html), Stephen C Cousins' [Small Computer Monitor](https://smallcomputercentral.wordpress.com/small-computer-monitor/), or Wayne Warthen's [RomWBW](https://github.com/wwarthen/RomWBW), other system components permitting. As a general rule, CP/M requires your system be able to boot from ROM, then page it out for RAM (basic memory boards can't do this, pageable ones can).

### Technical details
Interfacing a CompactFlash card to the RC2014 bus is nothing new. Short of doing all access via an 8255 PPI, this generally involves mapping the (ATA-mode) CF address space to I/O addresses 0x10 through 0x17, and connecting 8 of the data lines to the RC2014 databus. However, the fast level transitions of modern CompactFlash are only intended for the short traces between card and interface in a system, and do not play nice with the long paths of the unterminated RC2014 bus.

My approach to handling this was informed by Bill Shen's posts in the [retro-comp Google Group](https://groups.google.com/g/retro-comp). My takeaways from these, were that:
* CS should be asserted a little before IORD and IOWR
* The data line transitions should be limited, either via 100Ω resistors (good), or a 74x245 bus transceiver (better)
* CompactFlash is CMOS (not TTL), so said transceiver should be 74HC (not 74HCT)

With these in mind, the IORD and IOWR signals are set up with a 2-gate delay from the address decoder (which is 74AHCT, to give it a little head start). The CF data lines end at a 74HC245 bus transceiver, from which nice RC2014-friendly level transitions meet the bus. In addition, I *really* wanted to mount the CF socket directly to the board (and skip the IDE-CF adapter). I practiced mounting common SMT CF sockets to boards, and while I am able to do this, the difficulty involved is undeniable. But as luck would have it, I came across a [through-hole CF socket](https://www.mouser.com/ProductDetail/200-CFT12501LDRA01SL) that is readily available. The end result is an all-through-hole board, with no adapters or edge-soldered pin headers.

### Compatibility
The intention is to support **all** CompactFlash memory cards. All cards *should* work - please let me know if you have one that doesn't work with the board. Software-wise, the board is compatible with the CP/M loaders and distributions listed above. The mentioned systems have their own requirements - refer to their documentation for dependencies and instructions. CP/M will only with if you have compatible memory boards - examples are the official RC2014 [RAM](https://rc2014.co.uk/modules/64k-ram/) and [ROM](https://rc2014.co.uk/modules/pageable-rom/) boards, the RC2014 [512K+512K RAM+ROM](https://rc2014.co.uk/modules/512k-rom-512k-ram-module/) board, my [PD101](https://github.com/PickledDog/rc-z80ram) and [PD104](https://github.com/PickledDog/rc-rom) boards, my [PD106](https://github.com/PickledDog/rc-ramrom) board, or Rob Dobsons' [RAMRaider](https://github.com/robdobsn/LinearPagedRAMROM1MB) board. Alternatively, motherboards that have pageable memory systems built-in will work, such as Stephen C Cousins' [SC114](https://smallcomputercentral.wordpress.com/sc114-documentation/), [SC126](https://smallcomputercentral.wordpress.com/sc126-z180-motherboard-rc2014/), and [SC130](https://smallcomputercentral.wordpress.com/sc130-z180-motherboard/). The official 32K RAM, 8K ROM, Switchable ROM, RC2014 Mini/Micro, and boards from the [Classic II](https://rc2014.co.uk/full-kits/rc2014-classic-ii/) kit will *not* work.

## Bus connection
Enhanced bus connections are provided, but the extra pins are solely used to provide stronger ground and power connections. As such, the wider connector is optional, but recommended if you are using an Enhanced backplane.

## Note on assembly
The CF socket lacks metal pins or other mounting tabs, providing only plastic pegs on its underside. Gaps in the pegs are intended for solder to flow into during... I guess wave soldering?

![Samtec datasheet snippet](/img/mounting.png)

It seems to rely on solder melting and solidifying in the "hook" of the peg. There's no way to feed this solder in from the top, but I had some luck hand-inserting dry solder before pushing the socket down and heating the mounting holes with the iron.

## Part selection
Bill Of Materials and part references are below. If you want the 50-pin RC2014 Enhanced bus connector, you'll need to make it from a 2x40-pin right-angle header and pull out most of the pins from the top row (needle-nose pliers work well). I recommend using gold-plated header for this - I use these ones from [Pololu](https://www.pololu.com/product/2668) or [Sparkfun](https://www.sparkfun.com/products/12792) (these are cheaper than the Mouser listing below).

The specified parts are just the ones I used, and can be substituted as needed - Mouser links provided for convenience and reference.

| Reference | Value | Qty | Mouser link |
| --------- | ----- | --- | ----------- |
| C1-C3 | 100nF ceramic | 3 | [KEMET C315C104M5U5TA](https://www.mouser.com/ProductDetail/C315C104M5U5TA7303) |
| C4 | 10μF ceramic | 1 | [TDK FG18X5R1E106MRT06](https://www.mouser.com/ProductDetail/FG18X5R1E106MRT06) |
| D1 | 3mm red LED | 1 | [Lite-On LTL-4221N](https://www.mouser.com/ProductDetail/LTL-4221N) |
| J1/2 | 2x40 right-angle header | 1 | [3M 2380-5121TG](https://www.mouser.com/ProductDetail/2380-5121TG) |
| J3 | CompactFlash socket | 1 | [Samtec CFT-125-01-L-D-RA-01-SL](https://www.mouser.com/ProductDetail/200-CFT12501LDRA01SL) |
| R1 | 330Ω resistor | 1 | [Yageo CFR-25JT-52-330R](https://www.mouser.com/ProductDetail/CFR-25JT-52-330R) |
| RN1 | Bussed 1kΩ×4 resistor | 1 | [Bourns 4605X-AP1-102LF](https://www.mouser.com/ProductDetail/4605X-AP1-102LF) |
| U1 | 74AHCT138 | 1 | [TI SN74AHCT138N](https://www.mouser.com/ProductDetail/SN74AHCT138N) |
| U2 | 74HCT32 | 1 | [TI SN74HCT32N](https://www.mouser.com/ProductDetail/SN74HCT32N) |
| U3 | 74HC245 | 1 | [TI SN74HC245N](https://www.mouser.com/ProductDetail/SN74HC245N) |