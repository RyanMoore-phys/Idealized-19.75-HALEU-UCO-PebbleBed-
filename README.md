<h1> Idealized-19.75-HALEU-UCO-PebbleBed- </h1>
Using OpenMC, I have modeled a 19.75% enriched HALEU UCO HTGR Pebble with roughly 13 thousand TRISO pellets. Using idealized conditions (reflective boundary BC's &amp; cold temperature), this project determines Fresh-fuel k-inf, spectrum, and reaction rate ratios. The project also includes a 3 Year Burnup curve and Isotope Inventory 


Fresh-fuel k∞ of 1.625 is consistent with the published range for 19.75% HALEU UCO TRISO fuel under infinite-lattice, cold, clean assumptions. The value is ~0.07 higher than reported X-energy Xe-100 fresh k∞ (15.5% HALEU), which is consistent with the ~25% higher fissile loading. Finite-core values would be lower by roughly 20% after including factors such as leakage and operational reactivity holds.
<br>
<br>


<h2>Key Isotopes vs Burnup</h2>
<img width="790" height="490" alt="UCOIsotopes3YearBurnup" src="https://github.com/user-attachments/assets/e464f089-7aa8-4178-8854-6ea6a97adac6" />

<sub>Figure 1: Key isotope tracking during a 1080 EFPD burn at 595 W per pebble. U-235 depletes to roughly 48% of initial, while Pu-239 increases from U-238 (µ,𝛾) capture and saturates at ~6% relative to initial U-235. Xe-135 reaches equilibrium within the first time step. By end of 1080 EFPD, ~15% of fission occur on bred Pu-239 rather than U-235, demonstrating partial-breeder behavior of thermal HALEU reactors<sub>
<br>
<br>


<h2>Pebble k∞ Depletion</h2>
<img width="1189" height="390" alt="UCOpebbleK-infVSTime Burnup" src="https://github.com/user-attachments/assets/1e8ee141-87dc-44cf-9ddb-4fa4f559a131" />

<sub>Figure 2: Pebble k∞ vs burnup for a 19.75% HALEU UCO TRISO pebble (HTR-PM geometry). Steep initial drop is from Xe-135 equilibrium. Linear decline is U-235 depletion. Projected discharge burnup ~ 340 MWd/kgU (infinite-lattice approx., real-core value of ~160 MWd/kgU after accounting for leakage and burnable poisions).<sub>
<br>
<br>

<h2>Pebble Core Map</h2>
<img width="400" height="400" alt="CrossUCO" src="https://github.com/user-attachments/assets/340a7de1-1aa5-41c9-8a81-81976378f08e" />

<sub>Figure 3: Geom cross-section of HALEU UCO Pebble bed with 9% fill. Reflective BC on the cube around the pebble, forming an infinite pebble lattice. The colors are trivial, but dots represent the TRISO particles. <sub>

