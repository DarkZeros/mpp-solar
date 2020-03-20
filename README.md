# MPP Solar Inverter Python Package

[![Build Status](https://travis-ci.org/jblance/mpp-solar.svg?branch=master)](https://travis-ci.org/jblance/mpp-solar)
[![Coverage Status](https://coveralls.io/repos/github/jblance/mpp-solar/badge.svg?branch=master)](https://coveralls.io/github/jblance/mpp-solar?branch=master)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/08c51e13554d4f77836c6cc7b010df2c)](https://www.codacy.com/app/jblance/mpp-solar?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=jblance/mpp-solar&amp;utm_campaign=Badge_Grade)

Python package with reference library of serial commands (and responses)
for PIP-4048MS inverters - aka:
- PIP-4048MS
- IPS-4000WM
- Voltronic Axpert MKS 5KVA Off-grid Inverter-Chargers

Good overview and technical discussion here
http://forums.aeva.asn.au/viewtopic.php?f=31&t=4332

## Tested On
- a Raspberry Pi 3
- using 2x USB to serial adapters (like https://www.adafruit.com/product/18)
- to connect to 2x PIP-4048MS inverters connected in parallel

Also
- a Raspberry Pi 3
- USB cable to USB port of PIP-4048MS (connected to Pi as /dev/hidraw0)

## Install
`sudo python ./setup.py install`

## Usage
- The commands default to using `/dev/ttyUSB0` if you are using direct USB connection try adding `-d /dev/hidraw0` to the commands
- if you have other USB devices connected the inverter might show up as `/dev/hidraw1` or `/dev/hidraw2`
- if uncertain, remove and re-connect the connection to the inverter and look at the end of the `dmesg` response to see what was reconnected
- also in some instances only root has access to the device that the inverter is connected to - if you are getting no response try using `sudo`
- if you are getting no/unexpected results add `-D` to the command to get a heap of extra information

`$ mpp-solar -h`
```
usage: mpp-solar [-h] [-c COMMAND] [-D] [-d DEVICE] [-b BAUD] [-l] [-s] [-t]
                 [-R]

MPP Solar Command Utility

optional arguments:
  -h, --help            show this help message and exit
  -c COMMAND, --command COMMAND
                        Command to run
  -D, --enableDebug     Enable Debug
  -d DEVICE, --device DEVICE
                        Serial device to communicate with
  -b BAUD, --baud BAUD  Baud rate for serial communications
  -l, --listknown       List known commands
  -s, --getStatus       Get Inverter Status
  -t, --getSettings     Get Inverter Settings
  -R, --showraw         Display the raw results

```

## Available Commands
`$ mpp-solar -l` or `$ mpp-solar -l -d /dev/hidraw0`
```
F
Set Device Output Frequency
 -- examples: F50 (set output frequency to 50Hz) or F60 (set output frequency to 60Hz)

MCHGC
Set Max Charging Current (for parallel units)
 -- examples: MCHGC040 (set unit 0 to max charging current of 40A), MCHGC160 (set unit 1 to max charging current of 60A)

MNCHGC
Set Utility Max Charging Current (more than 100A) (for 4000/5000)
 -- example: MNCHGC1120 (set unit 1 utility max charging current to 120A)

MUCHGC
Set Utility Max Charging Current
 -- example: MUCHGC130 (set unit 1 utility max charging current to 30A)

PBCV
Set Battery re-charge voltage
 -- example PBCV44.0 - set re-charge voltage to 44V (12V unit: 11V/11.3V/11.5V/11.8V/12V/12.3V/12.5V/12.8V, 24V unit: 22V/22.5V/23V/23.5V/24V/24.5V/25V/25.5V, 48V unit: 44V/45V/46V/47V/48V/49V/50V/51V)

PBDV
Set Battery re-discharge voltage
 -- example PBDV48.0 - set re-discharge voltage to 48V (12V unit: 00.0V/12V/12.3V/12.5V/12.8V/13V/13.3V/13.5V/13.8V/14V/14.3V/14.5, 24V unit: 00.0V/24V/24.5V/25V/25.5V/26V/26.5V/27V/27.5V/28V/28.5V/29V, 48V unit: 00.0V/48V/49V/50V/51V/52V/53V/54V/55V/56V/57V/58V, 00.0V means battery is full(charging in float mode).)

PBFT
Set Battery Float Charging Voltage
 -- example PBFT58.0 - set battery float charging voltage to 58V (48.0 - 58.4V for 48V unit)

PBT
Set Battery Type
 -- examples: PBT00 (set battery as AGM), PBT01 (set battery as FLOODED), PBT02 (set battery as USER)

PCP
Set Device Charger Priority
 -- examples: PCP00 (set utility first), PCP01 (set solar first), PCP02 (HS only: set solar and utility), PCP03 (set solar only charging)

PCVV
Set Battery C.V. (constant voltage) charging voltage
 -- example PCVV48.0 - set charging voltage to 48V (48.0 - 58.4V for 48V unit)

PEPD
Set the enabled / disabled state of various Inverter settings (e.g. buzzer, overload, interrupt alarm)
 -- examples: PEABJ/PDKUVXYZ (enable A buzzer, B overload bypass, J power saving / disable K LCD go to default after 1min, U overload restart, V overtemp restart, X backlight, Y alarm on primary source interrupt, Z fault code record)

PF
Set Control Parameters to Default Values
 -- example PF (reset control parameters to defaults)

PGR
Set Grid Working Range
 -- examples: PCR00 (set device working range to appliance), PCR01 (set device working range to UPS)

POP
Set Device Output Source Priority
 -- examples: POP00 (set utility first), POP01 (set solar first), POP02 (set SBU priority)

POPM
Set Device Output Mode (for 4000/5000)
 -- examples: POPM01 (set unit 0 to 1 - parallel output), POPM10 (set unit 1 to 0 - single machine output), POPM02 (set unit 0 to 2 - phase 1 of 3), POPM13 (set unit 1 to 3 - phase 2 of 3), POPM24 (set unit 2 to 4 - phase 3 of 3)

PPCP
Set Parallel Device Charger Priority (for 4000/5000)
 -- examples: PPCP000 (set unit 1 to 00 - utility first), PPCP101 (set unit 1 to 01 - solar first), PPCP202 (set unit 2 to 02 - solar and utility), PPCP003 (set unit 0 to 03 - solar only charging)

PPVOKC
Set PV OK Condition
 -- examples: PPVOKC0 (as long as one unit has connected PV, parallel system will consider PV OK), PPVOKC1 (only if all inverters have connected PV, parallel system will consider PV OK)

PSDV
Set Battery Cut-off Voltage
 -- example PSDV40.0 - set battery cut-off voltage to 40V (40.0 - 48.0V for 48V unit)

PSPB
Set Solar Power Balance
 -- examples: PSPB0 (PV input max current will be the max charged current), PSPB1 (PV input max power will be the sum of the max charge power and loads power)

Q1
Q1 query


QBOOT
DSP Has Bootstrap inquiry


QDI
Default Settings inquiry
 -- queries the default settings from the Inverter

QFLAG
Flag Status inquiry
 -- queries the enabled / disabled state of various Inverter settings (e.g. buzzer, overload, interrupt alarm)

QID
Device Serial Number inquiry
 -- queries the device serial number

QMCHGCR
Max Charging Current Options inquiry
 -- queries the maximum charging current setting of the Inverter

QMOD
Mode inquiry
 -- queries the Inverter mode

QMUCHGCR
Max Utility Charging Current Options inquiry
 -- queries the maximum utility charging current setting of the Inverter

QOPM
Output Mode inquiry
 -- queries the output mode of the Inverter (e.g. single, parallel, phase 1 of 3 etc)

QPGSn
Parallel Information inquiry
 -- example: QPGS1 queries the values of various metrics from instance 1 of parallel setup Inverters (numbers from 0)

QPI
Protocol ID inquiry
 -- queries the device protocol ID. e.g. 30 for HS series

QPIGS
General Status Parameters inquiry
 -- queries the value of various metrics from the Inverter

QPIRI
Current Settings inquiry
 -- queries the current settings from the Inverter

QPIWS
Warning status inquiry
 -- queries any active warnings flags from the Inverter

QVFW
Main CPU firmware version inquiry
 -- queries the main CPU firmware version

QVFW2
Secondary CPU firmware version inquiry
 -- queries the secondary CPU firmware version
```

## Example
`$ mpp-solar -s`
```
================ Status ==================
Parameter                       Value           Unit
ac_input_frequency              00.0            Hz
ac_input_voltage                000.0           V
ac_output_active_power          0152            W
ac_output_apparent_power        0207            VA
ac_output_frequency             50.0            Hz
ac_output_load                  004             %
ac_output_voltage               230.2           V
allowscconflag                  01
battery_capacity                100             %
battery_charging_current        018             A
battery_discharge_current       00000           A
battery_temperature             046             Deg_C
battery_voltage                 57.40           V
battery_voltage_from_scc        57.45           V
bus_voltage                     459             V
chargeaveragecurrent            00
fan_lock_status                 Not locked
fan_pwm_speed                   0030            Percent
inverter_charge_status          bulk stage
inverter_heat_sink_temperature  0057            Deg_C
inverter_temperature            034             Deg_C
pv_input_current_for_battery    0021            A
pv_input_voltage                069.9           V
scc_charge_power                1258            W
scc_flag                        SCC is powered and communicating
scc_pwm_temperature             051             Deg_C
sync_frequency                  50.00
transformer_temperature         057             Deg_C
```
