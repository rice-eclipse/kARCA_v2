# kARCA 2.0 Main Documentation

This document highlights the features and design choices of kARCA 2.0 as a reference for future work on the project.


## Project Overview
kARCA 2.0 is an engine control system  that is part of Rice Eclipse's Engine Testing Setup. The controller interfaces directly with the engine through the controller software `slonk`, which runs on the Raspberry Pi. The user interfaces with the controller using a frontend JavaScript dashboard `Quonkboard` from which they can visualize the data collected and send commands to actuate valves and start the ignition sequence.

![Engine Test Setup](https://github.com/rice-eclipse/kARCA_v2/tree/main/karca_v2/Docs/readme_images/engine_test_setup.png)

## System Requirements
1. Collect data from 4 load cells, pressure transducers, and thermocouples at a 5kHz samplingfrequency
2. Control the 600W ignition system by driving an external two-stage relay
3. Control 5 valves, each drawing up to 1A of current at 12V
4. Monitor Battery voltage and current
5. Transmit real time data to the user

## Design Outline

### Power Distribution

The main power source is a 12V Lead-Acid Battery, which is spliit into 5V and 12V lines.

The 12V line is used for the drivers, ignition circuitry, and the pressure transducers.

The 5V line is used for the remaining ciruitry. It is supplied by a DC-DC Power converter module, which is essentially a low-noise switching coverter. This minimizes power dissipation while reducing switching noise.

The battery current is monitored using a current shunt and a current sense amplifier. The voltage is monitored using a simple resistive divider circuit.

![Power Regulation](https://github.com/rice-eclipse/kARCA_v2/tree/main/karca_v2/Docs/Schematics/Regulators.jpg)
![Battery Monitoring](https://github.com/rice-eclipse/kARCA_v2/tree/main/karca_v2/Docs/Schematics/Regulators_Battery-monitoring.jpg)

### Load Cell Data Collection
When supplied with an excitation voltage of 5V, the cell outputs a differential voltage proportonal to the applied force:
$$V_{dm} = \frac{V_{FSO}}{F_{FSO}}V_{DD}F$$
Where:
1. $V_{dm}$ is the output differential voltage
2. $V_{FSO}$ is the load cell's full scale voltage output: 3mV per volt of excitation when 3000lbf is applied
3. $V_{DD}$ is the exccitation voltage(5V in this case)
4. $F$ is the applied force

$V_{dm}$ is amplified using an instrumentation amplifier to obtain a single-ended readable signal (compatible with the ADC voltage resolution of 1.22mV). A two-pin jumper can be used to select the gain resistor connected to the instrumentation amplifier. A gain of 250 for the Titan load cell or a gain of 17 for the Proxima load cell can be chosen.

The single-ended signal is then filtered using a unity-gain active filter with a cutoff frequency of $1.5kHz$. This is to prevent aliasing during sampling.
![Load Cell Amplification](https://github.com/rice-eclipse/kARCA_v2/tree/main/karca_v2/Docs/Schematics/Load_Cells_amplification.jpg)

### 

