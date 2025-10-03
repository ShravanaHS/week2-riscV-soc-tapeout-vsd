# VSD RISC-V SoC Tapeout Program – Week 2
[![VSD](https://img.shields.io/badge/VSD-RISC--V%20SoC-blue?style=for-the-badge)](https://www.vlsisystemdesign.com/)
[![Linux](https://img.shields.io/badge/OS-Linux%20%7C%20Ubuntu-orange?style=for-the-badge&logo=linux)](www.linux.org)
[![Git](https://img.shields.io/badge/Version%20Control-Git-black?style=for-the-badge&logo=git)](https://github.com/)
[![RISC-V](https://img.shields.io/badge/Architecture-RISC--V-blue?style=for-the-badge&logo=riscv)](https://riscv.org/)

---
## 🎯 Week 2 Objective: BabySoC Fundamentals & Functional Modelling

This repository documents my progress and submissions for **Week 2** of the VSD RISC-V SoC Tapeout Program.

The primary focus this week was on the foundational principles of SoC design, moving from concept to a functional hardware model. This involved designing core RTL modules in Verilog, performing functional verification through simulation, and understanding the process of logic synthesis and optimization.

---
## 📝 Weekly Submissions

My work for this week is divided into two main tasks, covering the theoretical and practical aspects of functional modelling for the BabySoC.

| Task | Focus Area | View Submission |
|:----:|--------------------------------------------------|:----------------:|
| 1    | **SoC Design Fundamentals** | [**Task 1 Report**](https://github.com/ShravanaHS/week2-riscV-soc-tapeout-vsd/blob/main/task1.md) |
| 2    | **Hands-on Functional Modelling of BabySoC** | [**Task 2 Report**](https://github.com/ShravanaHS/week2-riscV-soc-tapeout-vsd/blob/main/task2.md) |

---
## 🛠️ Tools & Technologies Used

This week's tasks provided hands-on experience with the following open-source VLSI tools:
* **BabySoc** : [VSDBabySoC is a compact educational SoC](https://github.com/manili/VSDBabySoC)
* **Icarus Verilog**: The primary simulator used for compiling and running functional verification on the RTL designs.
* **GTKWave**: A graphical waveform viewer used to visualize and debug simulation outputs.

---
## ✅ Key Learnings & Competencies Gained


This project provides a practical, hands-on introduction to several core concepts in modern System-on-Chip design. The key takeaways from simulating the VSDBabySoC include:
* **Introduction to SoC** : build a solid understanding of SoC fundamentals.

* **Mixed-Signal Integration**: A clear, visual understanding of how digital components (the CPU) interface with and control analog components (the DAC) within a single chip.

* **RISC-V in Action**: A practical demonstration of a simple RISC-V core executing a program to perform a specific hardware task, illustrating the fundamentals of instruction set architecture.

* **Complete SoC Architecture**: A tangible example of how a CPU, a clocking system (PLL), and an I/O peripheral (DAC) work together to form a complete, functional system.

* **VLSI Toolchain Experience**: Hands-on use of a standard open-source simulation flow, including Verilog compilation (`iverilog`), simulation (`vvp`), and graphical waveform analysis (`GTKWave`).

---
### 🙏 Acknowledgments
I sincerely thank:  
- **Mr. Kunal Ghosh** and the entire **VSD (VLSI System Design)** team for this incredible learning opportunity.  
- The program mentors and the VSD community for their continuous guidance and support.  
- The open-source community for developing and maintaining essential tools like **Yosys, Icarus Verilog, and GTKWave**.
