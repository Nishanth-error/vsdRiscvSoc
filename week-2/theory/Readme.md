# Week 2 Task – BabySoC Fundamentals & Functional Modelling 🚀

## Objective 🎯
The goal of this task is to understand **System-on-Chip (SoC) fundamentals** and practice **functional modelling** of the BabySoC using **Icarus Verilog** and **GTKWave**.  

---

## Part 1 – Theory (Conceptual Understanding) 📚

### What is a System-on-Chip (SoC)? 💡
A **System-on-Chip (SoC)** is a mini-computer on a single chip. It combines the CPU, memory, interfaces, and other components, making devices **smaller, faster, and more energy-efficient**.  

**Key benefits:**  
- 🏠 **Space Saving:** All components on one chip.  
- 🔋 **Energy Efficient:** Short connections reduce power consumption.  
- ⚡ **High Performance:** Data moves quickly inside the chip.  
- 💰 **Cost Effective:** Cheaper than using separate parts.  
- ✅ **Reliable:** Fewer parts, fewer failures.  

---

### Components of a Typical SoC 🧩
- **CPU 🧠:** Executes instructions and manages data.  
- **Memory 💾:**  
  - **RAM:** Temporary storage.  
  - **ROM/Flash:** Permanent storage.  
- **I/O Ports 🔌:** Connects to external devices like USB, cameras, audio.  
- **GPU 🎨:** Handles graphics and visuals.  
- **DSP 🎵:** Processes audio and video signals.  
- **Power Management ⚡:** Optimizes energy use.  
- **Special Features 🌐:** Optional modules like Wi-Fi, Bluetooth, or security.  

---

### Types of SoCs 🛠️
- **Microcontroller-based SoC:** Low-power devices like IoT gadgets.  
- **Microprocessor-based SoC:** Supports operating systems, used in smartphones/tablets.  
- **Application-Specific SoC (ASIC):** Optimized for tasks like AI, graphics, or multimedia.  

---

## BabySoC – A Learning SoC 🍼💻

**VSDBabySoC** is a small, open-source SoC using the **RVMYTH RISC-V CPU**. It is designed as a **learning platform** for digital and analog interfacing.  

### Key Components ⚙️

#### 1. RVMYTH CPU 🧠
- **Open-source RISC-V core** designed for simplicity and educational purposes.  
- Handles **instruction execution, data processing, and register management**.  
- In BabySoC, register **`r17`** is used to hold values for **DAC conversion**, allowing the system to generate analog outputs.  
- RVMYTH demonstrates **basic CPU functionality**, making it ideal for functional modelling and understanding core CPU operations.  

#### 2. Phase-Locked Loop (PLL) ⏱️
- A **control system** that generates a clock signal synchronized to an input reference.  
- BabySoC uses an **8x PLL** to produce a stable and synchronized clock for the CPU and DAC.  
- **Main components of a PLL:**  
  - **Phase Detector:** Compares input and output phases and generates an error signal.  
  - **Loop Filter:** Smooths the error signal into a control voltage.  
  - **Voltage-Controlled Oscillator (VCO):** Adjusts frequency based on control voltage.  
- **Function:** Ensures all components run in sync, prevents timing mismatches, and provides frequency stability.  

#### 3. Digital-to-Analog Converter (DAC) 🔊📺
- Converts **digital values from RVMYTH** into **analog signals**.  
- BabySoC uses a **10-bit DAC**, which can output **1024 distinct voltage levels**, allowing fine control over the analog signal.  
- Two common DAC types:  
  - **Weighted Resistor DAC:** Uses resistors of different weights.  
  - **R-2R Ladder DAC:** Uses a repeating resistor network for simpler and scalable design.  
- **Purpose in BabySoC:** Interface with devices like **speakers, TVs, or other analog systems**, demonstrating real-world digital-to-analog conversion.  

---

### BabySoC Functional Diagram 🖼️
                        
                                      ┌─────────────┐
                                      │  Clock In   │
                                      └─────┬───────┘
                                            │
                                           PLL ⏱️
                                            │
                                    ┌───────┴────────┐
                                    │   RVMYTH CPU   │ 🧠
                                    │  (Registers)   │
                                    └───────┬────────┘
                                            │
                                            ▼
                                           DAC 🔊
                                            │
                                   ┌────────┴────────┐
                                   │ ExternalDevices │ 📺🎵
                                   └─────────────────┘



---

### Functional Modelling 🧪
Functional modelling is done **before RTL or physical design** to:  
- ✅ Test the logic of each module.  
- ✅ Verify data flow between CPU, PLL, and DAC.  
- ✅ Ensure expected behavior in simulation.  

**Tools used:**  
- **Icarus Verilog:** Simulate BabySoC modules.  
- **GTKWave:** Visualize signals, clocks, and DAC outputs.  

---

## Summary ✨
BabySoC is a **hands-on platform** for understanding SoC fundamentals, including **CPU operation, clock generation, and digital-to-analog interfacing**. Functional modelling allows testing in a **simulation environment**, bridging the gap between theoretical concepts and real-world implementation.  

Learning BabySoC provides a strong foundation for **RTL design, timing analysis, and analog-digital integration** in modern embedded systems.  

---

**References 🔗**  
- [Fundamentals of SoC Design Notes](https://github.com/hemanthkumardm/SFAL-VSD-SoC-Journey/tree/main/11.%20Fundamentals%20of%20SoC%20Design)

