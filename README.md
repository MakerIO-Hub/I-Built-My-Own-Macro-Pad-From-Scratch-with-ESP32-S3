# **MACROPAD V1 - COMPLETE SETUP AND WIRING GUIDE**

This guide walks you through all phases of the project in sequential order, from the hardware component list and wiring schematics to the required computer drivers and library installations.

## **SECTION 1: Hardware Components Used**

The following hardware components are integrated to ensure the stable and reliable operation of the system:

- **ESP32 Development Board (USB-OTG Supported):** The brain of the system. It connects to the internet via Wi-Fi and emulates a native USB hardware keyboard when connected to a computer.
- **ILI9341 TFT LCD Color Display (320x240 Pixels, SPI):** The primary screen displaying time, weather, and volume levels.
- **KY-040 Rotary Encoder:** A rotary button used for volume adjustments that also functions as a Play/Pause control when pressed.
- **8 x Mechanical Keyboard Switches (Buttons):** Physical buttons used to trigger macro shortcuts.

## **SECTION 2: Hardware Wiring and Pinout Configuration**

Every component must be connected to the correct GPIO pins on the ESP32 board. Wire the system according to the tables below:

###

### **1\. Display (ILI9341 TFT) Connections:**

| **Display Pin** | **ESP32 GPIO Pin**     | **Description**          |
| --------------- | ---------------------- | ------------------------ |
| **VCC**         | 3.3V                   | Power Input              |
| ---             | ---                    | ---                      |
| **GND**         | GND                    | Ground                   |
| ---             | ---                    | ---                      |
| **CS**          | GPIO 10                | Chip Select              |
| ---             | ---                    | ---                      |
| **RST / RESET** | GPIO 9                 | Reset                    |
| ---             | ---                    | ---                      |
| **DC / RS**     | GPIO 11                | Data / Command Selection |
| ---             | ---                    | ---                      |
| **MOSI / SDI**  | GPIO 13                | SPI Data Input           |
| ---             | ---                    | ---                      |
| **SCK / SCLK**  | GPIO 12                | SPI Clock                |
| ---             | ---                    | ---                      |
| **LED**         | 3.3V (or via Resistor) | Display Backlight Power  |
| ---             | ---                    | ---                      |

###

### **2\. Rotary Encoder Connections:**

| **Encoder Pin** | **ESP32 GPIO Pin** | **Description**               |
| --------------- | ------------------ | ----------------------------- |
| **CLK**         | GPIO 4             | Rotational Direction Tracking |
| ---             | ---                | ---                           |
| **DT**          | GPIO 5             | Rotational Direction Tracking |
| ---             | ---                | ---                           |
| **SW**          | GPIO 6             | Button Click (Play/Pause)     |
| ---             | ---                | ---                           |
| **\+ / VCC**    | 3.3V               | Power Input                   |
| ---             | ---                | ---                           |
| **GND**         | Ground             |                               |
| ---             | ---                | ---                           |

###

### **3\. Macro Buttons (Shortcut Keys) Connections:**

One terminal of each button must be connected to its designated GPIO pin, while the other terminal connects to a common GND (Ground) line _(internal Pull-Up resistors are enabled via software)._

- **Button 1:** GPIO 1
- **Button 2:** GPIO 2
- **Button 3:** GPIO 3
- **Button 4:** GPIO 7
- **Button 5:** GPIO 8
- **Button 6:** GPIO 14
- **Button 7:** GPIO 15
- **Button 8:** GPIO 16

## **SECTION 3: Required Drivers and Software**

To program the board and ensure your computer correctly recognizes the Macropad, you must install the following software and drivers:

### **1\. Arduino IDE (Coding Environment)**

- **What is it?:** The main software used to compile and upload your source code to the ESP32 board.
- **Where to download?:** Download and install the latest version for your operating system from arduino.cc/en/software.

### **2\. USB-to-UART Bridge Drivers (Mandatory Installation)**

When you connect the ESP32 board to your PC via a USB cable, these drivers allow the computer to detect the board and display a "COM Port" option in the Arduino IDE. Identify the small square interface chip on your ESP32 board and install the corresponding driver:

- **CP210x Driver (Most Common):** Go to silabs.com, navigate to the "Downloads" tab, and install the version matching your OS.
- **CH340 / CH341 Driver:** If your board uses the CH340 chip, search for the official driver online and install it.

## **SECTION 4: Arduino IDE Configuration and Library Installation**

Once the IDE is installed, additional core configurations and libraries must be loaded to support the hardware:

### **1\. Installing the ESP32 Board Core Package:**

- Launch **Arduino IDE**. Go to **File > Preferences**.
- Locate the **Additional Boards Manager URLs** field and paste the following link:  
   <https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json>
- Click **OK** to save and exit.
- Click on the **Boards Manager** icon located on the left vertical toolbar.
- Type esp32 into the search field.
- Find the package by **Espressif Systems** and click **Install**.

### **2\. Installing the LovyanGFX Graphics Library (For Display Operation):**

- Click on the **Library Manager** icon on the left vertical toolbar.
- Type LovyanGFX into the search field.
- Locate the official **LovyanGFX** library and click **Install**.

## **SECTION 5: Firmware Preparation and Uploading Procedures**

Before initiating the flashing process, the development environment must be fully configured to recognize your hardware, and your personal parameters must be declared within the source file.

Execute the following steps sequentially without skipping:

### **STEP 1: Configuring the Boards Manager URL**

To enable the Arduino IDE to download the core software development packages for the ESP32:

- Open the **Arduino IDE**.
- Navigate to **File > Preferences**.
- In the preferences window, locate the **Additional Boards Manager URLs** field.
- Copy and paste the following URL directly into the field _(if other URLs exist, separate them using a comma)_:  
   <https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json>
- Click **OK** to commit changes and save.

### **STEP 2: Downloading Hardware Board and Library Packages**

- **A) Installing Core Board Files:**
  - Click the **Boards Manager** icon on the left vertical navigation rail _(or navigate via Tools > Board > Boards Manager)_.
  - Search for esp32 in the search box.
  - Find the package maintained by **Espressif Systems** and hit **Install** _(this download may take a minute or two depending on network speeds)_.
- **B) Installing Graphical Library Drivers:**
  - Click the **Library Manager** icon on the left vertical navigation rail _(or go to Sketch > Include Library > Manage Libraries)_.
  - Search for LovyanGFX.
  - Locate the library published by **lovyand03** and select **Install**.

### **STEP 3: Modifying User Configuration Parameters within the Code**

Locate the global variable definitions at the absolute top of the source code file and update the following strings with your network parameters:

- const char\* ssid = "Your*WiFi_Name"; \$\\rightarrow\$ The precise name (SSID) of your home Wi-Fi network *(case-sensitive)\_.
- const char\* password = "Your_WiFi_Password"; \$\\rightarrow\$ The password of your wireless network.
- const char\* apiKey = "cff69c0dd777fa73518805a72bd697ce"; \$\\rightarrow\$ Your dedicated OpenWeatherMap API authentication token utilized to pull down live weather metrics for İzmir _(pre-configured and ready for use)_.

### **STEP 4: Configuring the Arduino IDE "Tools" Menu**

Connect your device to your computer using a data-backed USB cable. Access the **Tools** dropdown menu at the top of your interface and match all settings precisely to the following specification array:

- **Board:** "ESP32S3 Dev Module"
- **Port:** "COM9" _(Note: The virtual communications port designation will vary depending on your physical system port mapping; identify and select your active COM designation, e.g., COM3, COM4, COM5)_
- **USB CDC On Boot:** "Enabled" _(Crucial requirement: Enables the core microcontroller hardware to natively register as a Human Interface Device peripheral)_
- **CPU Frequency:** "240MHz (WiFi)"
- **Core Debug Level:** "None"
- **USB DFU On Boot:** "Disabled"
- **Erase All Flash Before Sketch Upload:** "Disabled"
- **Events Run On:** "Core 1"
- **Flash Mode:** "QIO 80MHz"
- **Flash Size:** "16MB (128Mb)"
- **JTAG Adapter:** "Disabled"
- **Arduino Runs On:** "Core 1"
- **USB Firmware MSC On Boot:** "Enabled (Requires USB-OTG Mode)"
- **Partition Scheme:** "Default 4MB with spiffs (1.2MB APP/1.5MB SPIFFS)"
- **PSRAM:** "OPI PSRAM"
- **Upload Mode:** "UART0 / Hardware CDC"
- **Upload Speed:** "921600"
- **USB Mode:** "USB-OTG (TinyUSB)" _(Mandatory condition: Required by the embedded software stack to safely instantiate keyboard emulations)_
- **Zigbee Mode:** "Disabled"

### **STEP 5: Executing the Firmware Upload**

- Once all parameters match the specifications, click the **Upload** button _(the right-facing horizontal arrow icon)_ at the top left of the IDE.
- The compiler will parse and verify the structure of the codebase (**Compilation** phase).
- Upon compilation success, the flash utility terminal window will show real-time memory address allocations tracking up to completion (Writing at 0x00010000... %100).
- When the flashing routine finishes, the ESP32 chip will execute a reset, establish a handshake with your configured wireless local network, sync up local time, and render your center-aligned, modern UI dashboard displaying time, calendar telemetry, and live weather conditions for İzmir. Your device is now active and operational!

## **SECTION 6: Operation and User Instructions**

- **Time and Date:** The unit establishes real-time connections with Network Time Protocol (NTP) servers via Wi-Fi, updating the clock on the UI precisely every second.
- **Live Weather Updates:** Environmental telemetry for İzmir updates silently in the background every 15 minutes, displaying conditions in English on the primary graphical screen.
- **Volume Control:** Rotating the encoder clockwise increments the host machine's system volume by +2%; rotating it counter-clockwise decrements it by -2%. Modifying volume metrics immediately triggers an animated progress dashboard overlay titled **"VOLUME LEVEL"** on the bottom quadrant of the screen. This system overlay remains active for exactly 3 seconds post-adjustment before fading away to bring back the default operational layout.
- **Play / Pause Media Control:** Pressing the rotary encoder shaft inward transmits standard consumer control macro keycodes to play or pause media applications natively across the OS framework (e.g., YouTube, Spotify).
- **Macro Function Triggers:** Actuating any of the 8 custom tactile mechanical key switches instantly dispatches complex application shortcuts (e.g., triggering Google Chrome, launching Google Drive) or native OS system macros (e.g., Copy, Paste). The active operation is logged dynamically inside the contextual display bar at the bottom of the interface.
