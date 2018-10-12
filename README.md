# ruuvi.nrf5_sdk15_bootloader.c
Source code for Nordic Semiconductor SDK15 / Softdevice 6.1.0 bootloaders used by Ruuvi
The project is essentially nrf5 SDK15.2 secure_bluetooth examples with a few configuration constants and naming changed.

*Note* The start address of flash is 0x75000 to maintain compatibility with RuuviTag 12.3 bootloader. SDK example ships with 0x72000.
Some logging has been disabled to clear up the space. 

# Setting up the project
Download [Nordic Semiconductor SDK 15.2]() and unzip at the same level as this git repository, i.e.

- nrf_sdk15.2
- ruuvi.nrf5_sdk15_bootloader.c

# Compiling
The project can be compiled with [Segger Embedded Studio]()

# Usage
Flash the bootloader and Softdevice s132 6.1.0 to RuuviTag

# License
Project is under the nRF5_Nordic_license.txt
