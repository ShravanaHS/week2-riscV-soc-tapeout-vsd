# week2-riscV-soc-tapeout-vsd
week 2 tasks and project submissions for the VSD RISC-V SoC Tape out program, documenting step-by-step progress from Week 0 to final tape out.


# BabySoC Fundamentals & Functional Modelling  

This repository documents the **Week 2 task of the SFAL-VSD-SoC Journey**.  
The objective is to build a solid understanding of **System-on-a-Chip (SoC) fundamentals** and practice **functional modelling of the BabySoC** using **Icarus Verilog** and **GTKWave**.  

---

## Part 1: Foundational Concepts (Theory)  

### What is a System-on-a-Chip (SoC)?  
A **System-on-a-Chip (SoC)** is a single integrated circuit that combines all the essential components of a computer or electronic system onto one chip. It integrates CPU, memory, peripherals, and interconnect into a power-efficient unit, widely used in smartphones, IoT, and embedded systems.  

### Key Components of a Typical SoC  
- **Processor Core (CPU):** Executes instructions (may include DSPs or accelerators).  
- **Memory:** SRAM, ROM, and DRAM interfaces.  
- **Peripherals/Interfaces:** UART, SPI, I2C, USB, sensor/display controllers.  
- **Interconnect:** Bus/NoC for communication among blocks.  

### VSDBabySoC Architecture  
The **VSDBabySoC** integrates three IP cores:  
- **RVMYTH RISC-V CPU**  
- **Phase-Locked Loop (PLL)** (8x frequency generator)  
- **10-bit DAC**  

### CPU Core (RISC-V RVMYTH)  
Implements the **RISC-V ISA**, using:  
- Instruction Decoder  
- Register File  
- Arithmetic Logic Unit (ALU)  
- Control Unit  

---

## Part 2: Labs (Hands-on Functional Modelling)  

### Step 1: Install Simulation Tools  

#### Windows  
1. Download from [http://bleyer.org/icarus/](http://bleyer.org/icarus/)  
2. Run installer and enable **Add to PATH**  
3. Verify:  
iverilog

text

#### macOS  
brew install icarus-verilog
brew install gtkwave

text

#### Linux (Ubuntu/Debian)  
sudo apt update
sudo apt install iverilog gtkwave

text

---

### Step 2: Clone Repository  
git clone https://github.com/hemanthkumardm/SFAL-VSD-SoC-Journey.git
cd SFAL-VSD-SoC-Journey/12.\ VSDBabySoC\ Project/

text

---

### Step 3: Compile and Simulate  
Compile Verilog design and testbench:  
iverilog -o babysoc.vvp tb_VSDBabySoC.v VSDBabySoC.v RVMYTH.v

text
Run simulation:  
vvp babysoc.vvp

text
Generates waveform file: `dump.vcd`.  

---

### Step 4: View Waveforms in GTKWave  
gtkwave dump.vcd

text

#### Suggested Checks  
- **Reset Behavior:** PC initialized correctly.  
- **Clocking:** Signals change on active clock edges.  
- **Instruction Fetch:** `pc` increments by 4, memory responds with data.  
- **Dataflow:** Trace signals like `mem_addr` and `mem_rdata`.  

---

## Part 3: Deliverables  

### 1. Simulation Logs  
Include `$display` / `$monitor` outputs from terminal.  

### 2. GTKWave Screenshots  
Example:  

**Reset Operation**  
![Reset Example](https://i.imgur.com/placeholder_reset.png)  

*At reset = 1, `pc_out` holds `0x00000000`. When reset is de-asserted, pc_out increments on rising clock edges.*  

**Instruction Fetch**  
*Shows program counter incrementing by 4 each cycle, fetching sequential instructions.*  

(Add screenshots for clocking and dataflow).  

---

## Outcome  
- Understood **SoC fundamentals**.  
- Modeled and simulated the **BabySoC**.  
- Verified reset, instruction fetch, and dataflow using Icarus Verilog + GTKWave.  
