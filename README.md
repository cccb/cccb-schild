![maintenance](https://img.shields.io/maintenance/yes/2023?style=for-the-badge)
![license](https://img.shields.io/github/license/cccb/cccb-schild?style=for-the-badge)
[![WLED GUI](https://img.shields.io/badge/-WLED%20GUI-blue?style=for-the-badge)](http://schild.club.berlin.ccc.de)

# CCCB street sign

This is the documentation for the CCCB street sign internals. Here you can figure out how the electronics work, if for example the magic smoke escaped and you need to fix it. It also describes how the firmware works and has a backup of a working state.

# Electronics

[![connection diagram for the electronics](https://github.com/cccb/cccb-schild/raw/main/electronics.drawio.svg)](https://app.diagrams.net/?mode=github#Hcccb%2Fcccb-schild%2Fmain%2Felectronics.drawio.svg)

The Microcontroller used is a [Olimex ESP32-POE-ISO-EA-IND](https://www.olimex.com/Products/IoT/ESP32/ESP32-POE-ISO/open-source-hardware) which supports Ethernet and POE. This simplified the power management and connectivity.

Instead of a normal level shifter, a [Diodes 74AHCT1G126](https://www.diodes.com/assets/Datasheets/74AHCT1G126.pdf) is used. Don't ask me why this works.

For the LEDs, WS2815 was choosen. They operate with 12V power so the strips can be as long as needed without complicated power feeds. The [LEDs](https://www.aliexpress.com/item/32961181562.html) and [power supply](https://www.aliexpress.com/item/4001260747482.html) are from [BTF Lightning](https://btf-lighting.aliexpress.com/).

Both sites should display the same "content", so they are connected to a single GPIO on the micro controller. This should mirror the output to both sites. All electronic components have a layer of [Kontakt Chemie PLASTIK 70](http://www.kontaktchemie.com/koc/KOCproductdetail.csp?division=&product=PLASTIK%2070&ilang=en&plang=en) on them to be weather proof. The diffusing material is 3mm thick acrylic glas. The CCCB logo is produced by foil on top of the acrylic glas.

# Firmware

The firmware used is [WLED](https://kno.wled.ge/). 

These instruction are proably outdated, so please check with the official guide [here](https://kno.wled.ge/basics/install-binary/).

## Flash via web

Go to `https://install.wled.me/` and choose the latest version. Also check `My board has Ethernet`. Then follow the procedure.

## Flash via terminal

If you don't trust WebUSB (and you shouldn't), get the latest firmware release from [Github](https://github.com/Aircoookie/WLED/releases). The file you need is called `WLED_0.13.3_ESP32_Ethernet.bin` (with the version being the newest). Also download the newest bootloader file. At the time of writing this is version 4. You can find it [here](https://github.com/Aircoookie/WLED/releases/download/v0.13.1/esp32_bootloader_v4.bin).

Then install [esptool](https://github.com/espressif/esptool). Your Distribution probably comes with it, so try `apt install esptool` or similar.


Flash it to the ESP32 with these commands:

```shell
# Erase the current flash contents (this will also delete your current config)
esptool.py erase_flash
# Flash the bootloader
esptool.py write_flash 0x0 esp32_bootloader_v4.bin
# Flash the WLED firmware
esptool.py write_flash 0x10000 WLED_0.13.3_ESP32_Ethernet.bin
```

When this is done, the ESP32 should open a wifi called `WLED-AP`. The default password is `wled1234`. Connect to it, configure Ethernet and disable wifi. The ESP32 should receive it's designated IP address as long as the MAC address doesn't change. If it does change, tell the DHCP admins the new MAC address so they can update the static DHCP entry.


To be continued...

---

Made with ❤️ and ✨ by [XenGi](https://github.com/xengi).
