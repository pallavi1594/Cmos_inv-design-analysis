
To write a clean and minimal **GitHub blog-style README** using your inverter project images without overwhelming the reader, follow this refined approach:

---

## 📈 MOSFET Characteristics


In this section, we begin with the analysis of MOSFET models provided in the Sky130 PDK. We'll focus on 1.8V transistors, specifically the nfet_01v8 and pfet_01v8 devices.

To visualize the basic characteristic curves of an NMOS transistor (Ids vs Vgs and Ids vs Vds), we set up the schematic in Xschem using:

nfet_01v8.sym from the sky130_fd_pr library

vsource.sym and code_shown.sym from the devices library

Once the schematic is saved:

Click Netlist

Then click Simulate

This launches NGSpice, which begins loading required libraries and models. After initialization, enter the following commands in the NGSpice terminal:

ngspice
Copy
Edit
display         ; Lists all simulation vectors
setplot         ; Shows available simulation plots
setplot tran1   ; (example) Select a plot
plot vds#branch ; Plots the current through the Vds branch
After executing the above, you should see a plot like the one below — especially if you performed a DC sweep on the VGS source for varying VDS values.



<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/blob/main/Images/nfet_for_vgs_vs_ids.png?raw=true" width="640">

<br><br>

From the Ids vs Vgs plot, it's evident that the threshold voltage (V<sub>th</sub>) lies between 600 mV and 700 mV. For further analysis and sizing decisions, we’ll use an approximate threshold value of 650 mV.

Similarly, by sweeping the VDS source across varying VGS values, we obtain the following curve:



<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/blob/main/Images/nfet_Ids_vs_Vgs.png?raw=true" width="640">

<br><br>
The above curves match expected NMOS characteristics. For deeper analysis, we select a specific curve for parameter extraction.

Next, we plot g<sub>m</sub> (transconductance) and g<sub>o</sub> (output conductance) using the same DC sweep. These values are key for calculating important device parameters.




<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_Ids_vs_Vds.png" width="640">

<br><br>

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_gds.png" width="640">

<br><br>
Since we’re designing an inverter, we’ll use the maximum available V<sub>DS</sub> = 1.8V.
Update the V<sub>DS</sub> source value to 1.8V, then hit Netlist → Simulate to run the analysis.




<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_go.png" width="640">

<br><br>

The plot shows that at V<sub>GS</sub> = 1.8V, the drain current I<sub>DS</sub> ≈ 320 µA.

To compute G<sub>m</sub> (transconductance), use the deriv() function in ngspice, which differentiates the output current with respect to the input voltage:

ini
Copy
Edit
Gm = dIds/dVgs
After plotting using deriv(), measure the value at V<sub>GS</sub> = 1.8V to obtain G<sub>m</sub>.

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_ids_vs_Vgs_Vds18.png" width="640">

<br><br>
Similaraly I did the same for Ids vs Vds and also used that to find rds

## ⚙️ Parameter Extraction

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_Gm_at_Vds018.png" width="640">

<br><br>

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_ids_vs_Vds_Vgs18.png" width="640">

<br><br>

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_rds_Vgs18.png" width="640">

<br><br>

## ➕ PMOS Exploration

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/pfet_test_ckt.png" width="640">

<br><br>

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/pfet_Ids_vs_Vds_for_Vsg018.png" width="640">

<br><br>

## 🧠 Inverter Behavior

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nmos_as_inverter.png" width="640">

<br><br>

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nmos_tran_inverter.png" width="640">

<br><br>

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/pmos_as_inverter.png" width="640">

<br><br>

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/pmos_tran_inverter.png" width="640">

<br><br>

## 💡 CMOS Inverter Design

<img src="https://user-images.githubusercontent.com/43693407/143431624-72bece76-3d5a-41fd-bca7-d21beaecd977.gif" width="640">

<br><br>

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/CMOS_inv_sch_and_sym.png" width="640">

<br><br>

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/cmos_inv_tb.png" width="640">

<br><br>

## 📉 Inverter Performance

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/cmos_inv_dc_anal.png" width="640">

<br><br>

<img src="https://user-images.githubusercontent.com/43693407/143764318-d0893545-f47c-44b8-a27c-8de8bc4f0759.jpg" width="640">

<br><br>

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/cmos_inv_vtc_150.png" width="640">

<br><br>

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/cmos_inv_vih_vil_plot.png" width="640">

<br><br>

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/cmos_inv_vih_vil_val.png" width="640">

<br><br>

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/cmos_inv_curr_plot.png" width="640">

<br><br>

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/cmos_inv_pdiss.png" width="640">

<br><br>

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/cmos_inv_vdd_variations.png" width="640">

<br><br>

---

✅ All images are scaled to `width="640"` and separated with `<br><br>` for clean spacing.
✅ Layout is now optimized to showcase **one image at a time**.
✅ You can now add captions or markdown text between them to build a compelling narrative.

Let me know when you're ready to insert descriptions, captions, or explanation blocks!


