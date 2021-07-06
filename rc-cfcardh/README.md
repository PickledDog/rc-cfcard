# rc-cfcardh
 Buffered CompactFlash card for RC2014, female header version
![Assembled PD105H](/img/rc-cfcardh/assembled.jpg)

## Overview
Want something easier to solder, or just prefer the CompactFlash socket to be above the other RC2014 boards? This variant of rc-cfcard uses a female IDE header (and IDE-to-CF adapter) instead of a directly-mounted CF socket. It is otherwise the same as the primary design; refer to [the main project](/) for anything not covered here.

## Part selection
Bill Of Materials and part references are below. The LED and its resistor can be considered optional, since IDE-to-CF adapters usually have an activity LED of their own. Said adapter can be [sourced from eBay](https://www.ebay.com/sch/i.html?_nkw=ide+to+compact+flash+44+pin) - the board is designed for this style of adapter:
![CompactFlash adapter](/img/rc-cfcardh/cf-adapter.jpg)

| Reference | Value | Qty | Mouser link |
| --------- | ----- | --- | ----------- |
| C1-C3 | 100nF ceramic | 3 | [KEMET C315C104M5U5TA](https://www.mouser.com/ProductDetail/C315C104M5U5TA7303) |
| C4 | 10μF ceramic | 1 | [TDK FG18X5R1E106MRT06](https://www.mouser.com/ProductDetail/FG18X5R1E106MRT06) |
| D1 | 3mm red LED | 1 | [Lite-On LTL-4221N](https://www.mouser.com/ProductDetail/LTL-4221N) |
| J1/2 | 2x40 right-angle header | 1 | [3M 2380-5121TG](https://www.mouser.com/ProductDetail/2380-5121TG) |
| J3 | 2x22 female header (2mm) | 1 | [Samtec SQT-122-01-F-D-RA](https://www.mouser.com/ProductDetail/200-SQT12201FDRA) |
| R1 | 470Ω resistor | 1 | [Yageo CFR-25JT-52-470R](https://www.mouser.com/ProductDetail/CFR-25JT-52-470R) |
| RN1 | Bussed 1kΩ×4 resistor | 1 | [Bourns 4605X-AP1-102LF](https://www.mouser.com/ProductDetail/4605X-AP1-102LF) |
| U1 | 74AHCT138 | 1 | [TI SN74AHCT138N](https://www.mouser.com/ProductDetail/SN74AHCT138N) |
| U2 | 74HCT32 | 1 | [TI SN74HCT32N](https://www.mouser.com/ProductDetail/SN74HCT32N) |
| U3 | 74HC245 | 1 | [TI SN74HC245N](https://www.mouser.com/ProductDetail/SN74HC245N) |