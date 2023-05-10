System Requirement
==================
STM32F401 Black Pill - USB type C cable
UART PA2 PA3 - USB to serial ???
STLink-V2
Install HoRNDIS on macOS, Linux and Windows should work out-of-the-box


Building LwIP Webserver Example
===============================

This tutorial gets you started immediately to test LwIP webserver application on STM32F4 Discovery.

1. Clone this repository to your local machine:
    ```
    git clone https://github.com/ekawahyu/tinyusb
    ```

2. Change into directory and checkout branch `dev/ekasusilo-lwip-webserver`:
    ```
    cd tinyusb
    git checkout dev/ekasusilo-lwip-webserver
    ```

3. Initialize and update submodules:
    ```
    git submodule init
    git submodule update hw/mcu/st/stm32f4xx_hal_driver
    git submodule update hw/mcu/st/cmsis_device_f4
    git submodule update lib/CMSIS_5
    git submodule update lib/lwip
    ```

4. Build the project:
    ```
    cd examples/device/net_lwip_webserver/
    make DEBUG=1 BOARD=stm32f401blackpill all
    ```
    You can remove `DEBUG=1` when building the release version.

5. Flash the binary with STLink-V2:
    ```
    openocd -f interface/stlink.cfg -f target/stm32f4x.cfg -c "program _build/stm32f401blackpill/net_lwip_webserver.bin reset exit 0x08000000"
    ```

6. For debugging, flash the binary but do not exit from OpenOCD:
    ```
    openocd -f interface/stlink.cfg -f target/stm32f4x.cfg -c "program _build/stm32f401blackpill/net_lwip_webserver.bin reset 0x08000000"
    ```
7. Start OpenOCD server:
    ```
    arm-none-eabi-gdb _build/stm32f401blackpill/net_lwip_webserver.elf
    (gdb) target extended-remote localhost:3333
    (gdb) monitor [target current] configure -event gdb-detach {shutdown}
    (gdb) tui enable
    ```

8. Press `r` to run, `s` to step into, or `n` to step over.