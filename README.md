
<table>
  <tr>
    <td align="center"><img src="bm-lab-logo-white.jpg" alt="BM LABS Logo" width="200"/></td>
    <td align="center"><img src="chip_foundry_logo.png" alt="Chipfoundry Logo" width="200"/></td>
  </tr>
</table>


# Neuromorphic X1 – Analog In-Memory Compute IP Macro

Neuromorphic X1 is a **compact, ultra-efficient analog in-memory compute (AiMC) IP macro** featuring a **32 × 32 1T1R crossbar** for low-power matrix operations—ideal for edge AI and embedded IoT deployments.  
[View Product Page](https://bmsemi.io/commercial-neuromorphic-x1.html)

---

## 🔹 Key Features.
- **Analog in-memory MAC engine** in a 32 × 32 1T1R array  
- Drop-in, licenseable macro for energy-efficient computation at the edge  
- Designed for embedded / IoT AI where **energy** and **latency** are critical

---

## 📑 Summary
Neuromorphic X1 brings **in-memory compute** to compact silicon form factors, reducing data movement and significantly lowering power consumption for AI workloads.  
It is optimized for:
- Always-on AI (keyword spotting, anomaly detection)
- Edge AI in battery-powered devices
- Low-latency on-device processing

**At a glance:**
- **Architecture:** 32 × 32 1T1R analog crossbar  
- **Compute Type:** Multiply–Accumulate (MAC) in crossbar array  
- **Target Applications:** TinyML, sensing, embedded AI inference  
- **Benefits:** High energy efficiency, low latency, small area footprint

## 📷 Process to Write to ReRAM CIM

![Process to Write to ReRAM CIM](doc/Process_to_write_to_ReRAM_CIM.png)

**Figure Explanation:**  
This diagram illustrates the **data write process** to the Neuromorphic X1’s **32×32 ReRAM crossbar** via a **Wishbone interface**.  
- **1 page = 4 bytes** (one wordline)  
- Data packets (P1–P5) are transferred from the Wishbone bus to the **page buffer**  
- From the buffer, data is written into the corresponding rows and columns of the ReRAM crossbar  
- Each write cycle takes approximately **10 µs per page**, enabling fast updates while maintaining energy efficiency

---

**Source:** [Neuromorphic X1 – bmsemi.io](https://bmsemi.io/commercial-neuromorphic-x1.html)


////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////


<table>
  <tr>
    <td align="center"><img src="bm-lab-logo-white.jpg" alt="BM LABS Logo" width="200"/></td>
    <td align="center"><img src="chip_foundry_logo.png" alt="ChipFoundry Logo" width="200"/></td>
  </tr>
</table>

# Neuromorphic X2 – Analog In-Memory Compute IP Macro

Neuromorphic X2 is a **512 × 512 ReRAM compute-in-memory IP macro** combining **256 Kb non-volatile storage** with analog multiply–accumulate operations. It is controlled through a **32-bit Wishbone interface** for energy-efficient edge-AI acceleration.

---

## Key Features

- **512 × 512 ReRAM crossbar** with 256 Kb storage
- **SET, RESET, READ, and COMPUTE** operating modes
- FIFO-decoupled Wishbone command and response paths
- TDC-style read and compute-result output
- Runtime reconfiguration without a full reset
- Simulation-ready behavioral model and testbench
- Current behavioral RTL uses a **32 × 32 logical abstraction** of the X2 interface

---

## Data Flow

```text
Wishbone Write
      ↓
Input / Command FIFO
      ↓
Configuration or SET / RESET / READ / COMPUTE
      ↓
512 × 512 ReRAM Crossbar
      ↓
TDC Read or MAC Result
      ↓
Output / Response FIFO
      ↓
Wishbone Read
```

Compute-mode commands are written to the **input FIFO**. The crossbar operation is then executed, and the resulting MAC data is returned through the **output FIFO**.

---

## Behavioral Model

- Main model: `hdl/beh_model/Neuromorphic_X2_wb_beh.v`
- Testbench: `hdl/beh_model/tb_Neuromorphic_X2_wb_beh.v`
- Wishbone address: `0x3000_0004`
- The first three writes after reset configure the model.
- COMPUTE mode accepts three input packets and generates 32 queued result words.
- Wishbone reads complete only when response data is available.

### Command Modes

| Mode | Value | Operation |
|---|---:|---|
| RESET | `00` | Program reset state |
| READ | `01` | Read TDC result |
| COMPUTE | `10` | Perform MAC operation |
| SET | `11` | Program set state |

---

## Simulation

Run the supplied ModelSim/Questa script from the behavioral-model directory:

```bash
cd hdl/beh_model
vsim -do run.do
```
