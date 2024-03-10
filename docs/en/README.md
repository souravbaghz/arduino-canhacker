# CANHacker CAN Adapter on Arduino + MCP2515

## Features

Implement communication with CAN bus via MCP2515 by CANHacker (lawicel) protocol.

- send & receive can frames
- supports standart (11 bit) & extended (29 bit) frames
- supports remote frames (RTR)
- supports filter by ID (mask + code)
- interface using [Stream](https://www.arduino.cc/en/Reference/Stream): ability to work with Serial, SoftwareSerial, Ethernet and other
- supported can baudrates from 10Kbps up to 1Mbps
- supported modules with different oscillators (8, 16, 20 MHZ), 8 MHZ is default, use setClock if your oscillator is not 8MHZ
- support [CanHacker](https://www.mictronics.de/img/2009/12/CANHackerV2.00.01.zip) (application for Windows)
- support [CANreader](https://github.com/autowp/CANreader) (application for Android)

## Pins Connection

|MCP2515 Pin|Arduino Pin|
|-----------|-----------|
|    VCC    |    5V     |
|    GND    |    GND    |
|    CS     |    D10    |
|    SO     |    D12    |
|    SI     |    D11    |
|    SCK    |    D13    |
|    INT    |    D2     |


## Library Installation

1. Install [MCP2515 Library](https://github.com/souravbaghz/arduino-mcp2515)
2. Download the ZIP file from https://github.com/souravbaghz/arduino-canhacker/archive/master.zip
3. From the Arduino IDE: Sketch -> Include Library... -> Add .ZIP Library...
4. You may need to restart the Arduino IDE to see the new "canhacker" library with examples

Testes with Arduino Nano Only.

## Usage

Upload the below sketch to work with CANHacker Windows:

```
#include <can.h>
#include <mcp2515.h>

#include <CanHacker.h>
#include <CanHackerLineReader.h>
#include <lib.h>

#include <SPI.h>
#include <SoftwareSerial.h>

const int SPI_CS_PIN = 10;
const int INT_PIN = 2;

const int SS_RX_PIN = 3;
const int SS_TX_PIN = 4;

CanHackerLineReader *lineReader = NULL;
CanHacker *canHacker = NULL;

SoftwareSerial softwareSerial(SS_RX_PIN, SS_TX_PIN);

void setup() {
    Serial.begin(115200);
    while (!Serial);
    SPI.begin();
    softwareSerial.begin(115200);

    Stream *interfaceStream = &Serial;
    Stream *debugStream = &softwareSerial;
    
    
    canHacker = new CanHacker(interfaceStream, debugStream, SPI_CS_PIN);
    //canHacker->enableLoopback(); // uncomment this for loopback
    lineReader = new CanHackerLineReader(canHacker);
    
    pinMode(INT_PIN, INPUT);
}

void loop() {
    CanHacker::ERROR error;
  
    if (digitalRead(INT_PIN) == LOW) {
        error = canHacker->processInterrupt();
        handleError(error);
    }

    error = lineReader->process();
    handleError(error);
}

void handleError(const CanHacker::ERROR error) {

    switch (error) {
        case CanHacker::ERROR_OK:
        case CanHacker::ERROR_UNKNOWN_COMMAND:
        case CanHacker::ERROR_NOT_CONNECTED:
        case CanHacker::ERROR_MCP2515_ERRIF:
        case CanHacker::ERROR_INVALID_COMMAND:
            return;

        default:
            break;
    }
  
    softwareSerial.print("Failure (code ");
    softwareSerial.print((int)error);
    softwareSerial.println(")");

    digitalWrite(SPI_CS_PIN, HIGH);
    pinMode(LED_BUILTIN, OUTPUT);
  
    while (1) {
        int c = (int)error;
        for (int i=0; i<c; i++) {
            digitalWrite(LED_BUILTIN, HIGH);
            delay(500);
            digitalWrite(LED_BUILTIN, LOW);
            delay(500);
        }
        
        delay(2000);
    } ;
}
```

## Protocol

Protocol CanHacker (lawicel) described in [CanHacker for Windows documentation](http://www.mictronics.de/projects/usb-can-bus/)

Library implements it partially. [Suported commands listed here](protocol.md).

## Contributing

Feel free to open issues, create pull requests and other contributions
