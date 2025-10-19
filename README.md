# SOC-TAPE-OUT-PROGRAM-WEEK-4

# Week 4 – CMOS Inverter Fundamentals & Robustness ✅


[![NGSPICE](https://img.shields.io/badge/NGSPICE-Experiment-blue)](#)
[![VLSI](https://img.shields.io/badge/VLSI-Lab-orange)](#)
[![Sky130](https://img.shields.io/badge/PDK-Sky130-green)](#)

---
<img width="1248" height="832" alt="Gemini_Generated_Image_ytekvxytekvxytek" src="https://github.com/user-attachments/assets/7ff88717-caad-45da-90c4-75d050e986ba" />


## 📅 Week 4 Timeline

| Day | Topic | Status |
| --- | ----- | ------ |
| Day 1 | CMOS Inverter Design & VTC | ✅ Completed |
| Day 2 | Delay & Load Capacitance | ✅ Completed |
| Day 3 | Switching Characteristics & Propagation Delay | ✅ Completed |
| Day 4 | Noise Margin Evaluation | ✅ Completed |
| Day 5 | Power Supply & Device Variation Robustness | ✅ Completed |

> You can visualize this as a mini progress tracker for Week 4 labs.  

---

## 🎯 Lab Objectives
- Understand CMOS inverter operation, VTC, and switching characteristics  
- Analyze propagation delay and effect of load capacitance  
- Evaluate noise margin and inverter robustness  
- Study power supply variation effects  
- Examine device variation robustness  

---

## 📋 Day-wise Summary

### **Day 1 – CMOS Inverter Design & DC Characteristics**  
**Objective:** Learn CMOS inverter operation and VTC.  
**Key Learnings:**  
- CMOS inverter = PMOS + NMOS complementary.  
- Important voltages: `VOH`, `VOL`, `VIH`, `VIL`, `VM`.  
- VTC shows sharp logic transition; sizing affects switching threshold.  
**NGSPICE Script:** [`day1_inv_design.spice`](./scripts/day1_inv_design.spice)  
**Example Plot:**  
  

---

### **Day 2 – Delay & Load Capacitance**  
**Objective:** Analyze propagation delay (`tpHL`, `tpLH`) and load capacitance effect.  
**Key Learnings:**  
- Delay ∝ Load capacitance (RC effect).  
- Rise/fall times depend on MOSFET drive strength (`W/L`).  
- Larger load → slower transitions.  
**NGSPICE Script:** [`day2_delay_load.spice`](./scripts/day2_delay_load.spice)  
**Example Plot:**  


---

### **Day 3 – Switching Characteristics & Propagation Delay**  
**Objective:** Measure inverter switching time under step input.  
**Key Learnings:**  
- Propagation delay: `tp = (tpHL + tpLH)/2`.  
- Balanced PMOS/NMOS sizing → symmetric rise/fall delays.  
- Example: `tpHL ≈ 66 ps`, `tpLH ≈ 70 ps`.  
**NGSPICE Script:** [`day3_switching.spice`](./scripts/day3_switching.spice)  
**Example Plot:**  


---

### **Day 4 – Noise Margin Evaluation**  
**Objective:** Evaluate CMOS inverter robustness to input noise.  
**Key Learnings:**  
- Noise Margin Low (`NML`) = `VIL − VOL`.  
- Noise Margin High (`NMH`) = `VOH − VIH`.  
- PMOS sizing improves `NMH`.  
- Example: `NML ≈ 0.65 V`, `NMH ≈ 0.75 V`.  
- CMOS maintains stable logic despite noise.  
**NGSPICE Script:** [`day4_noise_margin.spice`](./scripts/day4_noise_margin.spice)  
**Example Plot:**  

---

### **Day 5 – Power Supply & Device Variation Robustness**  
**Objective:** Analyze inverter behavior under `VDD` scaling & device variation.  
**Key Learnings:**  
- Lower `VDD` → reduced energy (`E ∝ VDD²`), higher gain until transistor too weak.  
- Delay increases at low `VDD`; optimum operation ~1–1.2 V.  
- Device variations (W/L, `t_ox`) shift `VM` slightly (~80 mV), logic remains intact.  
- CMOS inverters are robust to process and supply variations.  
**NGSPICE Scripts:**  
- [`day5_supply_variation.spice`](./scripts/day5_supply_variation.spice)  
- [`day5_device_variation.spice`](./scripts/day5_device_variation.spice)  
**Example Plot:**  

---

## 🏁 Week 4 – Key Takeaways
1. CMOS inverters have predictable `VOH` / `VOL` / `VM` and sharp transitions.  
2. Delay increases with load, decreases with stronger transistors.  
3. Noise margins quantify robustness against glitches.  
4. Lowering `VDD` reduces energy but increases delay; optimal operation exists.  
5. CMOS logic is intrinsically robust to fabrication and device variations.  

---



