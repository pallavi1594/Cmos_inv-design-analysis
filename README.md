## 📈 MOSFET Characteristics

In this section, we begin analyzing the MOSFET models available in the Sky130 PDK. We’ll focus on the 1.8 V transistor models, though others are available for experimentation. Below is the schematic created in Xschem:

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/blob/main/Images/nfet_for_vgs_vs_ids.png?raw=true" width="640">

### 🧩 Components Used

* `nfet_01v8.sym` from the `xschem_sky130` library
* `vsource.sym` from the `xschem devices` library
* `code_shown.sym` from the `xschem devices` library

This schematic helps us generate two key NMOS plots:

* Ids vs Vds
* Ids vs Vgs

To simulate:

1. Save the schematic.
2. Click **Netlist** → **Simulate**.
3. Ngspice will open and process the simulation.
4. In the Ngspice console, use:

   * `display` to list all vectors
   * `setplot` to show all available plots
   * `setplot <plot_name>` (e.g., `setplot tran1`)
   * `plot <vector>` (e.g., `plot -vds#branch`)

When a DC sweep is performed on V<sub>GS</sub> for different V<sub>DS</sub> values, we get:

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/blob/main/Images/nfet_Ids_vs_Vgs.png?raw=true" width="640">

This plot shows the threshold voltage between 600 mV and 700 mV. We'll use 650 mV for future calculations. Sweeping V<sub>DS</sub> for different V<sub>GS</sub> values results in:

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_Ids_vs_Vds.png" width="640">

These are standard MOSFET characteristics. Next, we plot **g<sub>m</sub>** and **g<sub>o</sub>** (or **r<sub>o</sub>**) values:

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_gds.png" width="640">
<br><br>
<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_go.png" width="640">

For inverter operation, we fix V<sub>DS</sub> at 1.8 V:

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_ids_vs_Vgs_Vds18.png" width="640">

From this, I<sub>DS</sub> ≈ 320 µA at V<sub>GS</sub> = 1.8 V. To find g<sub>m</sub>, we use the `deriv()` function (dIds/dVgs):

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_Gm_at_Vds018.png" width="640">

Similarly, for I<sub>DS</sub> vs V<sub>DS</sub>:

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_ids_vs_Vds_Vgs18.png" width="640">

... and its corresponding r<sub>ds</sub> plot:

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_rds_Vgs18.png" width="640">

We now have all important NMOS parameters. A similar process applies for PMOS. The goal is to match the current through both devices. From simulations:

* NMOS I<sub>DS</sub> ≈ 317 µA
* PMOS I<sub>DS</sub> ≈ 322 µA (with W/L ≈ 4× NMOS)

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/pfet_test_ckt.png" width="640">
<br><br>
<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/pfet_Ids_vs_Vds_for_Vsg018.png" width="640">

---

## 🧠 2.2 Strong 0 and Weak 1 — NMOS as Inverter

When a square wave is applied to the NMOS input:

* LOW (0 V) → Output is HIGH (1.8 V)
* HIGH (1.8 V) → Output ≠ 0 V (not LOW)

Why? At 1.8 V, the NMOS is in linear region, acting like a resistor. So the output forms a **voltage divider**.

Hence, **NMOS passes a strong 0** but only a **weak 1**.

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nmos_as_inverter.png" width="640">
<br><br>
<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nmos_tran_inverter.png" width="640">

---

## 🧠 Weak 0 and Strong 1 — PMOS as Inverter

PMOS does the opposite:

* HIGH input → LOW output (strong)
* LOW input → HIGH output (weak due to divider)

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/pmos_as_inverter.png" width="640">
<br><br>
<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/pmos_tran_inverter.png" width="640">

Hence, **PMOS is a strong 1 but a weak 0**.

To solve this, we combine both transistors in a CMOS configuration.

---

## 💡 CMOS Inverter Design

By placing:

* PMOS between V<sub>DD</sub> and output
* NMOS between output and GND

...we get a complementary configuration. One turns ON while the other turns OFF. This enables:

* **Strong HIGH** when PMOS pulls up
* **Strong LOW** when NMOS pulls down

This is the basis of a **CMOS inverter**.

<img src="https://user-images.githubusercontent.com/43693407/143431624-72bece76-3d5a-41fd-bca7-d21beaecd977.gif" width="640">

CMOS circuits have:

* **Pull-Up Network** (PMOS)
* **Pull-Down Network** (NMOS)

This duality minimizes voltage division and ensures strong output swings.

---

## 🔍 3.2 CMOS Inverter Analysis (Pre-Layout)

An **inverter** performs a NOT operation: 1.8 V → 0 V and vice versa.

While the concept is simple, real-world inverters deal with:

* Propagation delay
* Noise margins
* Loading
* Power dissipation

These make it a great learning model — like a "Hello World!" for transistor design.

To begin:

* NMOS: W/L = 1 / 0.30 µm
* PMOS: W/L = 4× NMOS

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/CMOS_inv_sch_and_sym.png" width="640">

We use this schematic in a common testbench:

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/cmos_inv_tb.png" width="640">

---

## 📉 3.2.1 DC Analysis & Key Parameters

Using DC analysis, we generate the **Voltage Transfer Characteristic (VTC)**:

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/cmos_inv_dc_anal.png" width="640">

The VTC curve has three zones:

1. Output HIGH
2. Transition zone (\~0.75 V)
3. Output LOW

Technically, it includes five regions based on transistor modes.

<img src="https://user-images.githubusercontent.com/43693407/143764318-d0893545-f47c-44b8-a27c-8de8bc4f0759.jpg" width="640">

### 📏 Critical Parameters from VTC

* VOH = 1.8 V
* VOL = 0 V
* VIH & VIL determined by slope = -1
* V<sub>th</sub> ideally = V<sub>DD</sub>/2 = 0.9 V

Initial sizing didn’t achieve this. After tuning:

* NMOS: 1.05 / 0.15 µm
* PMOS: 2.1 / 0.15 µm

Updated result:

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/cmos_inv_vtc_150.png" width="640">

To extract VIL and VIH:

* At VIH: NMOS → saturation, PMOS → linear
* At VIL: NMOS → linear, PMOS → saturation
* Both occur where |dV<sub>out</sub>/dV<sub>in</sub>| = 1

Plot and measurement:

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/cmos_inv_vih_vil_plot.png" width="640">
<br><br>
<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/cmos_inv_vih_vil_val.png" width="640">

The commands used:

* Declared a helper function to visualize slope ≥ 1
* Set plot to `dc1`
* Used `.meas` for quantitative extraction

This completes our inverter DC analysis section!
