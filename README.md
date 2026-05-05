# Idealized 19.75% HALEU UCO Pebble-Bed (OpenMC)

OpenMC neutronics and depletion of an idealized, single 19.75% HALEU UCO TRISO pebble in HTR-PM-style geometry. Contains reflective boundaries to approximate an infinite pebble lattic. Cross sections are ENDF/B-VIII.0.

The pebble is a 3 cm radius sphere with a 0.5 cm graphite shell. The fuel zone is packed at 9% pf with 13 thousand TRISO particles. Two notebooks: one for fresh-fuel k∞, spectrum, and U-235 reaction-rate ratios, the other for a 1080 EFPD (around 3 year) burnup sweep.

## Results

| Quantity | Value |
|---|---|
| Fresh-fuel k∞ | 1.625 |
| Power per pebble | 595 W (250 MWth / 420k pebbles) |
| Burn duration | 1080 EFPD (36 x 30 day steps) |
| U-235 at EOC | 48% of initial |
| Pu-239 at EOC | 6% of initial U-235 |
| Pu-239 share of fissions, EOC | 15% |
| Discharge burnup | 340 MWd/kgU (infinite lattice) |

Determined k∞ of 1.625 at fresh is consistent with the published range for 19.75% HALUE UCO TRISO under cold infinite-lattice assumptions, and runs about 0.07 above reported X-energy Xe-100 fresh k∞ with 15.5% HALUE. This lines up with the physics behind higher enrichment concentration. A finite-core k-effective would be roughly 20% lower once leakage and reactivity holds are folded in.

## Figures

**Key Isotopes vs Burnup**
<img width="790" height="490" alt="UCOIsotopes3YearBurnup" src="https://github.com/user-attachments/assets/e464f089-7aa8-4178-8854-6ea6a97adac6" />

<sub>Figure 1: Key isotope tracking during a 1080 EFPD burn at 595 W per pebble. U-235 depletes to roughly 48% of initial, while Pu-239 increases from U-238 (µ,𝛾) capture and saturates at ~6% relative to initial U-235. Xe-135 reaches equilibrium within the first time step. By end of 1080 EFPD, ~15% of fission occur on bred Pu-239 rather than U-235, demonstrating partial-breeder behavior of thermal HALEU reactors<sub>
<br>
<br>


**Pebble k∞ Depletion**
<img width="1189" height="390" alt="UCOpebbleK-infVSTime Burnup" src="https://github.com/user-attachments/assets/1e8ee141-87dc-44cf-9ddb-4fa4f559a131" />

<sub>Figure 2: Pebble k∞ vs burnup for a 19.75% HALEU UCO TRISO pebble (HTR-PM geometry). Steep initial drop is from Xe-135 equilibrium. Linear decline is U-235 depletion. Projected discharge burnup ~ 340 MWd/kgU (infinite-lattice approx., real-core value of ~160 MWd/kgU after accounting for leakage and burnable poisions).<sub>
<br>
<br>

**Pebble Core Map**
<img width="400" height="400" alt="CrossUCO" src="https://github.com/user-attachments/assets/340a7de1-1aa5-41c9-8a81-81976378f08e" />

<sub>Figure 3: Geom cross-section of HALEU UCO Pebble bed with 9% fill. Reflective BC on the cube around the pebble, forming an infinite pebble lattice. The colors are trivial, but dots represent the TRISO particles. <sub>

## Modeling

| | |
| Fuel | 19.75% HALEU UCO (~80/20 UO₂/UC₂), 10.9 g/cm³ |
| TRISO kernel / buffer / IPyC / SiC / OPyC outer radii | 215.5 / 312.5 / 352.5 / 387.5 / 427.5 µm |
| Pebble outer radius | 3.0 cm |
| Fuel-zone radius | 2.5 cm (0.5 cm graphite shell) |
| Packing fraction | 9% |
| BCs | reflective cube around the pebble |
| Cross sections | ENDF/B-VIII.0 |
| S(α,β) | `c_Graphite` on buffer / PyC / matrix |
| Fresh-fuel run | 100 batches × 2,000 particles, 20 inactive |
| Depletion run | 50 batches × 2,000 particles, 10 inactive, predictor, 30-day steps |
| Depletion chain | `chain_endfb80_pwr.xml` |

## Limitations

This is a screening-level infinite-lattice calculation, not a real safety
analysis. The big simplifications:

Cold temperature. Everything is at ~293 K. Going to operating temperature
(fuel ~1200 K, moderator ~900 K) would knock several thousand pcm off k∞ via
Doppler broadening of U-238 resonances. Fuel temperature coefficient is the
main passive-safety story for HTGRs, so leaving it out is the biggest gap
here.

Reflective BCs. Infinite lattice means no leakage. A finite core with
reflectors, control rods, and burnable absorbers ends up with noticeably
lower k_eff and a different flux shape.

Single pebble, single pass. Real HTR-PM cycles each pebble through the core
~10 times. This calculation runs one pebble straight through to EOC.

PWR depletion chain. `chain_endfb80_pwr.xml` is built for LWR spectra. An
HTGR-specific chain would do a better job on graphite-moderated transmutation
paths, especially for minor actinides.

Constant power. 595 W/pebble for the whole burn, no online refueling, no
power redistribution.

Most useful next step is a fuel-temperature sweep (300 / 600 / 900 / 1200 K)
to extract the temperature reactivity coefficient, since that's what's
missing from the safety story above.


## Layout

```
.
├── README.md
├── LICENSE
├── requirements.txt
├── pebble_model.py            # geometry + materials, shared by both notebooks
├── ModelingPebbleBed.ipynb    # fresh-fuel k_inf, spectrum, rate ratios
├── PebbleBedDepletion.ipynb   # 1080 EFPD burnup
└── figures/
```

## Running it

Download OpenMC and Conda beforehand. OpenMC has a compiled core, so conda is easier than pip:

```bash
conda create -n openmc -c conda-forge openmc python=3.11
conda activate openmc
pip install -r requirements.txt
jupyter lab
```

Cross sections and depletion chain (the host the OpenMC team uses):

- ENDF/B-VIII.0 library (~10 GB): https://anl.box.com/shared/static/uhbxlrx7hvxqw27psymfbhi7bx7s6u6a.xz
- ENDF/B-VIII.0 PWR depletion chain (27.5 MB): https://anl.box.com/shared/static/nyezmyuofd4eqt6wzd626lqth7wvpprr.xml

Set the paths in the first cell of either notebook:

```python
openmc.config['cross_sections'] = '~/lib80x_hdf5/cross_sections.xml'
openmc.config['chain_file']     = '~/chain_endfb80_pwr.xml'
```

## References

[1] Z. Zhang et al., "Current status and technical description of Chinese
2 × 250 MWth HTR-PM demonstration plant," Nucl. Eng. Des. 239 (2009) 1212.

[2] E.J. Mulder, W.A. Boyes, "Neutronics characteristics of a 165 MWth Xe-100
reactor," Nucl. Eng. Des. 357 (2020) 110415.

[3] P.A. Demkowicz, B. Liu, J.D. Hunn, "Coated particle fuel: historical
perspectives and current progress," J. Nucl. Mater. 515 (2019) 434.

[4] P.K. Romano et al., "OpenMC: A state-of-the-art Monte Carlo code for
research and development," Ann. Nucl. Energy 82 (2015) 90.

[5] IAEA-TECDOC-1694, "Evaluation of High Temperature Gas Cooled Reactor
Performance" (2013).


