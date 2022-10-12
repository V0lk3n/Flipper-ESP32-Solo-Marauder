# Flipper ESP32 Solo Marauder

This repository as for goal to explain how to compile and flash Marauder framework to an ESP32 Solo core to work on a Flipper Zero.

The main problem to flash marauder on my ESP was because marauder is mainly destined on ESP32 multi core. As my board was one core only, i got backtrace corrupted error while trying the OTA and Source Code methode. Or wrong chip id while trying using FZ Easy Marauder.

Here is how to make it work on a one core ESP32.

# Table of Content
* [Tested Stuff](#TestedStuff)
  * [Tested Device](#TestedDevice)
  * [Tested Environment](#TestedEnvironment)
* [Compilation and Flashing](#CompileAndFlash)
  * [Install Arduino IDE v.1.8.19](#InstallArduinoIDE)
  * [Modify ESP32 Library with the Solo Library](#ModifiyESP32Lib)
  * [Install Libraries](#InstallLibraries)
  * [Install ESP32 Fileserver Upload](#InstallESP32FS)
  * [Install Marauder From Sources](#InstallMarauder)
* [Special Thanks](#SpecialThanks)
  
## Tested Stuff<a name="TestedStuff"></a>

### Tested Device<a name="TestedDevice"></a>

Please feel free to make PR once you test on a different board.

- ESP32-DevKitM-1

### Tested Environment<a name="TestedEnvironment"></a>

Please feel free to make PR once you test on a different Environment.

* Windows 11
* Arduino IDE v.1.8.19
* ESP32 Library v.2.0.2
* ESP32 Solo Library v.2.0.2

## Compilation and Flashing<a name="CompileAndFlash"></a>

### Install Arduino IDE v.1.8.19<a name="InstallArduinoIDE"></a>

1.  Install and open the release version 1.8.19 of [Arduino IDE](https://www.arduino.cc/en/main/software)
2.  In the Arduino IDE, go to `File`>`Preferences`
3.  Add the following URLs to Additional Boards Manager URLs:
    -   [https://dl.espressif.com/dl/package_esp32_index.json](https://dl.espressif.com/dl/package_esp32_index.json)
    -   [https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_dev_index.json](https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_dev_index.json)
4.  Go to `Tools`>`Board`>`Boards Manager`, search for `esp32` and install `esp32 by Espressif Systems`
    -   Make sure it is version `2.0.2`
5.  Install the [CP210X Drivers](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers)
6.  Install the [CH340X Drivers](https://github.com/justcallmekoko/ESP32Marauder/blob/master/Drivers/CH34x_Install_Windows_v3_4.EXE)
6.  With any text editor, open `C:\Users\<USERNAME>\AppDate\Local\Arduino15\packages\esp32\hardware\esp32\2.0.2\platform.txt`
6.  Add `-w` to the end of line `33` so it appears like so
    -   `build.extra_flags.esp32=-DARDUINO_SERIAL_PORT=0 -w`
7.  Add `-zmuldefs` to the end of line `27` (`compiler.c.elf.libs.esp32=`)

### Modify ESP32 Library with the Solo Library<a name="ModifyESP32Lib"></a>

1. Download the source code of  [`Arduino ESP32 Solo`](https://github.com/lbernstone/arduino-esp32-solo) which match our library version (2.0.2). You can download it [here](https://github.com/lbernstone/arduino-esp32-solo/releases/tag/v2.0.2)
2. Unzip it and move the `tools` folder into your ESP32 Library (should be located at : `C:\Users\YourUsername\AppData\Local\Arduino15\packages\esp32\hardware\esp32\2.0.2`), replace every files when asked.
3. Close and re-open Arduino IDE.

### Install Libraries<a name="InstallLibraries"></a>

Download and install the following libraries using the ZIP library installer under `Sketch`>`Library`>`Add .ZIP Library`

-   [lv_arduino](https://github.com/lvgl/lv_arduino)
-   [LinkedList](https://github.com/ivanseidel/LinkedList)
-   [TFT_eSPI](https://github.com/justcallmekoko/TFT_eSPI)
-   [JPEGDecoder library](https://github.com/Bodmer/JPEGDecoder)
-   [NimBLE](https://github.com/h2zero/NimBLE-Arduino)
-   [NeoPixel](https://github.com/adafruit/Adafruit_NeoPixel)
-   [ArduinoJson](https://github.com/bblanchon/ArduinoJson/releases/tag/v6.18.2)
-   [SwitchLib](https://github.com/justcallmekoko/SwitchLib/releases/latest)

### Install ESP32 filesystem uploader<a name="InstallESP32FS"></a> 

-   Make sure you use one of the supported versions of Arduino IDE and have ESP32 core installed.
-   Download the tool archive from [releases page](https://github.com/me-no-dev/arduino-esp32fs-plugin/releases/latest).
-   Unpack the tool into tools directory, create it if it doesn't exist (by default, the path will look like `C:\Users\YourUsername\Documents\Arduino\tools\ESP32FS\tool\esp32fs.jar`).
-   Restart Arduino IDE.

### Install Marauder from Sources<a name="InstallMarauder"></a>

1. Download the whole repository. You can Download it [here](https://github.com/justcallmekoko/ESP32Marauder)
2. Open `ESP32Marauder\esp32_marauder\esp32_marauder.ino`, it will load the whole project into you'r Arduino IDE.
3.  Modify `config.h`, to match you'r hardware version. In my case i needed to pick "Marauder Flipper", scroll a bit to know why.

```C++
//#define MARAUDER_MINI
//#define MARAUDER_V4
//#define MARAUDER_V6
//#define MARAUDER_KIT
//#define GENERIC_ESP32
#define MARAUDER_FLIPPER
```


NOTE : In my first attempt, i've chosen "Marauder Mini" as my board is based on ESP32-Mini. But then flashing it failed (I guess because of SD card that isn't here, and maybe TFT Sceen).  
At my second attempt, i've chosen "Marauder Flipper", asking me "Well, you want to make it work for flipper zero, not to become the Marauder Mini Device, so why not?" and it worked. (Also, there is no SD Card on the Flipper Wifi Dev Board, and no TFT Screen)
 

4. Press the `BOOT` button of you'r ESP32 and plug it into a USB port.
5. Under `Tools` menu :
    - Select the COM port under `Tools`>`Port`, then allways under `Tools` select the following.
    - Select the board `Node32s`
    - Upload Speed set to `115200`
    - Flash Frequency set to `80Mhz`
    - Partition Scheme set to `Minimal SPIFFS (Large APPS with OTA)`
6. Under `Tools` menu, click `ESP32 Sketch Data Upload` and wait for the SPIFFS upload to finish.
7. Copy the `User_Setup.h` that you can find in the downloaded marauder repo at `ESP32Marauder/User_Setup.h`, and replace the one in your TFT_eSPI library folder located at `C:\Users\YourUsername\Documents\Arduino\libraries\TFT_eSPI` 
8. Click the upload button, it will compile the sketch and flash it on you'r ESP32. Once finished the board will automatically reboot.
9. Try to connect with putty to make sure that Marauder is running correctly.

```PowerShell
putty.exe -serial <COM_PORT> -sercfg 115200,8,n,1,N
```

## Special Thanks<a name="SpecialThanks"></a>

Thanks to [@justcallmekoko](https://github.com/justcallmekoko) for the [ESP32Marauder firmware](https://github.com/justcallmekoko/ESP32Marauder).  
Thanks to [@lbernstone](https://github.com/lbernstone) for the [arduino esp32 solo library](https://github.com/lbernstone/arduino-esp32-solo).  
Thanks to [@me-no-dev](https://github.com/me-no-dev) for the [arduino esp32 file system plugin](https://github.com/me-no-dev/arduino-esp32fs-plugin).  
