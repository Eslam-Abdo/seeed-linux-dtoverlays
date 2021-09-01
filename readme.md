
# MCP2515 for Jetson Nano

Fixes problem on Jetson Nano while attempting to communicate via CAN bus driver (MCP2515). This repo is [fork from seeed](https://github.com/Seeed-Studio/seeed-linux-dtoverlays#readme).

> **NOTE:** It is recommended to reflash your microSD if you did modify the system. Just make sure everything working as intended.

## Pin Configuration

| MCP2515 | Jetson Nano |
| :------ | :---------- |
| VCC     | 5V          |
| GND     | GND         |
| CS      | 24          |
| MISO    | 21          |
| MOSI    | 19          |
| SCK     | 23          |
| INT     | 31          |

## How to Use

1. Open terminal app at your Jetson Nano
2. Let's clone this repo. To do that, run these commands
   ```
   git clone https://github.com/Thor-x86/seeed-linux-dtoverlays
   cd seeed-linux-dtoverlays
   ```
3. Then build and install with these commands
   ```
   make all_jetsonnano
   sudo make install_jetsonnano
   ```
   > **NOTE:** You'll asked for password to modify the system. It is normal when you type your password but nothing happens. Just type it as usual then hit enter.
4. After that, open utility to configure the Jetson
   ```
   sudo /opt/nvidia/jetson-io/jetson-io.py
   ```
5. Hit up or down arrow to navigate. Make sure ` Configure Jetson for compatible hardware` is highlighted then hit enter.
6. Navigate to `MCP251x CAN Controller` and hit enter to select
7. Choose `Save and reboot to reconfigure pins` and hit enter
8. Hit enter again to reboot

## Starting Up CAN Driver

1. Open the terminal again, then enter this command
   ```
   ip link
   ```
2. Make sure this word below shown at terminal
   ```
   can0: ...
   ```
   If not shown, possibly the wiring is not connected properly or too loose. Already correct? Then the fix is not working and you have to reflash the micro SD and start over.
3. Now let's configure the bitrate by entering this command
   ```
   sudo ip link set can0 type can bitrate <your-bitrate>
   ```
   Change `<your-bitrate>` to your intended bitrate **in bits unit**. As example `125000` for 125kbps.
4. To start the CAN driver, enter this command
   ```
   sudo ifconfig can0 up
   ```
5. To stop the CAN driver, enter this command
   ```
   sudo ifconfig can0 down
   ```

## Testing the CAN Driver

- Read data from CAN bus
  ```
  candump can0
  ```
- Send "Hi!" text to CAN bus
  ```
  cansend can0 000#48.69.21.00
  ```
