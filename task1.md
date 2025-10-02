
# Week 2 – BabySoC Fundamentals & Functional Modelling  

##  Objective  
To build a solid understanding of **SoC fundamentals** and practice **functional modelling** of the BabySoC using **Icarus Verilog** & **GTKWave**.  

---

# Table of Contents

1. [Introduction What is an SoC?](#1-introduction-what-is-an-soc)
2. [SoC vs Microcontroller In-depth Comparison](#2-soc-vs-microcontroller-in-depth-comparison)
3. [SoC Block-by-Block](#3-soc-block-by-block)
4. [System on Chip (SoC) Real-world Applications & Overview](#4-system-on-chip-soc-real-world-applications--overview)
5. [References & Further Reading](#5-references--further-reading)


---

# 1. Introduction What is an SoC?
A **System on Chip (SoC)** is a complete electronic system packaged into a single integrated circuit (chip). Instead of using separate chips for CPU, memory, peripherals, and analog/electrical interfaces, an SoC combines many of those blocks on one piece of silicon.

**Key idea:** an SoC packs many functions close together on a chip so they can operate with low power, low latency, and small size. The result is what powers modern smartphones, tablets, wearables, smart appliances, IoT nodes, cars, and much more.

### Why SoCs exist (short)
- **Miniaturization:** Space-limited products need a tiny stack of electronics.  
- **Power efficiency:** On-chip integration reduces power spent driving signals between chips.  
- **Performance:** Shorter interconnections → faster communication → more throughput.  
- **Cost:** Mass-producing a single chip can be cheaper than many discrete chips.

---

# 2. SoC vs Microcontroller In-depth Comparison

**Microcontroller (MCU)** and **SoC** are sometimes mixed up. They overlap, but there are important differences.

### What is a Microcontroller (MCU)?
An MCU is a single integrated circuit with:
- CPU core (often single low-power core)
- Small amount of flash (program memory) and RAM
- Built-in peripherals (GPIO, timers, UART, SPI, I²C)
- Designed for embedded control tasks (sensors, motor control, simple devices)

**Examples:** STM32, AVR (ATmega), PIC, ESP32.

### How an SoC differs
- **Scale & complexity:** SoCs can contain multiple CPU cores, GPUs, DSPs, hardware accelerators, large memory controllers (DRAM), high-speed interfaces (PCIe, USB-C, MIPI), analog transceivers (cellular/Wi-Fi), and complex power management.
- **Target devices:** SoCs power smartphones, tablets, single-board computers (Raspberry Pi), and advanced IoT gateways. MCUs power simple controllers and low-power sensors.
- **Software complexity:** SoCs often run full OSes (Linux, Android) and require complex bootloaders, whereas MCUs run bare-metal code or simple RTOS.
- **Fabrication & cost:** SoCs are typically fabricated on advanced process nodes (e.g., 14nm, 7nm, or open-source Sky130 for educational flows), while many MCUs use mature nodes for cost-effectiveness.

### Simple analogy
- MCU = a *single-skill worker* with a toolbox for fixed tasks.  
- SoC = a *small team/department* with specialists (CPU, GPU, DSP, comms) working together.

---

# 3. SoC Block-by-Block

For each block we’ll cover:
- **What it is**  
- **Why it’s used**  
- **What it does (function)**  
- **Mobile phone example**  
- **Types (if applicable)**  
- **Simple example / code / notes**

---

## CPU (Processor)

### What it is
The CPU is the central processing unit — executes instructions, runs the OS and applications, schedules tasks, and coordinates data movement inside the SoC.

### Why used
It performs the general-purpose computation required by software (control flow, arithmetic, decision making).

### What it does (function)
- Fetches instructions from memory  
- Decodes them  
- Executes arithmetic/logic operations  
- Reads/writes memory and I/O registers  
- Manages control flow (branches/interrupts)

### Mobile phone example
The smartphone app code (UI, networking stack, frameworks) runs on the SoC CPU. Multiple CPUs/cores mean background tasks (notifications) can run while the foreground app responds.

### Types & variants
- **RISC (Reduced Instruction Set Computer)** — e.g., ARM, RISC-V. Simpler instruction sets, efficient for pipelines.  
- **CISC (Complex Instruction Set Computer)** — historically x86. Complex instructions; some modern chips blend styles.  
- **Single-core vs multi-core** — performance and power tradeoffs.  
- **Application CPUs vs real-time CPUs** — SoCs often include both: high-performance cores (for apps) and small real-time cores for sensor management

### Simple CPU pseudocode (fetch-execute loop)
```c
while (1) {
    instr = fetch(PC);      // read instruction from memory
    decoded = decode(instr); 
    result = execute(decoded); // ALU, logic, memory
    writeback(result);      
    PC = nextPC(decoded);   // update program counter
}
```
## Von Neumann vs Harvard Architectures

### 🔹 Von Neumann Architecture
- **Single memory space** for both instructions and data.  
- The CPU fetches instructions **and** data from the same memory.  
- **Simpler to program** and implement.  
- Limitation: Can suffer from **bus contention** because both instruction fetch and data access share the same bus.  
  - This bottleneck is often called the **"Von Neumann Bottleneck"**.  

#### ✅ Example (Mobile Phone)
When you open an app, the processor loads both the **instructions** (how the app should run) and the **data** (like your photos or text messages) from the **same memory space**.  
This can slow down performance if both need to be fetched at the same time.  

---

### 🔹 Harvard Architecture
- **Separate memory spaces** (and often buses) for instructions and data.  
- The CPU can **fetch an instruction and read/write data simultaneously** → improves throughput.  
- Commonly used in **DSPs (Digital Signal Processors)** and many **microcontrollers**.  

#### ✅ Example (Mobile Phone / Audio Processing)
When playing a song, the DSP in your phone can **fetch audio processing instructions** while **reading the music data** at the same time.  
This allows **real-time sound enhancement** (like noise cancellation) without delays.  

---

### 🔹 Which is Used Where?
- **General-purpose CPUs** (PCs, laptops, smartphones)  
  - Conceptually follow **Von Neumann** at the ISA level.  
  - But internally, they use **Harvard-like microarchitectures** (separate instruction/data caches and pipelines).  

- **Microcontrollers (MCUs)** and **DSPs**  
  - Often implement a **true Harvard architecture** for predictable timing and real-time efficiency.  

---

#### 🔹 Simple Example / Consequence
- In **Von Neumann**, if the CPU wants to fetch an instruction and read data, it has to wait for one to finish before starting the other.  
- In **Harvard**, both can happen in the **same cycle**.  
- This makes Harvard better for:  
  - Tight loops  
  - DSP processing  
  - Microcontrollers with **real-time constraints**  

---
## Memory in SoC (RAM, ROM, Flash, Cache)

### 🔹 What it is  
Memory components in an SoC are responsible for **holding code and data**.  
They exist in different levels and types, optimized for:  
- **Speed** (fast access for the CPU)  
- **Persistence** (whether data survives power cycles)  
- **Cost & capacity** (larger memories are cheaper but slower)  

---

### 🔹 Why it is used  
Different tasks in a system require **different memory properties**:  
- Some data must be **fast** (CPU instructions).  
- Some data must be **permanent** (firmware, OS).  
- Some memory must be **cheap and large** (apps, media).  

---

### 🔹 What it does (Functions of Different Memories)

### 🟢 RAM (Random Access Memory)  
- **Volatile** → data is lost when power is turned off.  
- Stores **runtime working data** such as stacks, heap, and buffers.  
- **Fast** enough for CPU and GPU execution.  

---

### 🟢 ROM (Read-Only Memory) / Flash  
- **Non-volatile** → retains data after power off.  
- Stores **firmware, bootloaders, and permanent instructions**.  
- Flash memory is **reprogrammable** (unlike true ROM).  

---

### 🟢 DRAM (Dynamic RAM)  
- Used as **main system memory**.  
- **Larger and cheaper** than SRAM, but slower.  
- Needs **constant refresh cycles** to maintain data.  
- Example in phones: **LPDDR (Low-Power DRAM)** used for multitasking and media apps

Simple examples & code
Access memory in C:
```c
int array[100];      // allocated in RAM
const char fw[] = "..."; // stored in flash (read-only)

```
---
## Interconnect in SoC (Buses, Crossbars, Network-on-Chip)

### 🔹 What it is  
The **interconnect** is the communication fabric inside an SoC.  
It connects different blocks like:  
- CPU(s)  
- Memory controllers  
- Peripherals  
- Hardware accelerators (GPU, DSP, ISP, etc.)  

---

### 🔹 Why it is used  
All blocks in an SoC need to **exchange data**. Interconnect ensures:  
- Efficient **data transfer** between components.  
- **Bandwidth management** (who gets access when multiple devices request).  
- **Arbitration & QoS (Quality of Service)** for real-time systems.  

---

### 🔹 What it does  
- Routes **reads/writes** between **masters** (CPU, DMA) and **slaves** (memory, peripherals).  
- Handles **priority & bandwidth partitioning**.  
- Maintains **cache coherency** in multi-core CPUs.  

---

### 🔹 Types of Interconnects  

#### 🟢 Shared Bus (e.g., AMBA AHB/APB)  
- Simple structure.  
- All devices share a single communication channel.  
- **Limitation:** Bandwidth bottleneck when multiple devices request simultaneously.  
- Example: Used in small microcontrollers and simple SoCs.  

#### 🟢 Crossbar  
- Multiple parallel paths for communication.  
- Allows **higher throughput** by enabling multiple transfers at the same time.  
- Example: Used in mid-range SoCs where CPUs, memory, and peripherals need frequent access.  

#### 🟢 Network-on-Chip (NoC)  
- Works like a computer network inside the chip (packet-switched).  
- Scalable to many cores and accelerators.  
- Provides **high bandwidth, low latency, and QoS features**.  
- Example: Modern smartphone SoCs with multiple CPU clusters, GPUs, ISPs, and AI accelerators.  

---

### 📱 Mobile Phone Example  
- A high-bandwidth NoC connects:  
  - **CPU clusters** (performance + efficiency cores)  
  - **GPU** (for gaming and graphics)  
  - **ISP (Image Signal Processor)** for camera  
  - **Memory controller (DRAM access)**  
- Ensures smooth multitasking, gaming, and camera capture without lag.  

---

## Peripherals & I/O in SoC (UART, SPI, I²C, USB, GPIO, Camera & Display Interfaces)

### 🔹 What they are  
**Peripheral controllers** provide standard interfaces to **external devices and sensors**.  

---

### 🔹 Why they are used  
- To **interact with the outside world** (sensors, displays, storage, radios).  
- Allow data transfer between SoC and external chips/modules.  

---

### 🔹 What they do (Functions of Peripherals)  

#### 🟢 UART (Universal Asynchronous Receiver-Transmitter)  
- Simple **serial communication**.  
- Used for debugging, console access, or low-speed device communication.  

#### 🟢 SPI (Serial Peripheral Interface)  
- **High-speed short-distance communication**.  
- Commonly used with flash memory, sensors, and displays.  

#### 🟢 I²C (Inter-Integrated Circuit)  
- **Two-wire protocol (SDA + SCL)**.  
- Lower speed than SPI, but supports multiple devices on the same bus.  
- Widely used for configuration chips, touch controllers, and sensors.  

#### 🟢 USB / MIPI / PCIe  
- **High-bandwidth communication** for external devices.  
- Examples: storage (USB drives), camera (MIPI CSI), display (MIPI DSI, DisplayPort).  

#### 🟢 GPIO (General Purpose Input/Output)  
- Configurable pins.  
- Used for LEDs, buttons, interrupts, and control signals.  

---

### 📱 Mobile Phone Example  
- **I²C** connects touch-screen controllers and power-management ICs.  
- **MIPI CSI** carries camera data from the sensor to the ISP.  
- **MIPI DSI / DisplayPort** drives the display.  
- **USB-C** provides charging, file transfer, and external device connection.  

---

### 🔹 Simple Example in C  
```c
i2c_start();
i2c_write(device_address<<1 | 0); // write mode
i2c_write(register_address);
i2c_restart();
i2c_write(device_address<<1 | 1); // read mode
data = i2c_read_nack();
i2c_stop();
```

---
## Graphics Processing Unit (GPU)

### 🔹 What it is  
A **specialized compute block** for rendering graphics (2D/3D) and accelerating UI/compositing tasks. Often programmable via shaders.

---

### 🔹 Why used  
- Graphics tasks are **parallel and compute-heavy**.  
- GPUs accelerate these tasks with **many parallel cores**, offloading work from the CPU.

---

### 🔹 What it does  
- Rasterization, texture mapping, pixel shading.  
- UI rendering, 3D game graphics, and video decoding.  

---

### 📱 Mobile Phone Example  
- Rendering home screen animations, games, and videos are handled by the GPU.  

---

### 🔹 Types  
- **Integrated GPU**: inside the SoC (typical for smartphones).  
- **Discrete GPU**: separate chip (desktops/laptops).  

---

## Digital Signal Processor (DSP)

### 🔹 What it is  
Processor specialized for **signal-processing tasks** (audio codecs, filters, sensor fusion) with features like SIMD instructions and predictable timing.

---

### 🔹 Why used  
- Repetitive math operations with **tight real-time constraints**.  
- Efficiently handles audio, image, and sensor processing.

---

### 🔹 What it does  
- Real-time audio processing, voice codecs (AAC/MP3).  
- Image pre-processing, camera pipelines.  
- Sensor data fusion and filtering.

---

### 📱 Mobile Phone Example  
- Noise suppression during calls.  
- Voice recognition pre-processing.  
- Camera image pre-processing.  

---

### 🔹 Simple DSP Example — FIR Filter in C

```c
// y[n] = sum_{k=0}^{N-1} b[k] * x[n-k]
float fir(float *b, float *x, int N) {
    float y = 0.0f;
    for (int k = 0; k < N; ++k) 
        y += b[k]*x[k];
    return y;
}
```

---
## Clocking & PLLs (Phase-Locked Loops)

### 🔹 What it is  
A **PLL** generates stable clock signals by locking an oscillator to a reference frequency/phase.

---

### 🔹 Why used  
- Provides **accurate clocks** for CPU, memory, and peripherals.  
- Supports **multiple clock domains** with different frequencies.  

---

### 🔹 What it does (Main Blocks)  
- **Phase Detector (PD):** compares reference and feedback phases.  
- **Loop Filter:** converts PD output to control voltage.  
- **Voltage-Controlled Oscillator (VCO):** frequency changes with control voltage.  
- **Feedback Divider:** multiplies/divides frequency as needed.  

---

### 📱 Mobile Phone Example  
- CPU cluster may run at 2.4 GHz, audio at 48 kHz — PLLs generate these derived clocks.  

---

### 🔹 Why not only off-chip clocks?  
- Reduces jitter.  
- Allows **multiple frequencies** on-chip.  
- Shorter distribution lines → faster, stable clocks.  

---

## Analog Blocks (ADC, DAC, Analog Front-Ends)

### 🔹 What it is  
Analog components **convert real-world signals** to digital and vice versa.  
Includes **ADC, DAC, amplifiers, filters, mixers**.  

---

### 🔹 Why used  
- Real-world interfaces like **microphone, radio, sensors** require analog front-ends.  

---

### 🔹 What it does  
- **ADC:** analog → digital (e.g., microphone input).  
- **DAC:** digital → analog (e.g., audio output).  
- **Analog amplifiers/filters/mixers:** process radio or sensor signals.  

---

### 📱 Mobile Phone Example  
- ADC digitizes microphone for calls.  
- DAC outputs audio to headphones.  
- Radio front-end uses LNAs, mixers, and ADCs.  

---
---
## Power Management in SoC (PMIC, DVFS, Power Domains)

### 🔹 What it is  
**PMICs** and on-chip regulators manage **voltages, sequencing, and power states**.  

---

### 🔹 Why used  
- Extend **battery life**.  
- Manage **thermal limits**.  
- **Isolate power** to unused blocks.  

---

### 🔹 What it does  
- Regulates voltage rails for CPU, GPU, and memory.  
- Implements **DVFS** (Dynamic Voltage and Frequency Scaling) to save power under light load.  
- Controls **power gating** to turn off blocks.  
- Monitors **battery charging and thermal conditions**.  

---

### 📱 Mobile Phone Example  
- Idle phone → CPU frequency reduced, some cores off, display dimmed.  
- Coordinated by PMIC and OS.  

---



# 4.System on Chip (SoC) Real-world Applications & Overview

---
## 1. Where SoCs Are Found — Real-world Examples & Use Cases

- **Smartphones / Tablets:** Most modern phones use complex SoCs integrating CPU, GPU, ISPs, and AI accelerators.  
- **Wearables:** Low-power SoCs handle step counting, heart-rate, notifications.  
- **Smart Home / IoT:** SoCs with Wi-Fi/BLE for sensors, smart bulbs, plugs.  
- **Consumer Electronics:** TVs, set-top boxes, streaming sticks.  
- **Automotive:** Infotainment and ADAS systems use SoCs for high reliability.  
- **Edge / AI Inference Nodes:** SoCs with NPUs for on-device machine learning.

---

## 2. Popular SoC Families (Overview)

| Manufacturer | Example SoCs | Notes |
|--------------|-------------|------|
| Apple        | A / M Series | High-performance, phones & Macs |
| Qualcomm     | Snapdragon   | Android phones, integrated modem options |
| Samsung      | Exynos       | Samsung devices |
| MediaTek     | Dimensity    | Cost/performance optimized phones |
| NVIDIA       | Tegra        | Multimedia and Jetson AI platforms |
| Broadcom     | BCM          | Raspberry Pi SoCs |
| NXP / TI / Qualcomm | i.MX, OMAP, MSM | Embedded & automotive applications |
| MCU-class    | STM32, NXP LPC, Atmel AVR, ESP32 | Integrated Wi-Fi/BLE (ESP32) |

*Add official links to datasheets or product pages in GitHub for references.*

---

## 3. Putting It Together — Typical SoC Boot & Data Flow (Mobile Phone Example)

### 🔹 Boot Flow (Simplified)
1. Power on → **PMIC releases power** to processor domain.  
2. **Boot ROM** (on-chip flash) runs first-stage bootloader.  
3. Bootloader initializes memory controllers, PLLs, and loads next stage (**U-Boot**) from flash.  
4. **OS kernel** (Linux/Android) loads from storage to DRAM.  
5. Device drivers initialize **peripherals** (display, radios, sensors).  
6. UI launches applications.

### 🔹 Data Flow Example — Playing Music
1. App requests music from **storage** (NAND/UFS).  
2. **Storage controller → DMA → DRAM**.  
3. **CPU / DSP** decodes audio into PCM samples.  
4. DSP applies filters and mixers (equalizer).  
5. **DAC** converts digital PCM → analog voltage → amplifier → speaker.  


---
# 5. References & Further Reading

- **Fundamentals of SoC Design** — [SFAL VSD SoC Journey Notes](https://github.com/hemanthkumardm/SFAL-VSD-SoC-Journey/tree/main/11.%20Fundamentals%20of%20SoC%20Design)  
- **RISC-V Official Documentation** — [RISC-V ISA Specifications](https://riscv.org/technical/specifications/)  
- **ARM Architecture Overview** — [ARM Developer Documentation](https://developer.arm.com/documentation)  
- **SkyWater Sky130 PDK Documentation** — [Sky130 Open-Source PDK](https://skywater-pdk.readthedocs.io/)  
- **Icarus Verilog & GTKWave** — [Icarus Verilog](http://iverilog.icarus.com/), [GTKWave](http://gtkwave.sourceforge.net/)  

