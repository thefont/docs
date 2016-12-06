# Roku Devices

### Overview
Roku provides the leading operating system for TV. We design the firmware and cloud services to run optimally across a wide range of devices. When developing on Roku OS, it's important for your channel(s) to perform across our wide range of products.

**Sections:**

* [Device Types](#rokus-wide-range-of-devices)
* [Create a device testing bank](#development-tip-create-your-device-testing-bank)
* [Device Chart](#device-chart)
 * [Current Models](#current-models)
 * [Supported Models](#supported-models)
 * [Legacy Models](#legacy-models)

- - - -

## Roku's wide range of devices
The Roku OS runs across a spectrum of hardware. The main form factors are:

* **Roku Players:** The original form factor for Roku devices, these are connected devices that stream video also known as Over-the-top transmission (OTT)
* **Roku Streaming Sticks:** A subset of players, these are smaller HDMI devices that directlt plug into TVs
* **Roku Powered:** Devices licensed to Pay-TV operators. Typically are variants or exact versions of our Roku Players.
* **Roku TVs:** Licensed Roku OS to an array of Television manufacturers using our hardware reference designs

## Development Tip: Create your device testing bank
Developers building channels need to be test across multiple devices prior to submitting for publication in the Roku Channel Store.

We strongly suggest you have a few devices that represent the different "families" of devices - which we refer to as Code names. For example, [**Giga**](#giga) is the family of Roku LT, Roku 2 HD, etc. that have a similar processor, RAM, and other technical specifications.

![Roku devices](https://roku-developer-home-ghost-staging.s3.amazonaws.com/2016/Jul/cm9rdV9kZXZpY2VzLTE0NjkxMjk4NTMzMzA=.png)
<p align=center><i>Caption: The following visual shows a sample of the various Roku devices out in the market.</i></p>

## Device chart
The following tables lists our devices over time - including Roku players, streaming sticks, and TVs. Our Marketing names for hardware are re-used over time (Roku 1, 2, 3 etc.) so you will want to ensure you understand which device you have.

> :information_source: All [Current](#current-models) and [Supported](#supported-models) models support Roku OS 7 or above and will be tested in channel certification.

### Current Models

|  | Roku 1, Roku SE | Roku TV | 4K Roku TV | Roku Streaming Stick | Roku Express | Roku Express Plus | Roku Premiere | Roku Premier Plus | Roku Ultra |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Code Name** | [Tyler](#tyler) | [Liberty](#liberty) | [Ft. Worth](#fort-worth) | [Briscoe](#briscoe) | [Littlefield](#littlefield) | Littlefield | [Cooper](#cooper) | Cooper | Cooper
| **roDeviceInfo.GetModel()** | 2710X | 5000X | 6000X | 3600X | 3700X | 3710X | 4620X | 4630X | 4640X
| **CPU** | MIPS 600 MHz | MIPS 1 GHz | ARM quad core | ARM Cortex A7 quad core 800 MHz | MIPS 900 MHz | MIPS 900 MHz | ARM Cortex A53 quad core 1.2 GHz | ARM Cortex A53 quad core 1.2 GHz | ARM Cortex A53 quad core 1.2 GHz
| **Accelerated Graphics API** <sup>1</sup> | n/a | n/a | OpenGL ES 2.0 | OpenGL ES 2.0 | n/a | n/a | OpenGL ES 2.0 | OpenGL ES 2.0 | OpenGL ES 2.0
| **RAM** | 512 MB | 512 MB | 1.5 GB | 512 MB | 512 MB | 512 MB | 1 GB | 1 GB | 1 GB
| **NVM (Flash) Storage** | 256 MB | 256 MB | 512 MB | 256 MB | 256 MB | 256 MB | 512 MB | 512 MB | 1 GB
| **HDMI Version** | 1.4a | 1.4 | 2.0 | 1.3a | 1.4 | 1.4 | 2.0a | 2.0a | 2.0a
| **HDCP Version** | 1.4 | 1.4 | 2.2 | 1.4 | 1.4 | 1.4 | 2.2 | 2.2 | 2.2
| **Composite Video Out** | yes | n/a | n/a | n/a | n/a | yes | n/a | n/a | n/a
| **HDMI 720p Video Out** | yes | yes <sup>2</sup> | yes <sup>2</sup> | yes | yes | yes | yes | yes | yes
| **HDMI 1080p Video Out** | yes | yes <sup>2</sup> | yes <sup>2</sup> | yes | yes | yes | yes | yes | yes
| **HDMI 2160p Video Out** | n/a | n/a | yes <sup>2</sup>	| n/a | n/a | n/a | yes | yes | yes
| **Max UI Resolution** | 1280x720 | 1280x720 | 1920x1080 | 1280x720 | 1280x720 | 1280x720 | 1920x1080 | 1920x1080 | 1920x1080
| **Max Playback Resolution** | 1920x1080 | 1920x1080 | 3840x2160 | 1920x1080 | 1920x1080 | 1920x1080 | 3840x2160 | 3840x2160 | 3840x2160
| **HDR10** | n/a | n/a | n/a | n/a | n/a | n/a | n/a | yes | yes
| **S/PDIF Port** | n/a | yes | yes | n/a | n/a | n/a | n/a | n/a | yes
| **Wi-Fi** | b/g/n | b/g/n dual-band | b/g/n/ac dual-band | b/g/n dual-band | b/g/n | b/g/n | b/g/n/ac dual-band | b/g/n/ac dual-band | b/g/n/ac dual-band
| **Ethernet Port** | n/a | n/a | 10/100 Mbps | n/a | n/a | n/a | n/a | 10/100 Mbps | 10/100 Mbps
| **USB Port** | n/a | yes | yes | n/a | n/a | n/a | n/a | n/a | yes
| **Max. Channel Size** | 4 MB | 4 MB | 4 MB | 4 MB | 4 MB | 4 MB | 4 MB | 4 MB | 4 MB
| **microSD Slot** | n/a | n/a | n/a | n/a | n/a | n/a | n/a | yes | yes
| **Enhanced Remote** | n/a | n/a | Supported, varies by model | Supported | n/a | n/a | Supported, not included | Supported | Supported
| **Gaming Buttons** | n/a | n/a | Supported, varies by model | n/a | n/a | n/a | Supported, not included | Supported, not included | Supported
| **Motion Control** | n/a | n/a | Supported, varies by model | n/a | n/a | n/a | n/a | n/a | n/a
| **Private Listening** | n/a | n/a | Supported, varies by model | Supported, Roku Mobile App | Supported, Roku Mobile App | Supported, Roku Mobile App | Supported, Roku Mobile App | Supported, via remote and Roku Mobile App | Supported, via remote and Roku Mobile App
| **Voice Remote Search** | n/a | n/a | Supported, varies by model | Supported, not included | n/a | n/a | Supported, not included | Supported, not included | yes

### Supported Models
_These models are no longer manufactured but still run the latest Roku OS and are still supported._

|   | Roku LT | Roku 2 HD | Roku 2 XD | Roku 2 XS | Roku LT | Roku HD | Roku Streaming Stick | Roku 3 | Roku LT | Roku 2 | Roku Streaming Stick | Roku 2 | Roku 3 | Roku 4
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Code Name** | [Giga](#giga) | Giga | Giga | Giga | [Paolo](#paolo) | Paolo | [Jackson](#jackson) | [Austin](#austin) | Tyler | Tyler | [Sugarland](#sugarland) | [Mustang](#mustang) | Mustang | [Dallas](#dallas)
| **roDeviceInfo.GetModel()** | 2400X | 3000X | 3050X | 3100X | 2450X | 2500X | 3400X, 3420X | 4200X | 2700X | 2720X | 3500X | 4210X | 4230X | 4400X
| **CPU** | ARM11 600 MHz | ARM11 600 MHz | ARM11 600 MHz | ARM11 600 MHz | MIPS 400 MHz | MIPS 400 MHz | ARM11 600 MHz | ARM Cortex A9 dual core 1 GHz | MIPS 600 MHz | MIPS 600 MHz | ARM11 600 MHz | ARM Cortex A9 dual core 1 GHz | ARM Cortex A9 dual core 1 GHz | ARM quad core
| **Accelerated Graphics API** <sup>1</sup> | OpenGL ES 2.0 | OpenGL ES 2.0 | OpenGL ES 2.0 | OpenGL ES 2.0 | n/a | n/a | OpenGL ES 2.0 | OpenGL ES 2.0 | n/a | n/a | OpenGL ES 2.0 | OpenGL ES 2.0 | OpenGL ES 2.0 | OpenGL ES 2.0
| **RAM** | 256 MB | 256 MB | 256 MB | 256 MB | 256 MB | 256 MB | 256 MB | 512 MB | 512 MB | 512 MB | 512 MB | 512 MB | 512 MB | 1.5 GB
| **NVM (Flash) Storage** | 256 MB | 256 MB | 256 MB | 256 MB | 256 MB | 256 MB | 256 MB | 256 MB | 256 MB | 256 MB | 256 MB | 256 MB | 256 MB | 512 MB
| **HDMI Version** | 1.3a | 1.3a | 1.3a | 1.3a | 1.3a | 1.3a | 1.3a | 1.4 | 1.4a | 1.4a | 1.3a | 1.4 | 1.4 | 2.0
| **HDCP Version** | 1.4 | 1.4 | 1.4 | 1.4 | 1.4 | 1.4 | 1.4 | 1.4 | 1.4 | 1.4 | 1.4 | 1.4 | 1.4 | 2.2
| **Composite Video Out** | yes | yes | yes | yes | yes | yes | n/a | n/a | yes | yes | n/a | n/a | n/a | n/a
| **HDMI 720p Video Out** | yes | yes | yes | yes | yes | yes | yes | yes | yes | yes | yes | yes | yes | yes
| **HDMI 1080p Video Out** | n/a | n/a | yes | yes | n/a | n/a | yes | yes | n/a | yes | yes | yes | yes | yes
| **HDMI 2160p Video Out** | n/a | n/a | n/a | n/a | n/a | n/a | n/a | n/a | n/a | n/a | n/a | n/a | n/a | yes
| **Max UI Resolution** | 1280x720 | 1280x720 | 1280x720 | 1280x720 | 1280x720 | 1280x720 | 1280x720 | 1280x720 | 1280x720 | 1280x720 | 1280x720 | 1280x720 | 1280x720 | 1920x1080
| **Max Playback Resolution** | 1280x720 | 1280x720 | 1920x1080 | 1920x1080 | 1280x702 | 1280x720 | 1920x1080 | 1920x1080 | 1280x720 | 1920x1080 | 1920x1080 | 1920x1080 | 1920x1080 | 3840x2160
| **S/PDIF Port** | n/a | n/a | n/a | n/a | n/a | n/a | n/a | n/a | n/a | n/a | n/a | n/a | n/a | yes
| **Wi-Fi** | b/g/n | b/g/n | b/g/n | b/g/n | b/g/n | b/g/n | b/g/n | b/g/n dual-band | b/g/n | b/g/n dual-band | b/g/n dual-band | b/g/n dual-band | b/g/n dual-band | b/g/n/ac dual-band
| **Ethernet Port** | n/a | n/a | n/a | 10/100 Mbps | n/a | n/a | n/a | 10/100 Mbps | n/a | n/a | n/a | 10/100 Mbps | 10/100 Mbps | 10/100 Mbps
| **USB Port** | n/a | n/a | n/a | yes | n/a | n/a | n/a | yes | n/a | n/a | n/a | yes | yes | yes
| **Max. Channel Size** | 4 MB | 4 MB | 4 MB | 4 MB | 4 MB | 4 MB | 4 MB | 4 MB | 4 MB | 4 MB | 4 MB | 4 MB | 4 MB | 4 MB
| **microSD Slot** | n/a| yes | yes | yes | n/a | n/a | n/a | n/a | n/a | n/a | n/a | yes | yes | yes
| **Enhanced Remote** | n/a | Bluetooth supported, not included | Bluetooth supported, not included | Bluetooth | n/a | n/a | Bluetooth | Wi-Fi Direct | n/a | Wi-Fi Direct | Bluetooth | Wi-Fi Direct supported, not included | Wi-Fi Direct | Wi-Fi Direct
| **Gaming Buttons** | n/a | Supported, not included | Supported, not included | Supported | n/a | n/a | Supported | Supported | n/a | n/a | Supported | Supported, not included | Supported | Supported
| **Motion Control** | n/a | Supported, not included | Supported, not included | Supported | n/a | n/a | Supported | Supported | n/a | n/a | n/a | Supported, not included | Supported | Supported
| **Private Listening** | n/a | n/a | n/a | n/a | n/a | n/a | n/a | Supported via remote | n/a | Supported via remote | n/a | Supported via remote, not included | Supported via remote | Supported via remote
| **Voice Remote Search** | n/a | n/a | n/a | n/a | n/a | n/a | n/a | Supported, not included | n/a | n/a | n/a | Supported, not included | Supported | Supported

> <sup>1</sup> The OpenGL ES 2.0 API is currently only available under NDA to select partners.
>
> <sup>2</sup> TV display resolution

### Legacy Models
_These models are discontinued and are no longer supported._

|   | Roku DVP | Roku SD | Roku HD | Roku HD-XR | Roku HD | Roku XD | Roku XDS
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Code Name** | [Griffin](#griffin) | [Redwood](#redwood) | Redwood | Redwood | [Pico](#pico) | Pico | Pico
| **roDeviceInfo.GetModel()** | N1000 | N1050 | N1100 | N1101 | 2000C | 2050X, 2050N | 2100X, 2100N
| **CPU** | MIPS 400 MHz | MIPS 400 MHz | MIPS 400 MHz | MIPS 400 MHz | MIPS 400 MHz | MIPS 400 MHz | MIPS 400 MHz
| **Accelerated Graphics API** | n/a | n/a | n/a | n/a | n/a | n/a | n/a
| **RAM** | 256 MB | 256 MB | 256 MB | 256 MB | 256 MB | 256 MB | 256 MB
| **NVM (Flash) Storage** | 64 MB | 64 MB | 64 MB | 256 MB | 64 MB | 64 MB | 256 MB
| **Composite Video Out** | yes | yes | yes | yes | yes | yes | yes
| **S-Video Out** | yes | n/a | yes | yes | n/a | n/a | n/a
| **Component Out** | yes | n/a | yes | yes | n/a | n/a | yes
| **HDMI 720p Video Out** | yes | no | yes | yes | yes | yes | yes
| **HDMI 1080p Video Out** | no	| no | no | yes | no | yes | yes
| **S/PDIF Port** | yes | n/a | yes | yes | n/a | n/a | yes
| **Wi-Fi** | b/g | b/g | b/g | b/g/n dual-band | b/g | b/g/n | b/g/n dual-band
| **Ethernet Port** | yes | yes | yes | yes | yes | yes | yes
| **USB Port** | no | no| no | yes | n/a | n/a | yes
| **Max. Channel Size** | 750 KB | 750 KB | 750 KB | 2 MB | 750 KB | 750 KB | 2 MB
