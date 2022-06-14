# ATTINY88_BBSERIAL
ATTINY BIT BANG SERIAL
SIGNAL LAMP DRIVER – ATTINY88 App 1.7

The Signal Lamp has 3 Lamps. To minimize wiring harness a simple ATTINY88 Lamp Slave application will drive each Lamp with 3 (colour) digital outputs. Colour Selected by a single Master Computer – NANO or UNO located externally or in one of the 3 Lamps. The Master communicates with the Lamp Slaves via Half Duplex Bit Bang Serial on Pin 3 – a One Wire Network – referenced to GND

OVERVIEW:
Each Lamp Slave has an Address: 0=Top, 1=Middle, 2=Bottom. Lamp Address is read from Pins 5(==1), 6(==2) where an Input of 1 = '1'. So if Both Address Lines are held Low, this will be the TOP Lamp.
Communications on Pin 3. Master initiates all Communication Transactions on One Wire Network
Pin 4 is driven LOW to connect to Address Pins to affect a '0'
Pin 7 is driven HIGH to connect to Address Pins for '1' – Also Reference voltage for external Comparator
Lamp Colour Outputs are:  8 == RED,  9== GRN,  10 ==YLW driven through external comparator
Flashing Option toggles Lamp at 500 ms rate

COMMUNICATIONS:
Communications Pin (3) is set as INPUT_PULLUP while waiting to Receive, and is only set to OUTPUT to send a message, restoring to INPUT as soon a message has been sent.
Bits are LOW ACTIVE with a 4 ms Bit Time Frame 
Each BYTE Begins with Low Active START_BIT followed by 7 Data Bits (7-Bit Text) and ending with High STOP_BIT 
Next Byte of a Message begins within 10 ms of end of previous byte. End of Message is detected if no new START is sensed within 10 ms.

SIGNAL  LAMP  PROTOCOL:
Signal Lamp Colour Commands begin with '$' followed by 3 colour fields for Top,Middle, Bottom Lamp in comma delimited fields.  
Example    $R,GF,Y           ;This sets TOP=RED, Middle=GRN-FLASHING, and Bottom=YLW
Each Lamp utilizes color selected by its Address (0, 1, 2 = Bottom)
No Reply to '$'  Signal Command
Master can Query Status of any single Lamp (0,1,2) with Msg    @1
Reply to Lamp Query  >#1GF         ;This example shows Lamp 1 is displaying GRN-FLASH

DIRECT IO ACCESS PROTOCOL:
IO Control Commands begin with '!'  3 Command options: GET, SET, CLR for all digital IO
Examples:  !G5      ;Get current value of Digital input 5.       !S5       ;Set Digital Output 5 = 1
                   !C12          ;Clear Digital Output 12 (Set=0)       !G52      ;Get Analog Input[2] value(0-1023)
                   !GET10     ;Get Dig Input Value for [10]            !SET9      ;Set Dig[9] = 1
                   !CLR9       ;Clear Dig[9]  (Set=0)
Accommodation included for Analog Inputs AI0 – AI7 by adding 50 to input number. However, Analog Input values do not appear to work correctly
Reply indicates value for GET, SET, CLR.  Examples Cmd>!G9    Reply>[9]=1
    !SET7      Reply>[7]=1                 !C7      Reply>[7]=0

CONNECTIONS:
VIN is from Power Supply (5.5 VDC),   GND = GND,  Pin(3) is communication Pin – use 220 Ohm at each station on the One Wire Network to limit the drive current on network with multiple nodes.
