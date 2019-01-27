
### TFT library for TTGO TS V1.2.

---
**This library is optimized for Eclipse as it has more features compared to Arduino IDE**
**This library must be built with the latest esp-idf master branch and xtensa toolchain**

If you are using the esp-idf v2.1, checkout the commit *0518df81a6566820352dad7bf6c539995d41ad18*

---

#### Features
* Support for **ST7735** based TFT modules in 4-wire SPI mode. Support for other controllers will be added later
* **18-bit (RGB)** color mode used
* **SPI displays oriented SPI driver library** based on *spi-master* driver
* Combined **DMA SPI** transfer mode and **direct SPI** for maximal speed
* **Grayscale mode** can be selected during runtime which converts all colors to gray scale
* SPI speeds up to **40 MHz** are tested and works without problems
* **Demo application** included which demonstrates most of the library features


* **Graphics drawing functions**:
  * **TFT_drawPixel**  Draw pixel at given x,y coordinates
  * **TFT_drawLine**  Draw line between two points
  * **TFT_drawFastVLine**, **TFT_drawFastHLine**  Draw vertical or horizontal line of given lenght
  * **TFT_drawLineByAngle**  Draw line on screen from (x,y) point at given angle
  * **TFT_drawRect**, **TFT_fillRect**  Draw rectangle on screen or fill given rectangular screen region with color
  * **TFT_drawRoundRect**, **TFT_fillRoundRect**  Draw rectangle on screen or fill given rectangular screen region with color with rounded corners
  * **TFT_drawCircle**, **TFT_fillCircle**  Draw or fill circle on screen
  * **TFT_drawEllipse**, **TFT_fillEllipse**  Draw or fill ellipse on screen
  * **TFT_drawTriangel**, **TFT_fillTriangle**  Draw or fill triangle on screen
  * **TFT_drawArc**  Draw circle arc on screen, from ~ to given angles, with given thickness. Can be outlined with different color
  * **TFT_drawPolygon**  Draw poligon on screen with given number of sides (3~60). Can be outlined with different color and rotated by given angle.
* **Fonts**:
  * **fixed** width and proportional fonts are supported; 8 fonts embeded
  * unlimited number of **fonts from file**
  * **7-segment vector font** with variable width/height is included (only numbers and few characters)
  * Proportional fonts can be used in fixed width mode.
  * Related functions:
    * **TFT_setFont**  Set current font from one of embeded fonts or font file
    * **TFT_getfontsize**  Returns current font height & width in pixels.
    * **TFT_getfontheight**  Returns current font height in pixels.
    * **set_7seg_font_atrib**  Set atributes for 7 segment vector font
    * **getFontCharacters**  Get all font's characters to buffer
* **String write function**:
  * **TFT_print**  Write text to display.
    * Strings can be printed at **any angle**. Rotation of the displayed text depends on *font_ratate* variable (0~360)
    * if *font_transparent* variable is set to 1, no background pixels will be printed
    * If the text does not fit the screen/window width it will be clipped ( if *text_wrap=0* ), or continued on next line ( if *text_wrap=1* )
    * Two special characters are allowed in strings: *\r* CR (0x0D), clears the display to EOL, *\n* LF (ox0A), continues to the new line, x=0
    * Special values can be entered for X position:
      * *CENTER*  centers the text
      * *RIGHT*   right justifies the text horizontaly
      * *LASTX*   continues from last X position; offset can be used: *LASTX+n*
    * Special values can be entered for Y:
      * *CENTER*  centers the text verticaly
      * *BOTTOM*  bottom justifies the text
      * *LASTY*   continues from last Y position; offset can be used: *LASTY+n*
  * **TFT_getStringWidth** Returns the string width in pixels based on current font characteristics. Useful for positioning strings on the screen.
  * **TFT_clearStringRect** Fills the rectangle occupied by string with current background color
* **Images**:
  * **TFT_jpg_image**  Decodes and displays JPG images
    * Limits:
      * Baseline only. Progressive and Lossless JPEG format are not supported.
      * Image size: Up to 65520 x 65520 pixels
      * Color space: YCbCr three components only. Gray scale image is not supported.
      * Sampling factor: 4:4:4, 4:2:2 or 4:2:0.
    * Can display the image **from file** or **memory buffer**
    * Image can be **scaled** by factor 0 ~ 3  (1/1, 1/2, 1/4 or 1/8)
    * Image is displayed from X,Y position on screen/window:
      * X: image left position; constants CENTER & RIGHT can be used; *negative* value is accepted
      * Y: image top position;  constants CENTER & BOTTOM can be used; *negative* value is accepted
  * **TFT_bmp_image**  Decodes and displays BMP images
    * Only uncompressed RGB 24-bit with no color space information BMP images can be displayed
    * Can display the image **from file** or **memory buffer**
    * Image can be **scaled** by factor 0 ~ 7; if scale>0, image is scaled by factor 1/(scale+1)
    * Image is displayed from X,Y position on screen/window:
      * X: image left position; constants CENTER & RIGHT can be used; *negative* value is accepted
      * Y: image top position;  constants CENTER & BOTTOM can be used; *negative* value is accepted
* **Window functions**:
  * Drawing on screen can be limited to rectangular *window*, smaller than the full display dimensions
  * When defined, all graphics, text and image coordinates are translated to *window* coordinates
  * Related functions
    * **TFT_setclipwin**  Sets the *window* area coordinates
    * **TFT_resetclipwin**  Reset the *window* to full screen dimensions
    * **TFT_saveClipWin**  Save current *window* to temporary variable
    * **TFT_restoreClipWin**  Restore current *window* from temporary variable
    * **TFT_fillWindow**  Fill *window* area with color
* **Touch screen** supported (for now only **XPT2046** controllers)
  * **TFT_read_touch**  Detect if touched and return X,Y coordinates. **Raw** touch screen or **calibrated** values can be returned.
    * calibrated coordinates are adjusted for screen orientation.
* **Read from display memory** supported
  * **TFT_readPixel**  Read pixel color value from display GRAM at given x,y coordinates.
  * **TFT_readData**  Read color data from rectangular screen area
* **Other display functions**:
  * **TFT_fillScreen**  Fill the whole screen with color
  * **TFT_setRotation**  Set screen rotation; PORTRAIT, PORTRAIT_FLIP, LANDSCAPE and LANDSCAPE_FLIP are supported
  * **TFT_invertDisplay**  Set inverted/normal colors
  * **TFT_compare_colors**  Compare two color structures
  * **disp_select()**  Activate display's CS line
  * **disp_deselect()**  Deactivate display's CS line
  * **find_rd_speed()**  Find maximum spi clock for successful read from display RAM
  * **TFT_display_init()**  Perform display initialization sequence. Sets orientation to landscape; clears the screen. SPI interface must already be setup, *tft_disp_type*, *_width*, *_height* variables must be set.
  * **HSBtoRGB**  Converts the components of a color, as specified by the HSB model to an equivalent set of values for the default RGB model.
  * **TFT_setGammaCurve()** Select one of 4 Gamma curves
* **compile_font_file**  Function which compiles font c source file to font file which can be used in *TFT_setFont()* function to select external font. Created file have the same name as source file and extension *.fnt*


* **Global wariables**
  * **orientation**  current screen orientation
  * **font_rotate**  current font rotate angle (0~395)
  * **font_transparent**  if not 0 draw fonts transparent
  * **font_forceFixed**  if not zero force drawing proportional fonts with fixed width
  * **text_wrap**  if not 0 wrap long text to the new line, else clip
  * **_fg**  current foreground color for fonts
  * **_bg**  current background for non transparent fonts
  * **dispWin** current display clip window
  * **_angleOffset**  angle offset for arc, polygon and line by angle functions
  * **image_debug**  print debug messages during image decode if set to 1
  * **cfont**  Currently used font structure
  * **TFT_X**  X position of the next character after TFT_print() function
  * **TFT_Y**  Y position of the next character after TFT_print() function
  * **tp_calx**  touch screen X calibration constant
  * **tp_caly**  touch screen Y calibration constant
  * **gray_scale**  convert all colors to gray scale if set to 1
  * **max_rdclock**  current spi clock for reading from display RAM
  * **_width** screen width (smaller dimension) in pixels
  * **_height** screen height (larger dimension) in pixels
  * **tft_disp_type**  current display type (DISP_TYPE_ILI9488 or DISP_TYPE_ILI9341)

---

Full functions **syntax and descriptions** can be found in *tft.h* and *tftspi.h* files.

Full **demo application**, well documented, is included, please **analyze it** to learn how to use the library functions.

---

#### Connecting the display - TTGO TS V1.2 specific configuration

| ESP32 pin | Display module | Notes |
| - | - | - |
| 23 | MOSI | SPI input on Display module |
| 19 | MISO | SPI output from Display module, optional |
| 5 | SCK | SPI clock input on Display module |
| 16 | CS  | SPI CS input on Display module |
| 17 | DC  | DC (data/command) input on Display module |
| GND | GND  | Power supply ground |
| 3.3V or +5V | Vcc  | Power supply positive |

**Make shure the display module has 3.3V compatible interface, if not you must use level shifter!**

---

Using *make menuconfig* **select tick rate 1000** ( → Component config → FreeRTOS → Tick rate (Hz) ) to get more accurate timings

---

#### How to build

Configure your esp32 build environment as for **esp-idf examples**

Clone the repository

`https://github.com/nf9/TTGO_TS_V1.2_TFT.git`

Execute menuconfig and configure your Serial flash config and other settings. Included *sdkconfig.defaults* sets some defaults to be used.

Navigate to **TFT Display DEMO Configuration** and set **SPIFFS** options.

Select if you want to use **wifi** (recommended) to get the time from **NTP** server and set your WiFi SSID and password.

`make menuconfig`

**Now the Eclipse part**. Please follow this link to import the project:
-Windows 'https://docs.espressif.com/projects/esp-idf/en/stable/get-started/eclipse-setup-windows.html'
-Linux/Mac 'https://docs.espressif.com/projects/esp-idf/en/stable/get-started/eclipse-setup.html'
Please note that you need to run make 'make menuconfig' first before buildong on Eclipse. Then, build it from Eclipse so that Eclipse will resolve the path to all your includes. You might need to clean the build if build is already done outside of Eclipse.

Make and flash the example.

You could add new build target named 'flash' in your Eclipse project to automatically flash your build to the device.

---

#### Prepare **SPIFFS** image

*The demo uses some image and font files and it is necessary to flash the spiffs image*

**To flash already prepared image to flash** execute:

`make copyfs`

---

You can also prepare different SFPIFFS **image** and flash it to ESP32. *This feature is only tested on Linux.*

Files to be included on spiffs are already in **components/spiffs_image/image/** directory. You can add or remove the files you want to include.

Then execute:

`make makefs`

to create **spiffs image** in *build* directory **without flashing** to ESP32

Or execute:

`make flashfs`

to create **spiffs image** in *build* directory and **flash** it to ESP32

---


---

**Example output:**

```
[0;32mI (425) cpu_start: Pro cpu up.[0m
[0;32mI (428) cpu_start: Application information:[0m
[0;32mI (433) cpu_start: Project name:     tft_demo[0m
[0;32mI (438) cpu_start: App version:      2aa16a2[0m
[0;32mI (443) cpu_start: Compile time:     22:05:18[0m
[0;32mI (448) cpu_start: Compile date:     Jan 24 2019[0m
[0;32mI (453) cpu_start: ESP-IDF:          v3.3-beta1-223-ga62cbfe[0m
[0;32mI (460) cpu_start: Starting app cpu, entry point is 0x40081138[0m
[0;32mI (0) cpu_start: App cpu up.[0m
[0;32mI (470) heap_init: Initializing. RAM available for dynamic allocation:[0m
[0;32mI (477) heap_init: At 3FFAE6E0 len 00001920 (6 KiB): DRAM[0m
[0;32mI (483) heap_init: At 3FFBBD08 len 000242F8 (144 KiB): DRAM[0m
[0;32mI (489) heap_init: At 3FFE0440 len 00003AE0 (14 KiB): D/IRAM[0m
[0;32mI (496) heap_init: At 3FFE4350 len 0001BCB0 (111 KiB): D/IRAM[0m
[0;32mI (502) heap_init: At 40092D78 len 0000D288 (52 KiB): IRAM[0m
[0;32mI (508) cpu_start: Pro cpu start user code[0m
[0;32mI (79) cpu_start: Starting scheduler on PRO CPU.[0m
[0;32mI (0) cpu_start: Starting scheduler on APP CPU.[0m


==============================

TFT display DEMO, LoBo 11/2017

==============================

Pins used: miso=19, mosi=23, sck=5, cs=16

==============================



SPI: display device added to spi bus (1)

SPI: attached display device, speed=8000000

SPI: bus uses native pins: false

SPI: display init...

OK

SPI: Max rd speed = 1000000

SPI: Changed speed to 26666666



---------------------

Graphics demo started

---------------------


[0;32mI (7517) [SPIFFS]: Registering SPIFFS file system[0m
[0;32mI (7518) [SPIFFS]: Mounting SPIFFS files system[0m
[0;32mI (7518) [SPIFFS]: Start address: 0x180000; Size 1024 KB[0m
[0;32mI (7523) [SPIFFS]:   Work buffer: 2048 B[0m
[0;32mI (7528) [SPIFFS]:    FDS buffer: 384 B[0m
[0;32mI (7532) [SPIFFS]:    Cache size: 2048 B[0m
[0;32mI (7574) [SPIFFS]: Mounted[0m


==========================================

Display: ST7735R: PORTRAIT 128,160 Color



     Clear screen time: 14 ms

Send color buffer time: 128 us (128 pixels)

```

---

### Tested on

TTGO TS V1.2 with 1.8in TFT, ST7735 controller, 128x160
![Tested on](https://github.com/nf9/TTGO_TS_V1.2_TFT/blob/master/Documents/s-l640%5B1%5D.jpg)
