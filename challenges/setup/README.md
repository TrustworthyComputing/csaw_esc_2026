# ESC 2026 — Setup & Flashing Guidance

Welcome to Embedded Security Challenge 2026! Before attempting the challenges, verify your hardware setup, wire your peripherals, and flash the built-in self-test (BIST) firmware to confirm your board is healthy.

> [!IMPORTANT]
> **Applies to all challenges**: Reverse engineering the solution from the binaries is forbidden and solutions based on it won't be accepted.

---

## 1. Hardware Kit & Bill of Materials (BOM)

Refer to the table below to identify each material/component in your hardware kit before beginning the setup:

| Material / Component | Visual Identification & Appearance | Primary Function / Description | Included Quantity & Connection Note |
|---|---|---|---|
| **Ideaspark ESP32 Development Board with 1.14in TFT** | Microcontroller board with built-in color display (ST7789 TFT screen), micro-USB/USB-C port, and 2 rows of pin headers. | Main compute unit running all challenge firmware logic. | 1 board |
| **MFRC522 RFID Reader Module** | Rectangular circuit board (red or blue) with a white printed antenna loop pattern on the PCB. | Scans 13.56 MHz RFID cards and key fobs over SPI interface. | 1 module |
| **RFID Credentials (S50 Card & Key Fob)** | White plastic card and blue tear-drop keychain tag. | Badges scanned by the Mifare RC522 reader during testing & challenges. | 1 card + 1 key fob |
| **AT24C256 I2C EEPROM Breakout Board** | Small green or black PCB breakout board containing an 8-pin EEPROM microchip. | External non-volatile memory storing information over I2C. | 1 module |
| **3.3V Power Splitter Cable** | 1-to-2 Y-splitter female jumper wire cable (1 female socket on one end split into 2 female connectors on the other end). | **Essential Power Splitter**: Connects to the single **3V3** pin on the ESP32 board to supply 3.3V power to **both** the RFID reader (`3.3V`) and EEPROM breakout (`VCC`). | 1 splitter cable |
| **Dupont Jumper Wires** | Flexible multi-colored ribbon cable with individual female-to-female connector pins. | Interconnects GPIO pins between the ESP32 board and external modules. | 1 pack |
| **USB Data Cable** | USB C to USB A Data Cable | Interconnects the ESP32 board to a computer | 1 cable |

---

## 2. Hardware Wiring Reference

Ensure your ESP32 board is **unplugged from the USB port** before making any wiring connections. Refer to the tables below to connect the external components to the Ideaspark ESP32 board.

> [!IMPORTANT]
> **Power Rail Note**: The Ideaspark ESP32 board features only **one** 3.3V (`3V3`) power output pin. You **must** use the **3.3V Power Splitter Cable** (listed in the BOM above) to supply 3.3V power to both the MFRC522 RFID module (`3.3V`) and the 24Cxx EEPROM breakout (`VCC`). Connecting 5V directly to either module will damage the components!
>
> **Pin Label Note**: Pin **D17** on the ESP32 is labeled as **TX2** on some board silk-screens. **D17** and **TX2** refer to the exact same physical pin header (GPIO17).

### MFRC522 RFID Module (SPI / VSPI Bus)
The RFID reader shares the SPI bus with the on-board display and is selected via the SS (SDA) pin.

| RC522 Label | ESP32 Pin | Description | Wiring & Cable Note |
|---|---|---|---|
| **SDA** (SS) | **D5** | Chip Select (Active Low) | Standard Jumper Wire |
| **SCK** | **D18** | SPI Clock | Standard Jumper Wire |
| **MOSI** | **D23** | Master Out Slave In | Standard Jumper Wire |
| **MISO** | **D19** | Master In Slave Out | Standard Jumper Wire |
| **RST** | **D17** *(or **TX2**)* | Hardware Reset | Standard Jumper Wire *(Pin GPIO17 is labeled **D17** or **TX2** on board)* |
| **3.3V** | **3V3 Rail** | **Must be 3.3V — NOT 5V!** | 🔌 **Requires 3.3V Splitter Cable** *(connects to 3V3 pin on ESP32)* |
| **GND** | **GND** | Common Ground | Standard Jumper Wire *(or Splitter Cable if sharing GND pin)* |

### 24Cxx I2C EEPROM Breakout
The EEPROM acts as the alarm status store and badge credential copy.

| EEPROM Label | ESP32 Pin | Description | Wiring & Cable Note |
|---|---|---|---|
| **SDA** | **D21** | I2C Serial Data | Standard Jumper Wire |
| **SCL** | **D22** | I2C Serial Clock | Standard Jumper Wire |
| **VCC** | **3V3 Rail** | Power (3.3V) | 🔌 **Requires 3.3V Splitter Cable** *(connects to 3V3 pin on ESP32)* |
| **GND** | **GND** | Common Ground | Standard Jumper Wire *(or Splitter Cable if sharing GND pin)* |

> [!NOTE]
> The A0/A1/A2 and WP jumpers on the EEPROM breakout are pre-fitted and grounded (writes enabled, I2C address `0x50`). Do not change these jumpers.

---

## 3. Built-in Self-Test (BIST) & Precompiled Firmware

Each challenge (and the initial self-test) ships as a single precompiled binary (`merged.bin`).
- **Self-Test Binary**: Located at `challenges/setup/firmware/bist_merged.bin`.
- Flashing this firmware tests the ESP32 SoC, ST7789 TFT display, MFRC522 RFID reader, and 24Cxx I2C EEPROM breakout.

---

## 4. Flashing Guidance for Competitors

You can flash precompiled `.bin` files to the ESP32 using either a web browser (no installation required) or the standard command-line `esptool`.

### Option A: Web Browser Flasher (Recommended / Zero Installation)

Competitors can flash `.bin` files directly from Google Chrome, Microsoft Edge, or Brave using the official **Espressif Web Flasher**:

🔗 **Web Tool**: [https://espressif.github.io/esptool-js/](https://espressif.github.io/esptool-js/)

#### Before Clicking Connect:
1. Connect your ESP32 board to your computer using a USB cable.
2. Open [https://espressif.github.io/esptool-js/](https://espressif.github.io/esptool-js/) in a supported browser (Chrome, Edge, Brave).
3. Under the **Program** section on the page, set the **Baudrate** dropdown menu to **`460800`** (recommended for fast flashing) or `115200`.

#### Flashing Steps:
4. Click the **Connect** button under the **Program** section.
5. A browser pop-up prompt will appear. Select your board's serial port:
   - **Windows**: Select `COM3`, `COM4`, etc.
   - **Linux / macOS**: Select `/dev/ttyUSB0`, `/dev/ttyACM0`, or `/dev/cu.usbserial-*`.
6. Scroll down to the **Flash Files** section:
   - In the address box, enter offset **`0x0`** (or `0x0000`).
   - Click **Choose File** and select your `.bin` file (e.g., `bist_merged.bin` or `<challenge>/merged.bin`).
7. Click the **Program** button to begin flashing.
8. Once the progress bar reaches 100% complete, press the physical **EN (Reset)** button on your ESP32 board to boot into the newly flashed firmware.

---

### Option B: Command-Line (`esptool`)

If you prefer using the command line:

```bash
# Linux / macOS
esptool --chip esp32 --port /dev/ttyUSB0 --baud 460800 write-flash 0x0 <file>.bin

# Windows (PowerShell / Command Prompt)
esptool --chip esp32 --port COM3 --baud 460800 write-flash 0x0 <file>.bin
```

---

## 5. Serial / UART Communication Guidance (After Flashing)

After successfully flashing firmware and pressing the **EN (Reset)** button, challenge outputs, prompts, and flags are transmitted over the board's serial (UART) interface. Several challenges require sending interactive commands back.

> [!NOTE]
> **Important**: The terminal software listed below are **just suggestions**. Competitors are free to use any serial terminal application of their choice, provided it supports 115200 baud, 8N1 serial communication.

### Required UART Connection Settings:
- **Baud Rate**: **`115200`**
- **Data Bits**: **`8`**
- **Parity**: **`None`** (`N`)
- **Stop Bits**: **`1`**
- **Flow Control**: **`None`**
- **Line Ending**: **`Both CR & LF`** (`\r\n`) or `LF` (`\n`)

---

### Suggested Serial Terminal Applications:

#### 1. Desktop GUI Terminal Apps
- **MobaXterm** (Windows): Click **Session** -> **Serial**, select your Serial Port (`COM3`, `COM4`, etc.), set Speed to **`115200`**, and click OK.
- **PuTTY** (Windows / Linux): Select Connection type **Serial**, enter your Serial line (e.g. `COM3` or `/dev/ttyUSB0`), and set Speed to **`115200`**.
- **Arduino IDE Serial Monitor** (Cross-Platform): Open Arduino IDE -> `Tools` -> `Serial Monitor` (or `Ctrl+Shift+M`), select Port, set baud rate dropdown to **`115200 baud`**, and line ending to **`Both NL & CR`**.
- **CoolTerm** / **SerialTool** (Windows / macOS / Linux): Dedicated serial GUI clients. Open `Options` -> select Port, Baud **`115200`**, Data Bits `8`, Parity `None`, Stop Bits `1`.

#### 2. Command-Line (CLI) Terminal Tools
- **`arduino-cli`**:
  ```bash
  arduino-cli monitor -p /dev/ttyUSB0 -c baudrate=115200
  ```
- **`picocom`**:
  ```bash
  picocom -b 115200 /dev/ttyUSB0
  ```
- **`minicom`**:
  ```bash
  minicom -D /dev/ttyUSB0 -b 115200
  ```
- **`screen`**:
  ```bash
  screen /dev/ttyUSB0 115200
  # (To exit screen, press Ctrl+A followed by k)
  ```
---

## 6. Bluetooth Low Energy (BLE) Guidance (Set 3 Challenges)

The challenges in **Set 3** introduce Bluetooth Low Energy (BLE) GATT interaction on the ESP32. Depending on the challenge, a BLE connection may be required to scan for advertised services, inspect GATT characteristics, or unlock gated serial interfaces.

> [!NOTE]
> Competitors are free to use any BLE scanner, GATT client app, or custom script of their choice.

### Suggested BLE Tools & Applications:

#### 1. Mobile Apps (Recommended for quick GUI exploration)
- **nRF Connect for Mobile** by Nordic Semiconductor ([iOS App Store](https://apps.apple.com/app/nrf-connect-for-mobile/id1054362403) / [Google Play Store](https://play.google.com/store/apps/details?id=no.nordicsemi.android.mcp)): The premier tool for inspecting BLE advertisements, connecting to GATT servers, and reading/writing GATT characteristics.

#### 2. Python Scripting (Recommended for automated solving)
- **`bleak`** (Cross-Platform Python BLE Library):
  ```bash
  pip install bleak
  ```
  `bleak` works seamlessly on Linux, macOS, and Windows for discovering devices, reading GATT characteristics, and writing payloads in Python scripts.

#### 3. Desktop & Command-Line (CLI) BLE Tools
- **nRF Connect for Desktop** (Windows / macOS / Linux): Nordic's desktop suite with a dedicated Bluetooth Low Energy app for USB Bluetooth adapters.
- **`bluetoothctl`** (Linux CLI): Native BlueZ utility on Linux:
  ```bash
  bluetoothctl
  # Inside bluetoothctl:
  scan on
  connect <DEVICE_MAC_OR_UUID>
  menu gatt
  list-attributes
  ```

### OS & Bluetooth Permission Notes:
- **Location & Bluetooth Permissions**: Mobile operating systems (Android/iOS) require **Location Services** and **Bluetooth** permissions enabled to scan for BLE advertisements.
- **Linux BlueZ Daemon**: Ensure the `bluetooth` service is active (`sudo systemctl status bluetooth`). If using Python (`bleak`), your user account should have access to D-Bus / Bluetooth.

---


## 7. Troubleshooting & Reference Notes (Optional)

> [!NOTE]
> The following notes are optional references for common platform-specific issues that may arise depending on your OS and serial terminal choice. These are not mandatory steps for every setup.

### OS-Specific Serial & Driver References
- **Linux**: Ensure your user account is added to the `dialout` or `uucp` group (`sudo usermod -aG dialout $USER`) for serial port access permissions. If `/dev/ttyUSB0` repeatedly disconnects upon connection, check if the `brltty` daemon is seizing the port and disable it (`sudo systemctl stop brltty-udev.service`).
  - *Reference*: [ArchWiki - Working with TTY Devices](https://wiki.archlinux.org/title/Working_with_the_serial_console#Accessing_tty_devices)
- **macOS**: Select the Call-Up device node (`/dev/cu.usbserial-*`) instead of `/dev/tty.usbserial-*` when launching serial monitors, as `/dev/tty.*` nodes block waiting for DCD (Data Carrier Detect) hardware line signals.
  - *Reference*: [Stack Overflow - Difference between /dev/tty.* and /dev/cu.* on macOS](https://stackoverflow.com/questions/8632586/what-is-the-difference-between-dev-tty-and-dev-cu-on-macos)

- **Windows**: Ensure USB-to-UART bridge drivers (such as [Silicon Labs CP210x VCP Drivers](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers) or [WCH CH340/CH341 Drivers](http://www.wch-ic.com/downloads/CH341SER_EXE.html)) are installed in Device Manager if the COM port does not enumerate automatically.

### Line Ending Configuration
- Interactive UART challenge prompts expect a standard newline terminator (`CRLF` / `\r\n` or `LF` / `\n`). Some command-line terminal tools (such as default `screen`) transmit `CR` (`\r`) alone on Enter, causing firmware string parsers (`readStringUntil('\n')`) to hang indefinitely. If your commands get no response, ensure your terminal is set to send `\r\n` (e.g., in `picocom`, use `--omap crcrlf`).

### Wi-Fi & Network Reference Notes
- **"No Internet" Auto-Disconnect**: Operating systems (Windows/macOS/mobile) may automatically disconnect from `SmartHomeGW` because the gateway has no WAN internet access. Uncheck *"Connect Automatically"* on venue/home Wi-Fi networks to prevent auto-switching.
- **Subnet Collisions (`192.168.4.1`)**: Active VPN tunnels or Docker bridge subnets using `192.168.4.x` can hijack HTTP requests meant for `http://192.168.4.1/`. Temporarily pause active VPNs or add a host route (`sudo ip route add 192.168.4.1 dev wlan0`).
- **Stale Browser Cache**: If your device is connected to the `SmartHomeGW` access point but the web UI still does not load, clear the browser's cache (or open `http://192.168.4.1/` in a private/incognito window) and reload.
- **Weak AP Signal**: If `SmartHomeGW` shows low signal strength or the connection keeps dropping, raise the ESP32 higher off the table and move it away from large metal surfaces, laptop lids, and other sources of 2.4 GHz interference.
- **Windows 11 Network Discovery**: Windows 11 intermittently fails to list `SmartHomeGW` even while the access point is broadcasting normally. To determine whether the gateway is at fault, check the Wi-Fi list on a phone or a second device; if the network appears there, the access point is healthy and the issue is local to Windows. To recover, disable Wi-Fi on the computer, close the network flyout from the taskbar (the pop-up panel that opens when you click the Wi-Fi icon in the system tray), then reopen the flyout and re-enable Wi-Fi. The network should enumerate on the next scan.


