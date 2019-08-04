# ruuvi.nrf5_sdk15_bootloader.c
Source code for Nordic Semiconductor SDK15 / Softdevice 6.1.1 bootloaders used by Ruuvi.
The project is essentially nRF5 SDK15.3 `secure_bluetooth` examples with a few configuration constants and naming changed.

*Note* The start address of RuuviTag B bootloader flash is 0x75000 to maintain compatibility with RuuviTag 12.3 bootloader. SDK debug example ships with 0x72000 and production ships with 0x78000.

Kaarle bootloader starts at 0x78000, and all bootloaders are production variants.

# Bootloader settings
All bootloaders can be activated via general purpose retention registers and buttonless DFU service. 
None of the bootloader has downgrade prevention enabled to allow users to switch between different applications.
This does pose a security risk, as attacker can wear out the flash with repeated firmware updates. 
Be sure to secure the ways to enter bootloader. 

The bootloader advertises itself as "RuuviBoot".

## RuuviTag B
RuuviTag B can enter bootloader via button press. If button B is pressed on boot the bootloader is activated.
- Hardware version is 0xB0, this is required in the DFU generation
- 20480 (0x5000) bytes are reserved for application data in flash. 
- Bootloader start address is 0x75000

## Kaarle
Kaarle enters bootloader after pinreset.
- Hardware version is 0xCA
- 32768 (0x8000) bytes are reserved for application data in flash. 
- Bootloader start address is 0x78000

## Keijo
- Hardware version is 0xCE
- 8192 (0x2000) bytes are reserved for application data in flash.
- Bootloader start address is 0x28000

# Setting up the project
Download [Nordic Semiconductor SDK 15.3](http://developer.nordicsemi.com/nRF5_SDK/) and unzip at the same level as this git repository, i.e.

- nrf_sdk15.3
- ruuvi.nrf5_sdk15_bootloader.c

# Compiling
The project can be compiled with [Segger Embedded Studio](https://www.segger.com/).

Select debug version in build options to skip version checks on upload, or generate your own keypair with:
```
nrfutil keys generate private.pem
nrfutil keys display --key pk --format code private.pem
```

and use production version. By default Ruuvi's `ruuvi_open_private.pem` signing key is used, which means
that anyone can create a package for your device.

# Usage
Flash the bootloader and Softdevice s132 6.1.1 to RuuviTag.

## Compiling apps for the bootloader
When you compile app for targeted bootloader be sure to generate the proper settings for the bootloader
and merge the generated .hex with bootloader:
- TODO example wired flash
- TODO example DFU packet

Be especially careful with the linker settings of the app: Verify that your app size does not extend to bootloader. 
Generally for RuuviTag B you'll want to define flash start as 0x26000 (after softdevice) and flash end at 0x74FFF (before bootloader). Flash size would then be 0x75000 - 0x26000 = 0x4F000 (zeroth byte counts as space). 

The bootloader will not overwrite area from 0x70000 upwards, inclusive. Store any persistent application data there. 

# License
Project is under the nRF5_Nordic_license.txt
