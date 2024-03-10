<p align="center">
  <img  width="500" src="docs/en/banner.png" />
</p>

<h1 align="center"> <b>CANHacker CAN Adapter</b></h1>
<h2 align="center"><b>Arduino Nano & MCP2515 Board</b></h3> 


## Features

Implement communication with CAN bus via MCP2515 by CANHacker (lawicel) protocol.

- send & receive can frames
- supports standart (11 bit) & extended (29 bit) frames
- supports remote frames (RTR)
- supports filter by ID (mask + code)
- interface using [Stream](https://www.arduino.cc/en/Reference/Stream): ability to work with Serial, SoftwareSerial, Ethernet and other
- supported can baudrates from 10Kbps up to 1Mbps
- supported modules with different oscillators (8, 16, 20 MHz), 8 MHz is default, use setClock if your oscillator is not 8MHz
- support [CanHacker](https://www.mictronics.de/img/2009/12/CANHackerV2.00.01.zip) (application for Windows)
- support [CANreader](https://github.com/autowp/CANreader) (application for Android)

## Documentation

[English](docs/en/)

## Library Installation

1. Install [MCP2515 Library](https://github.com/souravbaghz/arduino-mcp2515)
2. Download the ZIP file from https://github.com/souravbaghz/arduino-canhacker/archive/master.zip
3. From the Arduino IDE: Sketch -> Include Library... -> Add .ZIP Library...
4. Restart the Arduino IDE to see the new "canhacker" library with examples

Testes with Arduino Nano Only.

## Repository History
- Originally from [autowp](https://github.com/autowp/arduino-canhacker.git) Kudos to him :)
- Forked & Modified by [souravbaghz](https://github.com/souravbaghz/arduino-canhacker)
