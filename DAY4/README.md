# 🌙 Week 4 – Day 4  
## CMOS Inverter Noise Margin – Concept, Behavior & Lab Evaluation

---

<details>
<summary><h2> 🌟 THEORY </h2> </summary>

## 🔹 What is Noise Margin?

- Digital gates (including CMOS inverters) must tolerate voltage variations from **glitches, crosstalk, or supply noise**.  
- **Noise margin** quantifies the tolerance without logic failure.

**VTC Analysis:**

- X-axis: Vin, Y-axis: Vout  
- Ideal inverter: vertical transition at VDD/2  
- Practical inverter: gradual slope in switching region  

**Voltage thresholds:**

| Symbol | Meaning |
|--------|--------|
| VOH | Maximum output high ≈ VDD |
| VOL | Minimum output low ≈ 0V |
| VIL | Max input considered logic 0 |
| VIH | Min input considered logic 1 |

---

## 🔹 Logical Interpretation

- Vin: 0 → VIL → Output stays high (VOH)  
- Vin: VIH → VDD → Output goes low (VOL)  
- Vin: VIL → VIH → Undefined region → logic unstable if noise occurs  

---

## 🔹 Noise Margin Equations


- **Noise Margin Low (NML):**  
```math
NML = VIL - VOL
```
- **Noise Margin High (NMH):**  
```math
NML = VOH - VIH
```
PMOS Width Impact on Noise Margin
PMOS Size vs NMOS	Effect on NMH	Effect on NML
Equal sizing	Balanced	Balanced
   2× PMOS	NMH ↑	NML ~ same
   3× PMOS	NMH ↑ more	NML ~ same
   4× PMOS	NMH ↑ slightly	NML ↓ small
   5× PMOS	NMH peaks	NML small drop

Takeaways:

PMOS determines how strongly logic ‘1’ is preserved

Larger PMOS → better NMH

Excessive PMOS → slight NML degradation

Insight: CMOS inverters maintain robust noise margins despite sizing variations.

✅ Key Understanding

Noise robustness = NMH & NML

Derived from DC transfer curve (VTC)

PMOS sizing directly affects margins

Lab results confirm theory

CMOS logic stays stable even with sizing variation

</details> <details> <summary><h2> 🌟 LAB </h2> </summary>
🔹 Lab Validation (Ngspice)

Setup:

PDK: Sky130

Simulation: DC sweep Vin = 0 → 1.8V, step 0.01

Plot: Vout vs Vin

Threshold identification: Points where slope ≈ −1



Measured Values:
```
Parameter	Value (V)
VOH	1.75
VOL	0.084
VIL	0.737
VIH	0.996
```
Noise Margins:

```
NMH=VOH−VIH≈1.75−0.996=0.754VNML=VIL−VOL≈0.737−0.084=0.653V
```
Inference:

NMH > NML → inverter preserves logic ‘1’ slightly better

Values within safe range → CMOS inverter robust to noise
