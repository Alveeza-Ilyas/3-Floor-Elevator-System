# 3-Floor-Elevator-System

An interactive digital logic simulation of a **3-floor elevator control system** designed and built within **Logisim (v2.7.1)**. The system manages passenger requests from both inside the elevator cabin and outside on individual floors, tracking movement state, door mechanisms, and real-time floor telemetry.

## Circuit Architecture Overview

The project is modularized into three main subcircuits, prioritizing clean separation between the state controller, input registers, and display driver logic:

### 1. `main` (Top-Level Circuit)

* **Purpose:** Houses the physical layout of the system, combining user interfaces, memory elements, and output indicators.
* **Memory & Registers:** Utilizes **D Flip-Flops** as localized state-holding memory to latch "Go to Floor" requests (internal cabin buttons) and "Call to Floor" requests (external hallway buttons).
* **System Indicators:**
* `Door Open` / `Door Closed` LEDs to reflect structural safety status.
* `Up` / `Down` / `Ready` / `Busy` / `Usable` LEDs indicating operational motor telemetry.


* **Decoders:** Extracts multi-bit state information down to single-line signals to route data to proper display registers.

### 2. `Display`

* **Purpose:** Acts as a specialized **7-Segment Display Driver**.
* **Logic:** Employs optimized custom combinational logic (AND/OR/NOT arrays) to decode binary floor positions ($00$, $01$, or $10$) into the correct active-high signals ($a$ through $g$) required to cleanly render numbers `0`, `1`, and `2` on the display panel.

### 3. `Elevator`

* **Purpose:** The central Finite State Machine (FSM) core.
* **Logic:** Processes current floor registers against pending destination memory blocks. It manages directional prioritization, clock-synchronized transitions, and structural safety interlocks (such as locking down movement commands while the door state is set to open).

---

## System Input & Output Specifications

### User Interface Controls (Inputs)

| Component Label | Type | Location | Functional Description |
| --- | --- | --- | --- |
| `CALL to 2nd` / `1st` / `floor` | Button | Outside Cabin | Simulates a passenger waiting out in the hallway on that floor. |
| `Go 2nd` / `GO 1st` / `GO floor` | Button | Inside Cabin | Simulates the internal floor selection panel for a passenger on board. |
| `Clock` | Oscillator | System Base | Provides the execution pulse train driving sequential flip-flop transitions. |

### Diagnostic Metrics (Outputs)

* **7-Segment Display:** Visual indicator of the elevator's real-time floor position (`0`, `1`, or `2`).
* **Operational State LEDs:**
* 🟢 **Usable / Ready:** System idle, waiting for commands.
* 🟡 **Busy:** Cabin currently in transit.
* 🔴 **Up / Down:** Motor direction vectors.


* **Safety Status LEDs:** Dynamic visibility showing whether the cabin doors are currently open or closed during a delivery phase.

---

## How to Load and Run the Simulation

1. Download **Logisim v2.7.1** (or a fully compatible fork like Logisim-Evolution).
2. Save your circuit file with a `.circ` extension (e.g., `elevator_control.circ`).
3. Open Logisim, click **File ➔ Open**, and select your saved file.
4. Enable the simulation clock:
* Go to **Simulate** in the top toolbar.
* Ensure **Simulation Enabled** is checked (`Ctrl + E`).
* Select **Ticks Enabled** (`Ctrl + K`) to let the system cycle automatically, or adjust the **Tick Frequency** to see state changes step-by-step.


5. Use the **Poke Tool** (hand icon) to press call/destination buttons and observe how the system sets registers, updates direction flags, routes cabin movement, and cycles the doors!
