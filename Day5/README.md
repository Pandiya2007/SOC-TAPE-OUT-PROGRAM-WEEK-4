# 🌙 Week 4 – Day 5  
## CMOS Power Supply and Device Variation Robustness Evaluation

---

<details>
<summary><h2> 🌟 THEORY </h2> </summary>

## 🔹 CMOS Inverter Robustness — Power Supply Variation

*(Static behavior evaluation under varying supply voltages)*

### ⚙️ Simulation Setup

| Parameter | Description |
| --- | --- |
| PMOS Width | 0.975 µm |
| NMOS Width | 0.375 µm |
| Technology Node | 250 nm |
| Initial VDD | 2.5 V |
| Sweep Range | 2.5 V → 0.5 V (step = 0.5 V) |
| Load Capacitance | Same as previous inverter experiments |

- PMOS is made wider to balance NMOS and maintain symmetric transfer.

### 🔹 SPICE Control Script (TCL-style)

```spice
let power_supply = 2.5
let power_supply_variation = 0

dowhile (power_supply_variation < 5)
  alter vdd = power_supply
  dc vin 0 power_supply 0.01
  plot v(out) vs v(in)
  let power_supply = power_supply - 0.5
  let power_supply_variation = power_supply_variation + 1
end
```
Advantages of Lowering VDD

Improved Gain:
```
Av​=ΔVin​ΔVout​​
```

```
	VDD = 2.5 V → Gain ≈ 7.38

  VDD = 0.5 V → Gain ≈ 11.5 (+56%)
```

Reduced Energy Consumption:

```
	E=1/2CVDD2​​
```

Lower VDD → ~96% energy reduction
Ideal for mobile, wearable, IoT devices

❌ Disadvantages of Lowering VDD

Performance degradation: Rise/fall delays increase significantly

Reduced robustness: Noise margin and device matching worsen

At 0.5 V, load capacitor may not fully charge/discharge
```
Aspect	High VDD (2.5 V)	Low VDD (0.5 V)
Gain	7.38	11.5 ↑
Energy	High	96% ↓
Delay	Fast	Slow ×3–4
```
Reliability	Strong	Marginal
🔹 CMOS Inverter Robustness — Device Variation
Sources of Variation

Etching Process: Defines gate/channel/diffusion geometry → W/L variations → ID changes

Oxide Thickness (tox): Affects Cox → affects ID → changes switching threshold and delay

SPICE Simulation Concept

PMOS/NMOS widths swept to extremes

Strong PMOS / Weak NMOS → fast rising edge

Weak PMOS / Strong NMOS → fast falling edge

Example SPICE Loop
```
.param temp=27
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=1 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15

Cload out 0 50fF
Vdd vdd 0 1.8V
Vin in 0 1.8V

.control
let powersupply = 1.8
alter Vdd = powersupply
let voltagesupplyvariation = 0
dowhile voltagesupplyvariation < 6
  dc Vin 0 1.8 0.01
  let powersupply = powersupply - 0.2
  alter Vdd = powersupply
  let voltagesupplyvariation = voltagesupplyvariation + 1
end
plot dc1.out vs in dc2.out vs in dc3.out vs in dc4.out vs in dc5.out vs in dc6.out vs in xlabel "Vin(V)" ylabel "Vout(V)" title "Inverter VTC vs supply"
.endc
.end
```
🔹 Key Takeaways
Parameter	Observation	Impact
```
Switching Threshold (VM)	0.2 → 1.4 V shift	Acceptable
Noise Margin	0.4 V (High) → 0.3 V (Low)	Within robust range
VTC Shape	Sharp transition maintained	Logic integrity preserved
Functionality	Unaffected	Inverter behaves as ideal logic gate
```
CMOS inverters are intrinsically robust to etching, oxide, and device variation.

</details> <details> <summary><h2> 🌟 LAB </h2> </summary>
🔹 Sky130 Supply Variation Lab

Objective: Verify VDD variation trends using NGSPICE + Sky130 PDK

Setup: Wp/Wn = 2.77, VDD = 1.8 V → decreased 0.2 V per step (6 iterations), Vin 0 → 1.8 V

Procedure Example:

nano day5_inv_supplyvariation_Wp1_Wn036.spice
ngspice day5_inv_supplyvariation_Wp1_Wn036.spice


Observe VTC curves for 1.8, 1.6, 1.4, 1.2, 1.0, 0.8 V

Compute gain: Av = ΔVout / ΔVin
```
VDD (V)	Gain
1.8	7.72
1.6	8.9
1.4	9.3
1.2	9.5
1.0	~7.0
0.8	<7.0
```
Gain rises as VDD decreases until transistor drive weakens

Optimum operation ~1.0–1.2 V
🔹 Sky130 Device Variation Lab

Objective: Verify inverter robustness under Wp/Wn extremes
```

Setup: Wp = 7 µm, Wn = 0.42 µm, L = 0.15 µm, VDD = 1.8 V, Vin sweep 0 → 1.8 V

ngspice inv_device_variation.spice
plot v(out) vs v(in)

```
Observation:

Strong PFET → output holds at VDD longer

Switching threshold ≈ 0.988 V vs ideal 0.9 V → Δ ≈ 80 mV (<5% of VDD)

Conclusion: CMOS inverter remains highly robust even with large device mismatch.

Aspect	Observation	Conclusion
```
Etching Process	Alters W/L	Impacts delay & ID linearly
Oxide Thickness	Alters Cox	Minor impact on ID & threshold
Device Size Sweep	VTC shifts	Logic intact
Sky130 Lab	ΔVM ≈ 80 mV	Very robust
```
</details> 
