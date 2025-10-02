# VSDBabySoC Project

## What is VSDBabySoC?

VSDBabySoC is a small but powerful System on Chip (SoC) based on the RISC-V processor. It is designed to test three open-source components together and adjust its analog parts. The main parts are:

- RVMYTH microprocessor
- An 8x Phase-Locked Loop (PLL) that creates a steady clock signal
- A 10-bit Digital-to-Analog Converter (DAC) that connects to analog devices

## How It Works

### Starting Up and Clock Signal

When the SoC gets its first input, it turns on the PLL. The PLL creates a steady, synchronized clock signal that coordinates all parts of the SoC, especially the RVMYTH processor and the DAC. This clock signal makes sure everything works smoothly and at the right time.

### Processing Data in RVMYTH

RVMYTH, the brain of BabySoC, processes the data. It uses a special register called `r17` to store and cycle through values that will be sent to the DAC. As RVMYTH runs its instructions, it updates these values continuously so the DAC can convert them to analog signals.

### Creating Analog Signals with DAC

The DAC takes the digital data from RVMYTH and changes it into analog signals. These signals are saved in a file named `OUT` and can be sent to devices like TVs or mobile phones. These devices understand the analog signals and use them to play sounds or videos. This shows how digital data can be used to control real-world electronics.

![BabySoC Components](https://github.com/user-attachments/assets/38253bb7-b658-496d-a043-15402219e089)

## Key Parts of BabySoC

- **RVMYTH (RISC-V CPU):** This is the processor that runs programs and controls other parts of the SoC. It's simple, easy to customize, and good for learning how CPUs work.

- **Phase-Locked Loop (PLL):** It creates a steady clock signal to keep everything running in sync. The PLL makes sure all parts of the chip work together smoothly.

- **Digital-to-Analog Converter (DAC):** This changes digital signals from the processor into analog ones, like sounds or images, so that they can be used by other electronics.

## What is a Phase-Locked Loop (PLL)?

A PLL is a system that creates a signal which matches the phase and frequency of an input signal. This means the output signal has the same timing as the input, sometimes with a slight delay.

### How a PLL Works

- **Phase Detector:** Compares the input and output signals and finds the timing difference.
- **Loop Filter:** Smoothes out the difference signal to make a control voltage.
- **Voltage-Controlled Oscillator (VCO):** Changes its output frequency based on the control voltage to match the input signal.

### Why Use a PLL?

The PLL locks the output frequency to the input frequency and keeps their timing aligned. Sometimes, a frequency divider is used inside it so the output frequency can be a multiple of the input frequency.

### Why Not Always Use Off-Chip Clocks?

- Delays happen because the clock signal has to travel a long distance.
- The clock signal can flicker or jitter, causing timing problems.
- Different parts of a chip may need different clock speeds.
- Crystals used to generate clocks can have slight errors and change with temperature or age.

### PLL Block Diagram

![PLL Block Diagram](https://github.com/user-attachments/assets/217d602f-003d-4606-9bca-855a4832764c)

## What is a Digital-to-Analog Converter (DAC)?

A DAC turns digital data (bits of 0s and 1s) into an analog signal that can be used by things like speakers or displays.

### Types of DACs

- **Weighted Resistor DAC:** Uses resistors with different sizes to convert digital signals to voltages.

![Weighted Resistor DAC](https://github.com/user-attachments/assets/344e4ffd-7509-41e7-ac42-21a553b3db11)

- **R-2R Ladder DAC:** Uses a repeated pattern of resistors for an easier and scalable design.

![R-2R Ladder DAC](https://github.com/user-attachments/assets/5c15f424-1a94-4424-b019-a76c0ca0db43)
### DAC in VSDBabySoC

VSDBabySoC uses a 10-bit DAC, which means the digital signal has 10 bits, and the DAC converts it into analog output.

