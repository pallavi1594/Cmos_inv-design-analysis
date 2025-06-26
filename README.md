
## 📈 MOSFET Characteristics

In this section I start with our analysis of MOSFET models present in sky130 pdk. I would be using the 1.8v transistor models, but you can definitely use and experiment with other ones present there. below is the schematic I created in Xschem.
<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/blob/main/Images/nfet_for_vgs_vs_ids.png?raw=true" width="640">

The components used are:
nfet_01v8.sym - from xschem_sky130 library
vsource.sym - from xschem devices library
code_shown.sym - from xschem devices library

I used the above to plot the basic characteristic plots for an NMOS Transistor, That is Ids vs Vds and Ids vs Vgs. To do that, just save the above circuit with the above mentioned specifications and component placement. After this just hit Netlist then Simulate. ngspice would pop up and start doing the simulation based calculations. It will take time as all the libraries need to be called and attached to the simulation spice engine. Once that is done, you need to write a couple commands in the ngspice terminal:

Then you must see the plot below you, if you did a DC sweep on the VGS source for different values of VDS:
<br><br>


<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/blob/main/Images/nfet_Ids_vs_Vgs.png?raw=true" width="640">

<br><br>

This definitely shows us that the threshold value is between 600mV to 700mV and I think I will be using 650mV for my future calculations. Similarly, when I sweep VDS source for different values of VGS, I get the below plot:
<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_Ids_vs_Vds.png" width="640">

<br><br>
Now the above two definitely looks like what the characteristics curves should, but now we need to choose a particular curve that we would do further analysis on.

I also did plot gm and go(or ro) values for the above mosfet. This would be crucial as we can obtain a lot of parameters from these values. Both of these below are for the general dc sweep we did above.
<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_gds.png" width="640">

<br><br>

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_go.png" width="640">

<br><br>
Since I am making an inverter, let's choose the highest value avialable for the Vds, that is 1.8V. So to do that, we just change the value of Vds source to 1.8 and then hit netlist, then simulate to simulate the circuit.

<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_ids_vs_Vgs_Vds18.png" width="640">

<br><br>
This above plot also tells us the value of current at this value of Vgs which is around 320uA. Next step is to calculate the Gm, that is the transconductance parameter. To do that we simple type the commands as shown in the console in the left hand side. The deriv() function takes the derivative with respect to the independent variable present at the current simulation. From the definition of Gm we are aware that it is dIds/dVds. Hence, I did the same to plot the Gm of this nfet. After this I measured the value at 1.8V.


<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_Gm_at_Vds018.png" width="640">

<br><br>
Similaraly I did the same for Ids vs Vds and also used that to find rds
<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_ids_vs_Vds_Vgs18.png" width="640">

<br><br>


<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/nfet_rds_Vgs18.png" width="640">

<br><br>
Hence, we now have all our important values we needed. Same can be done for a PMOS. Motive is same, but expecially to extract the value of Aspect ratio for which the current is the same in both NMOS and PMOS. I have done some experimentation and found that at W/L of PMOS = 4 * (Aspect ratio of NMOS), the current value is pretty close. So, we found the NMOS had a current of 317 microamps while PMOS has the current of 322 microamps (both at |Vgs| = 1.8V). 


<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/pfet_test_ckt.png" width="640">

<br><br>


<img src="https://github.com/D-curs-D/Inverter-design-and-analysis-using-sky130pdk/raw/main/Images/pfet_Ids_vs_Vds_for_Vsg018.png" width="640">

<br><br>
The last one might be the most important one for an Inverter 
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


