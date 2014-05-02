#STM32L1-Discovery Application Template
This package is for use when compiling programs for STM32L152xx ARM microcontrollers using arm-none-eabi-gcc. The Makefile in the main directory will call the Make file in the Libraries directory, thereby automatically building the STM peripheral library. However, running 'make clean' will not affect the peripherals library (the same command can be run from the Libraries directory to do this).

This template will serve as a quick-start for those who do not wish to use an IDE, but rather develop in a text editor of choice and build from the command line. This is based on the basic template from Mike Szczys found at https://github.com/szczys/stm32f0-discovery-basic-template

##Subfolders:

1. Library/
   * This is the Library/ folder from the STM32L1xx_StdPeriph_Lib_V1.1.1 standard peripheral driver library produced by STM. This preserves the original structure which should make it easy to roll in library upgrades as they are published
   * **Makefile** is not part of the STM release, and must be copied over if the library is upgraded.
   * **stm32l1xx_conf.h** is used to configure the peripheral library. It must be copied here if the library is upgraded. The file was file taken from the STM32L1-Discovery firmware package. It is found in the following directory:
      * Project/Demonstration/
   * **Abstracting the libraries:** You may place this folder anywhere you like in order to use it for multiple projects. Just change the path of the STD_PERIPH_LIB variable in the Makefile

2. Device/
   * Folder contains device specific files:
   * **startup_stm32l1xx_md.s** is the startup file taken from the STM32L1-Discovery firmware package. It is found in the following directory:
      * Libraries/CMSIS/Devices/ST/STM32L1xx/Source/Templates/TrueSTUDIO/
   * Linker Scripts (Device/ldscripts)
      * These linker scripts are used instead of the stm32_flash.ld script which is included in the STM demo code. This is because the original file contains an unreasonably restrictive copyright assertion.

3. inc/
   * All include files for this particular project.

4. src/
   * All source files for this particular project (including main.c).
   * **system_stm32l1xx.c** can be generated using an XLS file developed by STM. This sets up the system clock values for the project. The file included in this repository is taken from the STM32F0-Discovery firmware package. It is found in the following directory:
      * Libraries/CMSIS/ST/STM32L1xx/Source/Templates/


5. extra/ 
   * This contains a procedure file used to write the image to the board via OpenOCD **Currently Broken**
   * **Abstracting the extra folder:** the .cfg file in the extra folder may be placed anywhere so that multiple projects can use one file. Just change the OPENOCD_PROC_FILE variable in the Make file to match the new location.

##Loading the image on the board

**Loading via openocd is currently broken**, load via arm-none-eabi-gdb

###UDEV Rule for the Discovery Board

If you are not able to communicate with the STM32F0-Discovery board without root privileges you should follow the step from [the stlink repo readme file](https://github.com/texane/stlink#readme) for adding a udev rule for this hardware.

##Compiling your own toolchain
It might be best to use a precompiled toolchain liked CodeSourcery G++: Lite Edition. But if you would prefer to compile your own, give [this guide](http://www.kunen.org/uC/gnu_tool.html) a try. Just google for the source code to make sure you're using the most recent versions. GCC now comes with the core and g++ code all in one archive.

###Possible compiling errors:
   * You may encouter unfulfilled dependecies when it comes to GMP, MPFR and MPC. According to [the GCC installation Wiki](http://gcc.gnu.org/wiki/InstallingGCC) you should install the following packages: libgmp-dev libmpfr-dev libmpc-dev. If that doesn't work, read the linked Wiki for further options.
   * If you get the error: "configure: error: Link tests are not allowed after GCC_NO_EXECUTABLES" try adding the following flags when configuring GCC: "--with-system-zlib --disable-shared"
