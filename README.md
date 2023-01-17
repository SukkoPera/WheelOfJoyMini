# WheelOfJoyMini
WheelOfJoyMini is an Open Hardware 4-player joystick adapter for the Commodore 16, 116 and Plus/4 with 2-button support.

![Board](https://raw.githubusercontent.com/SukkoPera/WheelOfJoyMini/master/img/render-top.png)

## Summary
After I released [WheelOfJoy](https://github.com/SukkoPera/WheelOfJoy), some people suggested that a 4-player adapter with support for 2 joystick buttons would probably be more useful, so here we go :).

Just like its bigger brother, the board plugs in the User Port, which means that in order to use it on a C16 or C116, you will need [a User Port card](https://github.com/SukkoPera/16up).

## Assembly
I recommend soldering all the resistor arrays first, then the chips and the capacitors, with the ports last.

The adapter can be connected to the computer through a cable with a User Port connector on one side and a male DB-15 on the other. I did this so that the adapter can be placed more freely on the table so that all players can reach it comfortably. The adapter-end of the cable can also be soldered directly to it, if you prefer. Pin names are noted on the board, but here's a handy table:

| DB-15 Pin # | Signal | User Port Pin # | Notes                  |
|-------------|--------|-----------------|------------------------|
|1	          | +5V    | 2               |                        |
|2	          | P0     | B               |                        |
|3	          | P1     | K               |                        |
|4	          | P2     | 4               |                        |
|5	          | P3     | 5               |                        |
|6	          | /RESET | 3               | Not used by this board |
|7	          | +9V    | 10              | Not used by this board |
|8	          | GND    | 1, 12, A or N   |                        |
|9	          | +5V    | 2               |                        |
|10	          | P4     | 6               |                        |
|11	          | P5     | 7               |                        |
|12	          | P6     | J               |                        |
|13	          | P7     | F               |                        |
|14	          | -9V    | 11              | Not used by this board |
|15	          | GND    | 1, 12, A or N   |                        |

This is exactly the same cable used by WheelOfJoy.

[This](https://www.thingiverse.com/thing:3368773) and [these](https://www.thingiverse.com/thing:5213203) will help you make a good-looking job with your cable.

I estimate all the components required to self-build this thing to cost < 10€, probably closer to 5€ if you get them from China.

## Supported Games
Of course there isn't any software support for this at the moment. It's up to the developers to write party games for the C16/+4 for up to 6 players!

## Programming
The board uses 3 dual 4-to-1 multiplexers, totalling to 6 multiplexers: one per direction plus one for every fire button. Channel selection happens in parallel on all multiplexers and is done with the 2 high bits of the User Port.

1. Write a value N at the User Port register depending on the port you want to read. General formula for port P ∈ [1,4] is the following:

   ```N = ((P - 1) << 6) | 0x3F```

   Or just pick your value from the following table:

   | # | Binary    | Hex| Dec |
   |---|-----------|----|-----|
   | 1 | 001 11111 | 3F |  63 |
   | 2 | 011 11111 | 7F | 127 |
   | 3 | 101 11111 | BF | 191 |
   | 4 | 111 11111 | FF | 255 |

2. Read the value of the User Port register, the lowest 6 bits will report direction/button status:

   | Bit 7 | Bit 6 | Bit 5  | Bit 4  | Bit 3 | Bit 2 | Bit 1 | Bit 0 |
   |-------|-------|--------|--------|-------|-------|-------|-------|
   |   X   |   X   |Button 2|Button 1| Left  | Right | Down  | Up    |

   Every bit will be 0 if the corresponding button is pressed, 1 otherwise.

## Compatibility
Any Atari-compliant joystick or joypad should work with this board. Button 2 is expected to be wired to pin 9 and to be active-low as all the other buttons. This is the most common setup, used by all 2-button joysticks and adapters designed for the Amiga, including [my PSX adapter](https://github.com/SukkoPera/OpenPSX2AmigaPadAdapter) and the [my variant of the Unijoysticle2](https://gitlab.com/SukkoPera/unijoysticle2).

Sega controllers also use the same configuration: you can use Master System and Mega Drive/Genesis - buttons B and C - controllers straight away as **the adapter also provides power to pin 5**.

The latter also means that you MUST NOT use the adapter with anything that can pull pin 5 straight to ground, such as 3-button Amiga mice (what would be the purpose of that anyway?).

Note that the few 2-button games for the C64 typically use inverted logic for button 2 (don't ask me why but this dates back to the C64 GS, I think), even if this was handled in SW, I cannot guarantee that any joysticks made this way will work correctly with the adapter.

Also keep in mind that the User Port is only guaranteed to provide 100 mA on the +5V pin, even though I don't think there's anything actually limiting that.

## Releases
If you want to get this board produced, you are recommended to get [the latest release](https://github.com/SukkoPera/WheelOfJoyMini/releases) rather than the current git version, as the latter might be under development and is not guaranteed to be working.

Every release is accompanied by its Bill Of Materials (BOM) file and any relevant notes about it, which you are recommended to read carefully.

## License
The WheelOfJoyMini documentation, including the design itself, is copyright &copy; SukkoPera 2019-2021 and is licensed under the [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-nc-sa/4.0/).

This documentation is distributed *as is* and WITHOUT ANY EXPRESS OR IMPLIED WARRANTIES whatsoever with respect to its functionality, operability or use, including, without limitation, any implied warranties OF MERCHANTABILITY, SATISFACTORY QUALITY, FITNESS FOR A PARTICULAR PURPOSE or infringement. We expressly disclaim any liability whatsoever for any direct, indirect, consequential, incidental or special damages, including, without limitation, lost revenues, lost profits, losses resulting from business interruption or loss of data, regardless of the form of action or legal theory under which the liability may be asserted, even if advised of the possibility or likelihood of such damages.

## Support the Project
If you want to get some boards manufactured, you can get them from PCBWay through this link:

[![PCB from PCBWay](https://www.pcbway.com/project/img/images/frompcbway.png)](https://www.pcbway.com/project/shareproject/WheelOfJoyMini_Commodore_16_116_4_4_Player_Joystick_Adapter_With_2_Button_Sup_d3f12122.html)

You get my gratitude and cheap, professionally-made and good quality PCBs, I get some credit that will help with this and [other projects](https://www.pcbway.com/project/member/shareproject/?bmbid=41100). You won't even have to worry about the various PCB options, it's all pre-configured for you!

Also, if you still have to register, [you can use this link](https://www.pcbway.com/setinvite.aspx?inviteid=41100) to get some bonus initial credit (and yield me some more).

You can also buy me a coffee if you want:

<a href='https://ko-fi.com/L3L0U18L' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://az743702.vo.msecnd.net/cdn/kofi2.png?v=2' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>

## Credits
* Thanks to siz for the idea.
* Thanks to Solder for being such a prolific hacker and a great inspiration :).
* 3D model of [DB-9](https://grabcad.com/library/d-sub-9-pin-male-1) and [DB-15 connector](https://grabcad.com/library/d-sub-15-pin-female-2-lines-1) by Alex K.
* Font used for port numbers is [Agreement Signature by wepfont](https://www.fontspace.com/a-agreement-signature-font-f52534) (800 DPI).
* Font used for the logo is [Senja Santuy by Zuzulgo](https://www.fontspace.com/senja-santuy-font-f74971) (450 DPI).
