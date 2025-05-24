# spg120
Python interface for the SPG 120 seriese by Shimadzu. It currently supports only the following products.
- SPG-120UV-REV
- SPG-120S-REV
- SPG-120IR-REV
- AT-120FLC
The developer owns only SPG-120UV-REV and AT-120FLC, which were tested with this python module. The other products (S and IR) should work in principle as only the difference from the controller side is different filter settings.

We use `PySigmaKoki` to controll SHOT702H or SHOT702 via `pyserial`.

## Installation
`pip install spg120`

## Basic usage
```
import spg120
C1, C2 = 2882, 0.008 # calibration parameters
controller = spg120.Controller('/dev/tty.usbserial-A4008T7f', spg120.DeviceType.UV, C1, C2)
for i in range(0, 1200, 100):
    controller.changeWaveLength(i)
Current Nominal and Actual Wavelengths =  0.000, -0.000 (nm)
Current Nominal and Actual Wavelengths =  100.000,  100.018 (nm)
Current Nominal and Actual Wavelengths =  200.000,  200.002 (nm)
Current Nominal and Actual Wavelengths =  300.000,  300.009 (nm)
Current Nominal and Actual Wavelengths =  400.000,  400.011 (nm)
Current Filter = 2
Current Nominal and Actual Wavelengths =  500.000,  499.991 (nm)
Current Nominal and Actual Wavelengths =  600.000,  600.005 (nm)
Current Filter = 3
Current Nominal and Actual Wavelengths =  700.000,  700.011 (nm)
Current Nominal and Actual Wavelengths =  800.000,  800.018 (nm)
Current Nominal and Actual Wavelengths =  900.000,  900.016 (nm)
Current Filter = 1
Current Nominal and Actual Wavelengths =  1000.000,  1000.000 (nm)
Current Nominal and Actual Wavelengths =  1100.000,  1100.003 (nm)
```

If you want to use a custom filter setting, please try the code below.
```
import spg120
controller = spg120.Controller('/dev/tty.usbserial-A4008T7f', spg120.DeviceType.UV, 2882, 0.0008)
Current Nominal and Actual Wavelengths =  0.000, -0.000 (nm)
Current Filter = 1
inf = float('inf')
config = spg120.FilterConfig()
config.addConfig(-inf, 400, 3)
config.addConfig(400, 600, 1)
config.addConfig(600, 900, 2)
config.addConfig(900, +inf, 3)
controller.setFilterConfig(config)
for i in range(0, 1200, 100):
    controller.changeWaveLength(i)
Current Filter = 3
Current Nominal and Actual Wavelengths =  0.000, -0.000 (nm)
Current Nominal and Actual Wavelengths =  100.000,  100.018 (nm)
Current Nominal and Actual Wavelengths =  200.000,  200.002 (nm)
Current Nominal and Actual Wavelengths =  300.000,  300.009 (nm)
Current Nominal and Actual Wavelengths =  400.000,  400.011 (nm)
Current Filter = 1
Current Nominal and Actual Wavelengths =  500.000,  499.991 (nm)
Current Nominal and Actual Wavelengths =  600.000,  600.005 (nm)
Current Filter = 2
Current Nominal and Actual Wavelengths =  700.000,  700.011 (nm)
Current Nominal and Actual Wavelengths =  800.000,  800.018 (nm)
Current Nominal and Actual Wavelengths =  900.000,  900.016 (nm)
Current Filter = 3
Current Nominal and Actual Wavelengths =  1000.000,  1000.000 (nm)
Current Nominal and Actual Wavelengths =  1100.000,  1100.003 (nm)
```