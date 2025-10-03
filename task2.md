# VSDBabySoC: A Tiny Mixed-Signal RISC-V SoC


## 🌟 Project Overview

In the world of chip design, even the simplest System-on-Chip (SoC) can teach us how digital and analog domains come together on silicon. **VSDBabySoC** is a compact educational SoC designed to demonstrate this interaction.

The mission of this project is to test three open-source IPs in combination and demonstrate **digital-to-analog control**, where a software program running on a CPU generates a real-world analog signal.

---
## 🧩 Core Components

At its heart, VSDBabySoC integrates three key blocks, working together like a biological system:


* 🧠 **RVMYTH Core (The Brain)**: A lightweight, educational **RISC-V CPU**. It acts as the digital brain, fetching and executing a hardcoded program. Its primary job is to perform calculations and drive output data through a special register, `r17`.

* ⏱️ **Phase-Locked Loop (The Heartbeat)**: An **8x PLL** that generates a clean, stable internal clock from an input source. This clock signal synchronizes the CPU and DAC, ensuring all operations happen in perfect time.

* 🗣️ **Digital-to-Analog Converter (The Voice)**: A **10-bit DAC** that serves as the voice of the chip. It takes the 10-bit digital value from the CPU's `r17` register and outputs a proportional analog voltage, allowing the chip to "speak" to the analog world.

  ---
## 🏛️ Architectural Block Diagrams

The following diagrams illustrate the high-level architecture of the SoC and the internal structure of the Phase-Locked Loop (PLL).

### VSDBabySoC System Architecture

This diagram shows how the three main components are interconnected to form the complete System-on-Chip.

![BabySoC Components](https://github.com/user-attachments/assets/38253bb7-b658-496d-a043-15402219e089)

The data flow is as follows:
1.  An external clock (`CLK`) and a `reset` signal are provided as inputs.
2.  The **PLL** (the "Heartbeat") takes the input clock and generates a stable, synchronized internal clock that drives the entire system.
3.  The **RVMYTH CPU** (the "Brain") is driven by the PLL's clock. It executes its internal program, performing calculations and placing the result into the `r17` register.
4.  The 10-bit digital value from the CPU is sent to the DAC via the `RV_TO_DAC` bus.
5.  The **DAC** (the "Voice") converts this digital value into a proportional analog voltage, which is sent out through the `OUT` port.

### Phase-Locked Loop (PLL) Internal Block Diagram

A PLL is a feedback control system that generates a stable output clock signal from a reference input. This ensures precise timing across the chip, which is critical for high-speed digital operations.

![PLL Block Diagram](https://github.com/user-attachments/assets/217d602f-003d-4606-9bca-855a4832764c)

The core components of the PLL work in a continuous loop:
* **Phase Detector (PD)**: Compares the phase of the input reference clock against the feedback clock and outputs an error signal proportional to the difference.
* **Loop Filter (LF)**: A low-pass filter that smooths the error signal from the PD, converting it into a stable DC control voltage.
* **Voltage-Controlled Oscillator (VCO)**: An oscillator whose frequency is controlled by the voltage from the Loop Filter. It adjusts its frequency until the feedback clock matches the reference clock, achieving a "locked" state.
* ### Digital-to-Analog Converter (DAC) Architecture

A DAC is a mixed-signal device that converts a digital input signal (a binary number) into a corresponding analog voltage or current. In VSDBabySoC, the DAC is what allows the digital CPU to produce a real-world analog waveform.

The project uses a **10-bit R-2R Ladder DAC**, a popular and efficient architecture for integrated circuits.

![R-2R Ladder DAC](https://github.com/user-attachments/assets/5c15f424-1a94-4424-b019-a76c0ca0db43)

The key principles of the R-2R Ladder are:
* **Resistor Network**: It's built using a repeating pattern of resistors with only two values: `R` and `2R`. This makes it easier to manufacture accurately on a chip compared to other designs.
* **Binary-Weighted Summation**: Each digital input bit (from the `RV_TO_DAC` bus) controls a switch. Based on whether the bit is a `1` or `0`, the switch connects a branch of the ladder to a reference voltage or to ground.
* **Voltage Output**: The ladder network functions as a series of current dividers. It sums the currents from each bit, with each bit contributing a weighted amount to the final output voltage. The Most Significant Bit (MSB) has the largest impact, and the Least Significant Bit (LSB) has the smallest. The result is an analog voltage that is directly proportional to the binary input value.

---

---
## 🚀 Project Setup & Structure

### Step 1: Clone the Repository

First, clone the official VSDBabySoC project repository to your local machine using `git`.

```bash
git clone https://github.com/manili/VSDBabySoC.git
cd VSDBabySoC/
```

### Step 2: Understanding the Directory Structure

Once cloned, you will find the following directory structure. The key files for our simulation are located within the `src/module/` directory.

```txt
VSDBabySoC/
├── src/
│   ├── include/          # Header files (*.vh) for Verilog
│   └── module/           # All design and testbench modules
│       ├── vsdbabysoc.v  # Top-level SoC module
│       ├── rvmyth.tlv    # The RVMYTH CPU Core in TL-Verilog (source)
│       ├── avsdpll.v     # The PLL Verilog module
│       ├── avsddac.v     # The DAC Verilog module
│       └── testbench.v   # The simulation testbench
│
├── output/               # Directory for simulation results
```
This structure separates the core design files (`module/`) from other project assets, keeping the workspace organized.

## 🛠️ Prerequisites & Toolchain Setup

To replicate this simulation, you will need the following tools. This guide is based on a Debian/Ubuntu-based Linux distribution.

* **Icarus Verilog**: A Verilog simulator.
* **GTKWave**: A digital waveform viewer.
* **Python3 & Pip**: For running the TL-Verilog conversion tool.

You can install all prerequisites with the following command:
```bash
sudo apt update && sudo apt install iverilog gtkwave python3-venv python3-pip
```
## 🔧 TLV → Verilog Conversion

Since **RVMYTH** is written in **TL-Verilog (.tlv)**, we need to convert it to Verilog before simulating.

```bash
# Install tools
sudo apt update
sudo apt install python3-venv python3-pip

# Create virtual env
python3 -m venv sp_env
source sp_env/bin/activate

# Install SandPiper-SaaS
pip install pyyaml click sandpiper-saas

# Convert TLV → Verilog
sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
```

✅ Now you’ll have `rvmyth.v` alongside your other Verilog files.

---





---
## 🧪 Simulation Workflow

The simulation process involves three main steps: converting the CPU source code, compiling the design, and running the simulation.

### Step 1: Converting the RVMYTH Core (TLV to Verilog)

The **RVMYTH CPU core** is written in TL-Verilog (`.tlv`), a modern language that simplifies designing complex hardware. However, standard simulators like Icarus Verilog cannot read this format directly. Therefore, our first step is to convert `rvmyth.tlv` into standard Verilog (`.v`) using a free tool called **SandPiper**.

To ensure a clean and conflict-free installation of this tool, we will use a **Python virtual environment**. This is a critical best practice that prevents issues by isolating our project's dependencies from the system's global Python packages.
> **📝 Note: Why Use a Virtual Environment?**
>
> During my initial attempts, I tried to install SandPiper globally without a virtual environment (`sudo pip install ...`). This led to a few common problems:
> * **Permission Errors**: Installing packages system-wide often requires `sudo` privileges, which can be risky.
> * **Dependency Conflicts**: A global installation can clash with different versions of packages required by other applications on the system.
> * **System Clutter**: It "pollutes" the main system Python with packages that are only needed for this one project.
>
> Creating a virtual environment (`venv`) solves all these issues by creating a private, isolated "sandbox" for our project's tools.

Here are the commands to set up the environment and run the conversion:

```bash
# 1. Create a Python virtual environment named 'sp_env'
# This creates a self-contained folder for our Python tools.
python3 -m venv sp_env

# 2. Activate the virtual environment
# Your command prompt will now be prefixed with (sp_env).
source sp_env/bin/activate

# 3. Install the SandPiper-SaaS tool within the active environment
pip install pyyaml click sandpiper-saas

# 4. Run the conversion command
# This finds the .tlv file and generates rvmyth.v in the same directory.
sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
```
### Step 2: Compile the Verilog Design

With the `rvmyth.v` file successfully generated, all the necessary source files are now in place. The next step is to compile the entire design using Icarus Verilog (`iverilog`). This command gathers all the modules and the testbench and compiles them into a single simulation executable file.

```bash
# Compile the design and create an output file named 'pre_synth_sim.out'
iverilog -o pre_synth_sim.out \
  -DPRE_SYNTH_SIM \
  -I src/include -I src/module \
  src/module/testbench.v
```
### Step 3: Run the Simulation

Now, execute the compiled file using the `vvp` runtime engine. This will run the simulation defined in the testbench and generate a waveform file (`.vcd`) containing all the signal data.

```bash
# Run the compiled simulation executable
vvp pre_synth_sim.out
```
### **Output File Location**
 After this command finishes, it will create a waveform file named **`pre_synth_sim.vcd`** in your main project directory (the same directory where you are running the command). **Verification**: You can verify the simulation was successful by checking for the presence of the `pre_synth_sim.vcd` file.

 ---
## 📊 Results and Analysis

The final step is to visualize and analyze the generated waveform to verify that the SoC is functioning as expected.

### Loading the Waveform

Launch GTKWave and open the `pre_synth_sim.vcd` file that was generated in the previous step.

```bash
gtkwave pre_synth_sim.vcd
```


### Waveform Analysis

Inside GTKWave, you need to add the relevant signals to the viewer and set the correct format for the analog output.

1.  In the **"SST" panel**, expand the `vsdbabysoc_tb` hierarchy.
2.  Select the key signals: `CLK`, `reset`, `RV_TO_DAC[9:0]`, and `OUT`.
3.  Click the **"Append"** button to add them to the main wave panel.
4.  To properly visualize the DAC's output, **right-click** the `OUT` signal, and change its **Data Format** to **`Analog > Step`**.

![Final Simulation Waveform](link/to/your/image.png)

The final waveform clearly shows the successful operation of the SoC:

* **Initialization**: The `reset` signal starts high and then goes low, allowing the RVMYTH CPU to begin executing its program.
* **Digital Control**: The `RV_TO_DAC[9:0]` signal shows the digital value of the CPU's `r17` register. You can see it first ramp up and then begin to oscillate as the program runs through its two main loops.
* **Analog Output**: The `OUT` signal, representing the DAC's voltage, perfectly mirrors the digital `RV_TO_DAC` values. It creates a stepped ramp followed by an oscillation, visually confirming that the digital CPU is in complete control of the analog output.ts and Analysis



