# Week 4 – Day 2  
## Velocity Saturation & CMOS Inverter VTC

---

<details>
<summary><h2> 🌟 THEORY </h2> </summary>


### 🎯 Objective
Compare **drain current** behavior in **long-channel (1.2 µm)** and **short-channel (0.2 µm)** NMOS devices using SPICE simulation.

---

### ⚙️ SPICE Setup

```spice
M1 vdd n100 0 0 nmos W=1.8u L=1.2u
R1 in n1 55
Vdd vdd 0 2.5
Vin in 0 2.5
.lib "025um_model.mod" CMOS_MODELS
.dc Vdd 0 2.5 0.1 Vin 0 2.5 0.2
```
🔍 Key Concepts
🧩 1. Velocity Saturation

When the electric field in a short-channel MOSFET becomes high, carrier velocity stops increasing linearly and saturates.
```
Condition	Velocity Relation
Low Field (E < EC)	v = μn * E
High Field (E ≥ EC)	v ≈ vsat

Typical values (Si NMOS at 300K):

Mobility (μn) ≈ 450 cm²/V·s

Saturation Velocity (vsat) ≈ 10⁷ cm/s

Critical Field (EC = vsat / μn) ≈ 2×10⁴ V/cm
```
⚡ 2. Current Equations

Long-Channel MOSFET:


```

ID=0.5∗μn∗Cox∗(W/L)∗(VGS−VT)2∗(1+λVDS)

```
ID=0.5∗μn∗Cox∗(W/L)∗(VGS−VT)
2
∗(1+λVDS)

Short-Channel (Velocity-Saturated):
```


ID=μn∗Cox∗(W/L)∗(VGS−VT)∗VDSsatVDSsat=(vsat∗L)/μn


```
ID=μn∗Cox∗(W/L)∗(VGS−VT)∗VDSsatVDSsat=(vsat∗L)/μn

Unified Model:
```

ID=Kn∗VGT∗Vmin−0.5∗Kn∗Vmin

```
ID=Kn∗VGT∗Vmin−0.5∗Kn∗Vmin
2
∗(1+λVDS)

where:

Vmin = min(VGT, VDS, VDSsat)

VGT = VGS − VT

Kn = μnCox(W/L)

🧭 3. Comparison Between Long & Short Channels
Feature	Long Channel	Short Channel
```
ID–VDS	Quadratic	Linear
ID–VGS	∝ (VGS − VT)²	∝ (VGS − VT)
Velocity	v = μE	v ≈ vsat
Threshold	≈ 0.8 V	≈ 0.77 V

```
Dominant Effect	Mobility	Velocity Saturation, DIBL
⚙️ 4. MOSFET Operation Regions
Region	Condition	Behavior
```
Cutoff	VGS < VT	ID ≈ 0
Linear	VDS < (VGS − VT)	ID ∝ VDS
Saturation	VDS ≥ (VGS − VT)	Channel pinches off, ID ≈ constant
```
⚡ CMOS Inverter Analysis
🟣 PMOS Load Curve

Source → VDD

Drain → Output (Vout)

Gate → Input (Vin)

Relation: Vout = VDD + VDSP

As Vout increases → ID decreases → capacitor charging current reduces.

🔵 NMOS Load Curve

Source → GND

Drain → Vout

Gate → Vin

Relation: VGSN = Vin, VDSN = Vout

Direct ID vs Vout plots for different Vin values.

🔀 Combining NMOS & PMOS Curves

The intersection points of both load curves (IDN = IDP) give the operating points of the inverter.
Plotting all intersection points forms the VTC (Voltage Transfer Characteristic) curve.

📉 CMOS Inverter VTC Behavior (VDD = 2 V)
```
Vin (V)	Vout (V)	PMOS	NMOS	Operation
0	≈2	ON (Linear)	OFF	Output HIGH
0.5	1.5–2	ON	Saturation	Output starts to fall
1.0	0.5–1.5	Saturation	Saturation	High-gain region
1.5	0–0.5	Saturation	ON	Output LOW
2.0	0	OFF	ON	Fully LOW
🧠 Key Points

```

Vin = 0 → Output HIGH (VDD)

Vin = VDD → Output LOW (0 V)

Middle Region: both devices in saturation → High gain

Static Power: very low (only one device ON at a time)

⚡ Impact of Scaling

As device length ↓ → Electric field ↑

Velocity saturation dominates → ID–VGS becomes linear

Reduces gain and affects inverter slope

Modern BSIM SPICE models include this effect automatically.

🧪 LAB: Sky130 Simulation
```
Technology: Sky130 (Typical Corner)
W/L: 0.39 µm / 0.15 µm
Supply Voltage: 1.8 V
```
🔹 a) ID–VDS Sweep
ngspice day2_nfet_idvds_L015_W039.spice
plot -vdd#branch


Observation:

For small VDS → Quadratic behavior

For large VDS (>1 V) → Linear (velocity saturation visible)

Peak ID ≈ 196 µA @ VGS = 1.8 V

🔹 b) ID–VGS Sweep
ngspice day2_nfet_idvgs_L015_W039.spice
plot -vdd#branch


Observation:

ID rises linearly at high VGS

Confirms velocity saturation dominance

🔹 c) Threshold Voltage (VT) Extraction

Steps:

Plot ID vs VGS curve

Draw a tangent in the steep region

Extend tangent to X-axis → Intersection = VT

Result:
VT ≈ 0.77 V (slightly lower due to DIBL)

✅ Final Summary
```
Long-channel: ID ∝ (VGS − VT)²

Short-channel: ID ∝ (VGS − VT) → velocity saturation

CMOS Inverter: Built from NMOS + PMOS load curves

High Gain Region: Both in saturation

Velocity Saturation: Crucial in modern short-channel devices

Velocity saturation reduces current drive in scaled MOSFETs.

CMOS inverter VTC shows the balance between PMOS & NMOS.

Both digital (logic switching) and analog (gain region) operations depend on this balance.
