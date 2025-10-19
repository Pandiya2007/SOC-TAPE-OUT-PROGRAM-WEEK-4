# RISC-V SoC Tapeout Program — Week 0

<details>
<summary><h2> 🌟 THEORY </h2> </summary>




# 1. Create the SPICE netlist for Inverter Transient Analysis.
# This file measures the delay (propagation time) of an inverter cell.
cat << EOF > day3_inv_tran_delay.spice
* Inverter Transient Simulation for Delay Measurement
* Measures Tplh and Tphl (propagation delay) for one load case.

.include ../sky130_fd_pr/models/sky130.lib.spice TT

* *** 1. Cell Definition: Inverter (M_p and M_n) ***
* Drain Gate Source Body Model
* PMOS (Wp=0.84u, L=0.15u) - Pulled up to VDD
Mp out in vdd vdd sky130_fd_pr__pfet_01v8 L=0.15u W=0.84u
* NMOS (Wn=0.36u, L=0.15u) - Pulled down to GND
Mn out in 0 0 sky130_fd_pr__nfet_01v8 L=0.15u W=0.36u

* *** 2. Power and Stimulus ***
Vdd vdd 0 DC 1.8V
* Input Pulse (Vin): 0V to 1.8V, 0 delay, 1ns rise/fall, 100ns pulse width, 200ns period
Vin in 0 PULSE(0 1.8 0 1n 1n 100n 200n)

* *** 3. Output Load ***
* CL (Load Capacitance) - This is the "Output Load" parameter from the delay table.
Cl out 0 50fF

* *** 4. Analysis: Transient Simulation ***
* Run simulation for 400ns with a step of 0.1ns
.tran 0.1n 400n

* *** 5. Output ***
.print tran V(in) V(out)

.end
EOF

# 2. Run the simulation in batch mode.
ngspice -b day3_inv_tran_delay.spice

# 3. Instructions for plotting the result.
echo " "
echo "✅ SPICE file created and Inverter delay simulation run."
echo "➡️ To visualize the Input/Output waveforms for delay analysis, start an interactive ngspice session and use the following commands:"
echo "   ngspice"
echo "   source day3_inv_tran_delay.raw"
echo "   plot V(in) V(out)"
echo "   quit"

## ⚡ NMOS: Structure → Terminals → Threshold Idea

### 1️⃣ What is an NMOS?



- **N-channel MOSFET** built on a **p-type substrate**.
- **Four terminals:** **Gate (G)**, **Drain (D)**, **Source (S)**, **Body (B)**.
- **Body Terminal Note:** In logic, **B is usually tied to ground (0 V)**. $V_{SB}$ shifts $V_T$ (**body effect**).

---

### 2️⃣ Physical Structure (Cross-section)



- **Isolation regions** separate neighboring devices.
- **n+ diffusion regions** form the **Source** and **Drain**.
- **Gate Stack:** Consists of **poly/metal gate** and thin **gate oxide** over the substrate.
- **MOS Capacitor:** Gate–oxide–substrate structure controls channel formation.

> 🧭 **SPICE Relevance:** SPICE models use the physical geometry ($W$, $L$, $t_{ox}$) to predict $I_D–V$ behavior and delay.

---

### 3️⃣ Role of Each Terminal

- **Gate (G):** Voltage-controlled “knob”; sets surface charge via the MOS capacitor.
- **Source (S) & Drain (D):** The n+ regions between which current flows when the channel forms.
- **Body (B):** Substrate reference. **$V_{SB}$** shifts the **threshold voltage ($V_T$)** via the **body effect**. (Typically $V_B = 0 \text{ V}$).

---
