# EV3-SPIKE_Sensor_Adapter-Staff
This Block allows you to connect LEGO SPIKE Prime/Robot Inventor sensors to LEGO EV3 with adapter board.

## How it works
The EV3 using UART protocol to communicate with sensors, and SPIKE Prime/Robot Inventor(Powered Up devices) using same protocol with some new extensions. So that we can build a simple adapter to connect sensors and EV3.

## Hardware
All EV3/SPIKE/RI Sensors (except EV3 Touch) have small MCUs (STM8) running inside it, EV3 running with 5 V standard, SPIKE/RI running with 3.3 V standard, and MCU in sensors support 2.95 V to 5.5 V, and seems there is no LDO on sensor's PCB, Power is directly connect to MCU's VDD. So that we can build a simple adapter or jump wire to connect sensors and hub.

### SPIKE/RI Sensors Connect to EV3:
Due to EV3 using Auto-ID to identify sensors, Pin 1 need lower than 100 mV, so EV3 side Pin 1 need short to ground (Pin 3) (In EV3 sensors they do the same things), but some SPIKE/RI sensor's component (Color, Ultra Sonic LEDs) using Pin 1 to get more power, so that SPIKE/RI side Pin 1 need short to VCC (Pin 4). Other Pins just connect it from side to side. (Pin6<->Pin6, Pin5<->Pin5...).


### EV3 Sensors Connect to SPIKE/RI Hub:
Because EV3 Sensor can work with 3.3v, so just simply connect Pin 3, 4, 5, 6 (Pin 1 and 2 NC), than SPIKE/RI Hub can read value from EV3!

          EV3 Sensor Pins      <------> SPIKE/RI Hub Pins
    Pin 1 (WHITE, ADC 5V ref.)    NC    Pin 1 M1
    Pin 2 (BLACK, AutoID     )    NC    Pin 2 M2
    Pin 3 (RED, GND          ) <------> Pin 3 GND
    Pin 4 (GREEN, VIN        ) <------> Pin 4 3V3
    Pin 5 (YELLOW, Rx, SCL   ) <------> Pin 5 Tx
    Pin 6 (BLUE, Tx, SDA     ) <------> Pin 6 Rx

## Software

### EV3 with SPIKE Sensors:
The Sensor will send information to the brick, so EV3 will know sensor's id, name, mode, data type...etc., and we make some blocks for EV3-G so that user can easily use those block to reading value from sensors.

> It's not possible control the LEDs on SPIKE/RI Color and Ultra Sonic Sensor in EV3 Environment, Since it use write command with CMD_WRITE_DATA, this function is omitted on EV3. Unless rebuild the EV3 firmware.

### SPIKE/RI Hub with EV3 Sensors:
Using following code to get the info and data from EV3 sensors:
#### Print Sensor Info

	from hub import port 
    
    print(port.A.info())

#### Print Sensor Value
    from hub import port 
    
    Sensor = port.A.device 
    Sensor.mode(0)
    Val = Sensor.get()[0] 
    print(Val)
    
#### EV3 Sensor Mode Info
Here is the mode and info of EV3 Sensors: [EV3 Typedata.rcf](https://github.com/mindboards/ev3sources/blob/78ebaf5b6f8fe31cc17aa5dce0f8e4916a4fc072/lms2012/lms2012/Linux_AM1808/sys/settings/typedata.rcf)

#### SPIKE/RI Sensor Mode Info
Here is the mode and info of SPIKE/RI Sensors: [SPIKE and Boost sensor modes](https://hubmodule.readthedocs.io/en/latest/sensors/#mode)

## Documents used
 - [PyBricks - LEGO Powered Up UART Protocol](https://github.com/pybricks/technical-info/blob/master/uart-protocol.md)
 - [gpdaniels - Experiments SPIKE Prime](https://github.com/gpdaniels/spike-prime)
 - [hub module docs](https://hubmodule.readthedocs.io/en/latest/)
 - [EV3 Hardware Developer Kit](https://education.lego.com/en-us/support/mindstorms-ev3/developer-kits)

## Disclaimer
LEGO® is a trademark of the LEGO Group of companies which does not sponsor, authorize or endorse this software.
The LEGO Group and contributors to this repo are not liable for any loss, injury or damage arising from the use or misuse of the provided code or hardware.
