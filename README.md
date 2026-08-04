<p align="center">
  <img src="https://img.shields.io/badge/Platform-STM32-blue?style=for-the-badge&logo=stmicroelectronics" alt="STM32"/>
  <img src="https://img.shields.io/badge/Language-C%2FC%2B%2B-informational?style=for-the-badge&logo=c" alt="C/C++"/>
  <img src="https://img.shields.io/badge/AI-Embedded%20ML%20%7C%20TinyML-brightgreen?style=for-the-badge&logo=tensorflow" alt="Embedded ML"/>
  <img src="https://img.shields.io/badge/Connectivity-BLE%20%7C%20WiFi%20%7C%20MQTT-orange?style=for-the-badge&logo=bluetooth" alt="Connectivity"/>
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge" alt="License"/>
</p>

<h1 align="center">⚡ PredictiveMotorAI</h1>

<p align="center">
  <strong>Production-Grade STM32 Firmware Ecosystem for Real-Time Motor Control, High-Performance Multi-Sensor Fusion, IoT Connectivity, and On-Device TinyML Predictive Analytics.</strong>
</p>

---

## 📌 Executive Summary

**PredictiveMotorAI** is an advanced, multi-project embedded systems ecosystem built on the **STM32** microcontroller architecture. It serves as a comprehensive portfolio demonstrating the convergence of physical hardware control, high-frequency sensor interfaces, IoT telemetry, and **edge-deployed Machine Learning (TinyML)**.

This repository features self-contained, optimized firmware implementations designed to monitor industrial motor health, analyze vibration anomalies, log high-frequency physics telemetry to local storage, and broadcast live diagnostics over Bluetooth Low Energy (BLE) and Wi-Fi/MQTT protocols.

> [!IMPORTANT]
> This portfolio showcases engineering proficiency in raw peripheral driver development, DMA (Direct Memory Access) optimization, real-time operating systems (RTOS) networking, low-power states, and embedded artificial intelligence compiler integration.

---

## 🛠️ Core Engineering Skills & Technologies Demonstrated

### 💾 Lower-Level Firmware & Hardware Interfacing
*   **Hardware Platforms:** STM32 Arm Cortex-M4/M7 MCUs (including `NUCLEO-F401RE`, `STEVAL-STWINKT1B` STWIN SensorTile Wireless Industrial Node, and custom boards).
*   **Hardware Peripherals:** Deep integration of **DMA (Direct Memory Access)**, **Timers (Advanced Control Timers for PWM generation)**, **ADC (Analog-to-Digital Converters)**, **SPI**, **I2C**, **USART**, and **SDIO/SDMMC** for FAT File System interfaces.
*   **Sensor Drivers:** Bare-metal register-level configurations and API development for industrial MEMS sensors (including the `IIS2DLPC` accelerometer and `ISM330DHCX` 6-axis IMU).

### 🧠 Embedded Artificial Intelligence (TinyML)
*   **Edge Inference:** Integration of pre-compiled machine learning classifiers directly on MCU flash memory using **STM32Cube.AI** and **TensorFlow Lite for Microcontrollers (TFLM)**.
*   **In-Sensor Classification:** Utilizing the hardware **Machine Learning Core (MLC)** of advanced ST MEMS sensors to run decision-tree logic directly inside the sensor ASIC, minimizing MCU wakeups and maximizing energy efficiency.

### 📶 Industrial IoT & Wireless Telemetry
*   **Bluetooth Low Energy (BLE):** Developing custom GATT services and profiles to stream real-time motor health telemetry, vibration spectra, and anomaly alerts.
*   **Wi-Fi & TCP/IP NetX Duo:** Implementing robust TCP/IP network sockets using LwIP/Azure RTOS NetX Duo over Wi-Fi modules (ESP8266/custom).
*   **MQTT Telemetry:** Connecting devices to local and cloud MQTT brokers to publish telemetry in structured JSON formats.

---

## 📁 Repository Architecture & Project Matrix

This repository is organized into distinct, modular firmware projects targeting specific hardware topologies and features:

| Folder / Project | Technical Domain | Key Features & Implementation Details |
| :--- | :--- | :--- |
| 📁 **`SupcomAI`** <br> 📁 **`Projet_PFE`** | **TinyML / Predictive Maintenance** | Edge AI motor diagnostics models deployed on STM32. Processes vibration time-series data using FFT (Fast Fourier Transform), calculating statistical features (RMS, Peak-to-Peak) to run anomaly detection models on-device. |
| 📁 **`BLEMLC`** <br> 📁 **`BLEDefaultFw`** <br> 📁 **`Bluetooth`** | **Wireless BLE & Sensor MLC** | Configures Custom BLE profiles (v4.2/v5.0) to stream sensor telemetry. The MLC project utilizes the ST `ISM330DHCX` sensor's internal Machine Learning Core to classify movement/vibration patterns with zero MCU overhead. |
| 📁 **`DATALOG2-STWIN.box`** <br> 📁 **`SDDataLogFileX`** | **High-Speed Data Acquisition** | Implements DMA-backed double-buffering schemes to stream multi-channel sensor data directly into an SD Card via SPI/SDIO using the `FatFS` library, preventing data loss during write cycles. |
| 📁 **`IIS2DLPC`** <br> 📁 **`ISM330DHCX`** | **MEMS Hardware Drivers** | Custom C driver interfaces for high-performance industrial-grade accelerometers and gyroscopes. Supports raw FIFO buffer reading, interrupt handling (data-ready, wake-up), and register configuration. |
| 📁 **`Nx_MQTT_Client`** <br> 📁 **`Wifi_MQTT`** | **IoT Wireless Protocols** | Dynamic IP leasing via DHCP, network connection stabilization, and MQTT client configurations. Publishes periodic diagnostics packets to IoT endpoints. |
| 📁 **`Led`** <br> 📁 **`uart2`** | **Low-Level Peripherals** | Basic hardware abstraction layer (HAL) and low-level (LL) drivers demonstrating clock configurations, GPIO management, and interrupt-driven UART ring-buffer communications. |

---

## 🧠 TinyML Implementation Pipeline


<div align="center">
<pre>
 [Industrial Motor] 
         │
         ▼
 ┌───────────────┐      SPI/I2C (DMA)       ┌──────────────────┐
 │ MEMS Sensors  │ ───────────────────────> │  STM32 MCU Core  │
 │ (ISM330DHCX)  │                          │  (Cortex-M4/M7)  │
 └───────┬───────┘                          └────────┬─────────┘
         │                                           │
         ▼ (In-Sensor Processing)                    ▼ (On-MCU Processing)
 ┌───────────────┐                          ┌──────────────────┐
 │ Machine Learn │                          │  TinyML Model    │
 │ Core (MLC)    │                          │  (STM32Cube.AI)  │
 └───────┬───────┘                          └────────┬─────────┘
         │                                           │
         └───────────────────┬───────────────────────┘
                             │
                             ▼ (Real-Time Decision)
                     ┌───────────────┐
                     │ Anomaly Alert │
                     │  & Telemetry  │
                     └───────┬───────┘
                             │
                             ▼ (BLE / WiFi MQTT)
                     ┌───────────────┐
                     │  IoT Gateway  │
                     └───────────────┘
</pre>
</div>


---

## 📚 Executive Report & Presentation

This repository represents the firmware portion of a massive research and engineering effort. It is accompanied by two key architectural documents:

*   📄 **100-page Engineering PDF Report:** Comprehensive analysis, mathematical models for predictive motor control, TinyML training methodology, neural network topologies, and experimental test results.
*   📊 **Technical Slide Presentation:** A highly visual executive summary highlighting the engineering achievements, hardware topologies, and real-time performance profiles.

> [!TIP]
> **Recruiters Only:** The full report and slide deck contain intellectual property and are hosted externally due to size limitations. Please email me at [mehdimejri15@gmail.com](mailto:mehdimejri15@gmail.com) or open an issue, and I will gladly share them with you!

---

## 🚀 Getting Started

### Hardware Prerequisites
*   **Target MCU:** STM32 Nucleo, Discovery, or STWIN industrial nodes.
*   **Programmer:** On-board ST-LINK/V2 or ST-LINK/V3 debugger.
*   **Sensors:** Compatible external sensor expansion boards or on-board MEMS.

### Software Prerequisites
*   [STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html) (GCC Compiler toolchain)
*   [STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html) (For peripheral configuration and initialization)
*   **X-CUBE-AI / X-CUBE-MEMS1** (For AI expansion packs and sensor algorithms)

### Installation & Build

1.  **Clone the Repository:**
    ```bash
    git clone https://github.com/Mejri-Mehdi/PredictiveMotorAI.git
    ```
2.  **Import to Workspace:**
    *   Open **STM32CubeIDE**.
    *   Navigate to `File` ➡️ `Import...` ➡️ `General` ➡️ `Existing Projects into Workspace`.
    *   Browse to the root of the cloned repository and select the specific project folder (e.g., `SupcomAI` or `Wifi_MQTT`) you wish to inspect.
3.  **Compile and Flash:**
    *   Right-click the imported project ➡️ `Build Project`.
    *   Connect your STM32 target board via USB.
    *   Click `Run` ➡️ `Run As` ➡️ `STM32 Cortex-M C/C++ Application` to compile, flash, and launch the firmware.

---

## ⚙️ Representative Implementation Code Snippet (MEMS Data Acquisition)

Here is a typical optimized register-read routing using low-overhead APIs to read the 6-axis acceleration data:

```c
#include "ism330dhcx_reg.h"

// High-speed, non-blocking polling sequence for 3D accelerometer data
int16_t data_raw_acceleration[3];
float acceleration_mg[3];

void Read_Sensor_Data(stmdev_ctx_t *ctx) {
    ism330dhcx_reg_t reg;
    
    // Check if new data is ready in the FIFO registers
    ism330dhcx_xl_flag_data_ready_get(ctx, &reg.status_reg.drdy_xl);
    if (reg.status_reg.drdy_xl) {
        // Read acceleration raw data (DMA preferred in production projects)
        memset(data_raw_acceleration, 0x00, 3 * sizeof(int16_t));
        ism330dhcx_acceleration_raw_get(ctx, data_raw_acceleration);
        
        // Convert LSBs to milligravities (mg) based on sensitivity factor
        acceleration_mg[0] = ism330dhcx_from_fs4g_to_mg(data_raw_acceleration[0]);
        acceleration_mg[1] = ism330dhcx_from_fs4g_to_mg(data_raw_acceleration[1]);
        acceleration_mg[2] = ism330dhcx_from_fs4g_to_mg(data_raw_acceleration[2]);
    }
}
```

---

## 📬 Contact & Inquiries

If you are a recruiter, engineering manager, or developer interested in my work on embedded systems, IoT, or TinyML:

*   📧 **Direct Email:** [mehdimejri15@gmail.com](mailto:mehdimejri15@gmail.com)
*   💼 **GitHub Profile:** [@Mejri-Mehdi](https://github.com/Mejri-Mehdi)
*   🚀 Feel free to open a **GitHub Issue** if you have questions about the STM32 codebase configuration!

---

## 📄 License
This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

<img width="1200" height="675" alt="Hardware Setup and Signal Processing Diagram" src="https://github.com/user-attachments/assets/976260b9-5958-49be-a51a-2643c2327908" />

<p align="center"><sub>Made with ❤️ by <a href="https://github.com/Mejri-Mehdi">Mejri Mehdi</a></sub></p>
