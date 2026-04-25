<h1 align="center"> SoC Communication Architecture: Peripheral → On-Chip Integration </h1>

<p align="center">
<img src="https://img.shields.io/badge/Peripheral%20Protocols-UART|SPI|I2C|USB-blue?style=for-the-badge"/>
<img src="https://img.shields.io/badge/On--Chip%20Protocols-AHB|APB-orange?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Design-RTL-green?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Goal-Complete%20SoC-purple?style=for-the-badge"/>
</p>

<p align="center">
<img src="https://img.shields.io/badge/Status-In%20Progress-yellow?style=flat-square"/>
<img src="https://img.shields.io/badge/Stage-Peripheral→OnChip-blue?style=flat-square"/>
<img src="https://img.shields.io/badge/Focus-System%20Architecture-informational?style=flat-square"/>
</p>

---

<p align="center">
This project shows the transition from <b>Peripheral Communication Protocols</b> to <b>On-Chip Bus Architecture</b> leading to a complete <b>SoC design</b>.
</p>

---

# Peripheral Protocols (Completed)

## UART (Asynchronous Serial Communication)

```mermaid
flowchart LR
    TX[Transmitter] --> DATA[Serial Data Stream]
    DATA --> RX[Receiver]
```

- Asynchronous communication  
- TX ↔ RX data transfer  
- No shared clock  

---

## SPI (Serial Peripheral Interface)

```mermaid
flowchart LR
    MASTER -->|MOSI| SLAVE
    SLAVE -->|MISO| MASTER
    MASTER -->|SCLK| SLAVE
    MASTER -->|SS| SLAVE
```

- Master-slave communication  
- Full duplex  
- High-speed synchronous protocol  

---

## I2C (Inter-Integrated Circuit)

```mermaid
flowchart LR
    MASTER -->|SCL| SLAVE1
    MASTER -->|SDA| SLAVE1
    MASTER -->|SCL| SLAVE2
    MASTER -->|SDA| SLAVE2
```

- Two-wire interface (SDA, SCL)  
- Multi-master support  
- Address-based communication  

---

## USB (Universal Serial Bus)

```mermaid
flowchart LR
    HOST -->|Token Packet| DEVICE
    DEVICE -->|Data Packet| HOST
    HOST -->|Handshake| DEVICE
```

- Packet-based communication  
- Host-device architecture  
- Differential signaling  
- High-speed data transfer  

---

# Limitation of Direct Peripheral Connection

```mermaid
flowchart LR
    CPU --> UART
    CPU --> SPI
    CPU --> I2C
    CPU --> USB
```

- Peripheral protocols (UART, SPI, I2C, USB) typically operate at **lower or moderate speeds compared to on-chip buses**  
- Direct CPU-to-peripheral connections increase routing complexity  
- Poor scalability as peripherals increase  
- Inefficient system design  

---

# On-Chip Protocols (Planned Development)

## AHB (Advanced High-performance Bus)

- We will be building a high-speed, low-latency on-chip interconnect  
- Will support burst transfers for efficient data movement  
- Will implement pipelined architecture for higher throughput  
- Target use: CPU, memory, and high-performance modules  


## APB (Advanced Peripheral Bus)

- We will be building a lightweight, non-pipelined bus  
- Optimized for low power and simple control logic  
- Designed for low-frequency peripheral communication  
- Target use: UART, SPI, I2C, USB, and similar peripherals  


## AHB-APB Bridge

- We will be designing a bridge between AHB and APB domains  
- Will handle protocol conversion and signal adaptation  
- Enables seamless communication between high-speed core and peripherals  


---

# SoC Architecture

```mermaid
flowchart LR
    CPU[CPU] --> AHB[AHB BUS]
    AHB --> MEM[Memory]
    AHB --> BRIDGE[AHB-APB Bridge]
    BRIDGE --> APB[APB BUS]

    APB --> WUART[APB UART Wrapper]
    APB --> WSPI[APB SPI Wrapper]
    APB --> WI2C[APB I2C Wrapper]
    APB --> WUSB[APB USB Wrapper]

    WUART --> UART
    WSPI --> SPI
    WI2C --> I2C
    WUSB --> USB
```
---

# Data Flow Example

```mermaid
flowchart LR
    CPU[CPU Address Data] --> AHB[AHB BUS]
    AHB --> BRIDGE[Protocol Conversion]
    BRIDGE --> APB[APB BUS]
    APB --> UART[UART Peripheral]
    UART --> DONE[Data Stored]
```

---

# Implementation Status

## Completed (Peripheral IPs)

- UART RTL implementation  
- SPI RTL implementation  
- I2C RTL implementation  
- USB RTL implementation  

Independent, reusable peripheral IP blocks developed and verified  

---

## Planned Development

### Phase 1: On-Chip Bus Design
- APB Bus (low-speed peripheral interconnect)  
- AHB Bus (high-performance system bus)  
- AHB Master (CPU-side transaction generator)  

### Phase 2: Peripheral Integration (APB Wrappers)
- APB UART  
- APB SPI  
- APB I2C  
- APB USB  
- APB RAM  

### Phase 3: Interconnect
- AHB to APB Bridge (protocol conversion + domain interfacing)  

### Phase 4: System Integration
- SoC Top-level design  
- Address decoding logic  
- Memory mapping and peripheral addressing  

### Phase 5: Advanced Integration
- RISC-V Core integration  

---

## Final Architecture Goal

```mermaid
flowchart LR
    CPU --> AHB
    AHB --> MEMORY
    AHB --> BRIDGE
    BRIDGE --> APB
    APB --> UART
    APB --> SPI
    APB --> I2C
    APB --> USB
```

A complete SoC integrating high-speed and low-speed domains using AMBA architecture  

---

# Planned Structure

```
ahb_bus/
apb_bus/
ahb_master/
ahb_apb_bridge/
apb_uart/
apb_spi/
apb_i2c/
apb_usb/
apb_ram/
soc_top/
riscv_core/
```

---

<p align="center"><b>
This repository shows the transition from standalone peripheral RTL to AMBA-based SoC architecture, progressing toward full system integration.
</p>

---
