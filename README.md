# # Raspberry Pi Setup Guide

This repository provides a comprehensive guide to install and configure a Raspberry Pi, focusing on essential setup steps for beginners and advanced users alike.

## Objectives

- Install the Raspbian operating system on an SD card.
- Configure the network to access the Raspberry Pi.
- Set up the serial port for communication.
- Perform system updates.
- Install Python and verify its functionality.

## Required Materials

- A Raspberry Pi (Model 3 or later).
- An SD card (minimum 8GB) with an adapter for PC.
- A computer with an SD card reader.
- An Ethernet cable (or Wi-Fi connection).
- A micro-USB cable for power.
- A screen and keyboard (optional for headless setup).

## Installation Steps

### 1. Download Raspbian Image

- Visit the official Raspberry Pi website: [https://www.raspberrypi.com/software/](https://www.raspberrypi.com/software/).
- Download the Raspberry Pi Imager tool or directly download the Raspbian image ("Raspberry Pi OS").

### 2. Flash the SD Card

- Install the Raspberry Pi Imager on your computer.
- Launch the tool and select:
  - **OS**: Raspberry Pi OS (32-bit recommended).
  - **Storage**: Your SD card.
- Click "Write" to flash the image onto the SD card.

### 3. Prepare the SD Card for SSH

- Eject and reconnect the SD card.
- Create an empty file named `ssh` (no extension) in the root of the SD card.
- For Wi-Fi, create a `wpa_supplicant.conf` file with the following content:
  ```
  country=FR
  ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
  update_config=1

  network={
      ssid="YourWiFiName"
      psk="YourWiFiPassword"
  }
  ```

### 4. Configure a Static IP Before Boot

- Edit the `cmdline.txt` file in the SD card's root directory.
- Add the following at the end of the existing line (in a single block, no line breaks):
  ```
  ip=192.168.1.100::192.168.1.1:255.255.255.0:rpi:eth0:off
  ```
  Replace `192.168.1.100` with the desired IP address, `192.168.1.1` with the gateway address, and `255.255.255.0` with the appropriate subnet mask.

### 5. Boot the Raspberry Pi

- Insert the SD card into the Raspberry Pi.
- Connect the power supply to boot up the device.

## Network Configuration

### 1. Connect via SSH

- Find the Raspberry Pi's IP address (using your router or a tool like `nmap`).
- Connect via SSH:
  ```
  ssh pi@<ip_address>
  ```
  Default password: `raspberry`.

### 2. Set a Static IP

- Edit the configuration file:
  ```
  sudo nano /etc/dhcpcd.conf
  ```
- Add the following:
  ```
  interface eth0
  static ip_address=192.168.1.100/24
  static routers=192.168.1.1
  static domain_name_servers=192.168.1.1
  ```
- Restart the service:
  ```
  sudo systemctl restart dhcpcd
  ```

## Serial Port Configuration

### 1. Enable the Serial Port

- Launch the configuration tool:
  ```
  sudo raspi-config
  ```
- Navigate to **Interface Options > Serial Port**.
- Enable the serial interface but disable the console on the serial port.

### 2. Test the Serial Port

- Install the `minicom` tool:
  ```
  sudo apt install minicom
  ```
- Test the port:
  ```
  minicom -b 9600 -o -D /dev/serial0
  ```

## System Updates

### 1. Update Packages

- Run the following commands:
  ```
  sudo apt update
  sudo apt upgrade -y
  ```

### 2. Clean Up Unused Files

- Remove unnecessary packages:
  ```
  sudo apt autoremove -y
  ```

## Python Installation

### 1. Check the Preinstalled Version

- Verify Python version:
  ```
  python3 --version
  ```

### 2. Install pip (Python Package Manager)

- Install pip:
  ```
  sudo apt install python3-pip
  ```

### 3. Install Python Libraries (Example)

- Install a library, e.g., `numpy`:
  ```
  pip3 install numpy
  ```

### 4. Test Python

- Launch Python:
  ```
  python3
  ```
- Test a simple script:
  ```python
  print("Hello, Raspberry Pi!")
  ```

## Conclusion

This guide has covered the installation and configuration of a Raspberry Pi, including network setup, serial port activation, system updates, and Python installation. These skills lay the foundation for more advanced Raspberry Pi projects.

