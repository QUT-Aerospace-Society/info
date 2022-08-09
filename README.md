
# QUT Aerospace HPR Avionics Parts Guide
  
## Design rules

### Schematics rules

1.  All signals should be labeled
    
2.  Power should be labeled with recommended VCC value
    
3.  GND should use marker…
    
4.  Resistors…
    
5.  Capacitors…
    
6.  Inductors…
    
7.  Diodes…
    

  

### Coding rules

1.  Code should have comments and clear examples
    
2.  Drivers should define how sensor is setup
    
3.  Driver should note how to set frequency
    
4.  Driver should clearly define how a value is called
    
5.  Drivers should clearly define how any conversion equations are completed
    
6.  …
    

## Parts Guide

### MCU Design

#### RP2040 chip SC0914(13)

ARM® Cortex®-M0+ series Microcontroller IC 32-Bit Dual-Core 133MHz External Program Memory 56-QFN (7x7)

[Datasheet](https://datasheets.raspberrypi.com/rp2040/rp2040-datasheet.pdf)

[Hardware design with RP2040](https://datasheets.raspberrypi.com/rp2040/hardware-design-with-rp2040.pdf)

  

##### Decoupling capacitors

Decoupling capacitors required for RP2040. These provide two basic functions. Firstly, they filter out power supply noise, and secondly, provide a local supply of charge that the circuits inside RP2040 can use at short notice. This prevents the voltage level in the immediate vicinity from dropping too much when the current demand suddenly increases. Because, of this, it is important to place decoupling close to the power pins. Ordinarily, we recommend the use of a 100nF capacitor per power pin.

![](/img/image0.png)

![](/img/image1.png)

##### Internal Voltage Regulator

The internal voltage regulator produces a 1.1V supply from an input of 3.3V. We simply connect the VREG_OUT pin to the DVDD pins. The regulator does have some special requirements when it comes to decoupling capacitors. We must place 1μF capacitors close to both the input (VREG_IN) and the output (VREG_OUT), in order to provide a stable 1.1V supply. The voltage regulator also has restrictions on the amount of ESR (equivalent series resistance) of these capacitors, but in practice, by using physically small ceramic chip capacitors, these requirements will almost certainly be met. In this design, capacitors C8 and C10 (Figure 5) are ceramic capacitors of 0402 size.

![](/img/image2.png)

##### Crystal oscillator

Providing an external frequency source can be done in one of two ways: either by providing a clock source with a CMOS output (3.3V square wave) into the XIN pin, or by using a 12MHz crystal connected between XIN and XOUT. Using a crystal is the preferred option here, as they are both relatively cheap and very accurate.

  

PCB design is critical to an external oscillator, like in other microcontrollers. First and foremost, the load capacitors must be very close to the crystal. The farther the load capacitors, the higher the parasitic capacitance introduced. This added capacitance, of course, changes the overall load capacitance required by the RP2040 for its oscillator.

  

Since this is a signal generating pin, keep the XIN and XOUT pins far away from other (high-speed) signal pins to avoid crosstalk and interference (e.g. opposite side, far away from the QSPI flash chip)

  
  

#### RP2040 Flash W25Q128JVSIM TR

FLASH - NOR Memory IC 128Mb (16M x 8) SPI - Quad I/O, QPI, DTR 133 MHz 8-SOIC

[Datasheet](https://www.winbond.com/resource-files/w25q128jv_dtr%20revc%2003272018%20plus.pdf)

  

![](/img/image3.png)

-   In order to be able to store program code which RP2040 can boot and run from, we need to use a flash memory, specifically, a quad SPI flash memory.
    
-   W25Q128JVS device is a 128Mbit chip (16MB). This is the largest memory size that RP2040 can support.
    
-   As for PCB design, the RP2040 QSPI pins must be wired as close to the external QSPI flash IC as possible to avoid crosstalk. The trace length must not exceed 20 mm with a width of 0.15 mm
    
-   Clock trace should be the longest trace among all the RP2040 to QSPI signals.
    
-   It is always a good idea to add a series termination resistor on the SPI clock line. The value can be 0 ohms, or between 10 ohms to 100 ohms if required.
    
-   Try to have a solid ground plane connecting the ground of the flash chip to the exposed pad of the RP2040.
    

  

### Data Storage / Memory

#### Memory W25N01GVZEIG TR

FLASH - NAND Memory IC 1Gb (128M x 8) SPI 104 MHz 8-WSON (8x6)

[Datasheet](https://www.winbond.com/resource-files/w25n01gv%20revl%20050918%20unsecured.pdf)

-   Use the same guidelines that apply to the QSPI flash chip.
    

  

#### SD Card Reader

-   Use the same guidelines that apply to the QSPI flash chip.
    

  

### Debugging and USB Communication

#### USB C connector 2137160001

USB-C (USB TYPE-C) USB 2.0 Receptacle Connector 24 (16+8 Dummy) Position Through Hole, Right Angle

[Datasheet](https://www.molex.com/webdocs/datasheets/pdf/en-us/2137160001_IO_CONNECTORS.pdf)

  

### Power

#### Buck regulator RT7250AZSP

Buck Switching Regulator IC Positive Adjustable 0.8V 1 Output 2A 8-SOIC (0.154", 3.90mm Width) Exposed Pad

[Datasheet](https://www.richtek.com/assets/product_file/RT7250A=RT7250B/DS7250AB-02.pdf)

  

#### Buck boost regulator MP2155GQ-Z

Buck-Boost Switching Regulator IC Positive Adjustable (Fixed) 1.5V (3.3V) 1 Output 1A, 2.2A 10-VFDFN Exposed Pad

[Datasheet](https://www.monolithicpower.com/en/documentview/productdocument/index/version/2/document_type/Datasheet/lang/en/sku/MP2155GQ/document_id/1760/)

  

#### Fuse MF-MSMF200-2

Polymeric PTC Resettable Fuse 8V 2 A Ih Surface Mount 1812 (4532 Metric), Concave

[Datasheet](https://www.bourns.com/docs/product-datasheets/mf-msmf.pdf)

  

#### Reverse polarity mosfet AON7421

P-Channel 20 V 30A (Ta), 50A (Tc) 6.2W (Ta), 83W (Tc) Surface Mount 8-DFN-EP (3.3x3.3)

[Datasheet](http://www.aosmd.com/res/data_sheets/AON7421.pdf)

  

#### Reverse current diode PMEG045V100EPDAZ

Diode Schottky 45 V 10A Surface Mount CFP15

[Datasheet](https://assets.nexperia.com/documents/data-sheet/PMEG045V100EPD.pdf)

  

### GPS

#### GPS SE868K3A232R001000

Series RF Receiver, Galileo, GLONASS, GNSS, GPS -164dBm 9.6kbps 32-LGA (11x11)

[Datasheet](https://www.telit.com/wp-content/uploads/2022/05/Telit_SE868K3-A_AL_Product_Brief.pdf)

  

### Sensors

#### Accelerometer ADXL375BCCZ

Accelerometer X, Y, Z Axis ±200g 0.05Hz ~ 1.6kHz 14-LGA (3x5)

[Datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/ADXL375.pdf)

  

#### IMU Accelerometer and Gyroscope LSM6DSO32XTR

Accelerometer, Gyroscope, Temperature Sensor I2C, SPI Output

[Datasheet](https://www.st.com/resource/en/datasheet/lsm6dso32x.pdf)

  

#### Barometer MS561101BA03-50

Pressure Sensor 0.15PSI ~ 17.4PSI (1kPa ~ 120kPa) Absolute 24 b 8-SMD

[Datasheet](https://www.te.com/commerce/DocumentDelivery/DDEController?Action=srchrtrv&DocNm=MS5611-01BA03&DocType=Data+Sheet&DocLang=English)

  

### RF Communication

#### Lora SX1262 chip SX1262IMLTRT

IC RF TxRx Only General ISM < 1GHz LoRaWAN 150MHz ~ 960MHz 24-VFQFN Exposed Pad

[Datasheet](https://semtech.my.salesforce.com/sfc/p/E0000000JelG/a/2R000000HT7B/4cQ1B3JG0iKRo9DGRkjVuxclfwB.3tfSUcGr.S_dPd4)

  

#### Lora E22-900M22S module

SX1262 868/915Mhz SPI SMD LoRA Module

[Datasheet](https://www.cdebyte.com/pdf-down.aspx?id=1514)

  

### External Control Systems

#### Pyro charge Solid State Relay TLP3480(TP,E

Solid State SPST-NO (1 Form A) 4-SMD (0.083", 2.10mm)

[Datasheet](https://au.mouser.com/datasheet/2/408/TLP3480_datasheet_en_20200611-1889768.pdf)

  

### Camera

#### Camera Sensor with RP2040

If you are using the parallel port feature of the RP2040 to interface it to a camera, the same general rules for QSPI chip will apply.

  

Make sure to reasonably match the camera interface traces in length and they should be around 6 to 8 mils wide.

  

Trace length should be approximately equal for all of the following lines, with a length mismatch of less than 200 mils for:

  

-   D[0] to D[n] for n data lines of the camera parallel port.
    
-   VSYNC and HSYNC
    
-   PCLK and any other clock lines
    
-   Other lines like camera sensor reset or enable, etc can be of any length.