# Week 2 – BabySoC Fundamentals & Functional Modelling  

## 🎯 Objective  
To build a solid understanding of **SoC fundamentals** and practice **functional modelling** of the BabySoC using **Icarus Verilog** & **GTKWave**.  

---

## 📖 Part 1 – Theory  

### 🔹 What is a System-on-Chip (SoC)?  
A **System-on-Chip (SoC)** is like a **mini-computer on a single chip**. Instead of separate components, it integrates everything—CPU, memory, I/O, and peripherals—into one small, efficient package.  

👉 SoCs are widely used in **smartphones, tablets, wearables, IoT devices, and cars** because they save **space, power, and cost** while providing **high performance**.  

---

### 🔹 Key Components of an SoC  

- **CPU (Central Processing Unit)** – The “brain” that processes instructions.  
- **Memory** – RAM (temporary storage) + Flash/ROM (permanent storage).  
- **I/O Ports** – Connect external devices (USB, camera, headphones).  
- **GPU (Graphics Processing Unit)** – Handles visuals & animations.  
- **DSP (Digital Signal Processor)** – Specializes in audio/video processing.  
- **Power Management** – Ensures efficient power usage.  
- **Other Features** – Wi-Fi, Bluetooth, Security modules.  

✅ **Benefits of SoCs:**  
- Compact & portable  
- Low power consumption  
- High performance  
- Cost effective  
- Reliable  

---

### 🔹 Types of SoCs  

1. **Microcontroller-based SoCs** – Low power, simple tasks (IoT, appliances).  
2. **Microprocessor-based SoCs** – More powerful, run OS (phones, tablets).  
3. **Application-specific SoCs** – Optimized for specialized tasks (graphics, AI).  

---

### 🔹 Why BabySoC?  
BabySoC is a **simplified educational SoC model** built using open-source IPs.  
- Helps learners understand the **basics of SoC architecture**.  
- Provides a **hands-on functional model** before diving into RTL/physical design.  
- Uses **Sky130 technology** for open-source fabrication readiness.  

---

## 🏗️ Part 2 – Introduction to VSDBabySoC  

### ✨ Overview  
VSDBabySoC is based on the **RISC-V RVMYTH processor** and integrates:  
- **RVMYTH CPU** (RISC-V core)  
- **Phase-Locked Loop (PLL)** – Stable clock generation  
- **10-bit DAC** – Digital-to-Analog conversion  

**Flow of Operation:**  
1. PLL generates a **synchronized clock**.  
2. RVMYTH processes data in register `r17`.  
3. DAC converts digital values to **analog output (audio/video)**.  

---

### 🔹 BabySoC Components  

- **RVMYTH (RISC-V CPU)**  
   - Handles processing tasks.  
   - Simple, open-source, learner-friendly CPU.  

- **Phase-Locked Loop (PLL)**  
   - Aligns clock with a reference signal.  
   - Ensures all SoC components stay in sync.  
   - Solves clock distribution, jitter, and frequency stability issues.  

- **Digital-to-Analog Converter (DAC)**  
   - Converts digital values into analog output.  
   - VSDBabySoC uses a **10-bit DAC**.  
   - Allows BabySoC to **connect with TVs, speakers, mobile devices**.  

---

## ⚡ Part 3 – Functional Modelling  

Functional modelling is the **pre-RTL stage** where we test **system-level behavior** without worrying about implementation details.  

### Tools Used  
- **Icarus Verilog (iverilog)** – For simulation  
- **GTKWave** – For waveform visualization  

### Why Functional Modelling?  
- Validate **design concepts early**.  
- Identify **logical/system-level errors** before hardware implementation.  
- Ensure proper **interaction of CPU, PLL, and DAC**.  

---


## 🔗 References  

- [Fundamentals of SoC Design – Notes](https://github.com/hemanthkumardm/SFAL-VSD-SoC-Journey/tree/main/11.%20Fundamentals%20of%20SoC%20Design)  

