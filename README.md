# Conversion of CO₂ into Methanol via Innovative Photocatalytic and Electrocatalytic Processes

**Principal Investigator:** Prof. Catherine Stampfl (The University of Sydney, Australia)

**Collaborators:**
- Dr. Marco Fronzi — School of Physics, The University of Sydney, Australia
- Prof. Paolo Mele — College of Engineering, Shibaura Institute of Technology, Japan
- Dr. Fabio Lisi — Graduate School of Science, The University of Tokyo, Japan
- Dr. Shivani Sathish — Atierra Ltd., OIST Innovation Incubator, Okinawa, Japan

**Funding:** Australia–Japan Innovation Foundation (AJIF), 2024–2026

**Corresponding author:** marco.fronzi@sydney.edu.au

---

## 1. Project overview

This repository contains the supporting material for the AJIF-funded project *"Conversion of CO₂ into Methanol via an Innovative Photocatalytic and Electrocatalytic Process for Sustainable Energy Applications."* The project combines **first-principles and machine-learning computational screening** with **experimental thin-film synthesis and characterisation** of two-dimensional (2D) hybrid materials designed to catalyse the six-electron reduction of CO₂ to methanol under sustainable conditions.

The overarching goal is to link theoretical electronic-structure descriptors (d-band centre, *CO binding energy, band-edge alignment) to experimentally verifiable optical, structural, and catalytic performance, and to identify carbon-supported transition-metal-oxide composites that occupy useful regions of the Hammer–Nørskov volcano for CO₂RR.

---

## 2. Computational study

### 2.1 Scope

Systematic screening of **94 two-dimensional bilayer heterostructures** composed of graphene paired with transition-metal oxides (CuO, AgO, WO₃) in different terminations and registries. Two systems — graphene/CuO and graphene/AgO — were taken forward for full CO₂RR mechanistic analysis.

For each system we computed:
- Equilibrium lattice constant by minimising the in-plane elastic strain energy
- 2D elastic constants of graphene, CuO, AgO
- Work function and vacuum-level alignment
- Band-edge positions vs NHE (Butler–Ginley empirical construction)
- Adsorption energies of the seven CHE intermediates: \*CO₂, \*COOH, \*CO, \*CHO, \*CH₂O, \*CH₃O, \*CH₃OH
- Computational hydrogen electrode (CHE) free-energy profiles in both 6-step (Peterson) and 7-step conventions
- Limiting potential *U*<sub>L</sub> and potential-determining step (PDS)
- Marcus PCET barriers along the cascade (λ ∈ [0.6, 0.9] eV uncertainty propagated)
- Volcano placement via Gaussian process regression through 12 literature metals
- Gärtner depletion-layer photocurrent under AM1.5G illumination
- \*COOH vs \*OCHO branch selectivity
- d-band centre analysis and BEP corrections quantifying the substrate-induced shift

### 2.2 Software stack

| Layer | Package | Notes |
|---|---|---|
| Plane-wave DFT | VASP 6.x | PBE-D3, PAW, used for BEP-correction reference calculations |
| Machine-learning potentials | MACE-MP-0 (medium, 2.5 M params) | Equivariant ACE architecture |
| | CHGNet (MP-trained) | Charge-informed graph network |
| Optimisation | ASE + FIRE / BFGS | f<sub>max</sub> ≤ 0.05 eV/Å chemisorbed |
| Reference structures | Materials Project API | bulk oxides and metal references |
| Volcano fitting | scikit-learn GPR | Matérn-2.5 + White kernel |
| Analysis & plotting | NumPy, pandas, Matplotlib | Python 3.10 |
| HPC | NCI Gadi, Pawsey Setonix, Sydney Artemis | project allocations jm11 / pawsey |

### 2.3 Key methodological choices

- **Reference energies** are constructed stoichiometrically from MACE-MP-0 gas-phase energies of CO₂, H₂, H₂O. The known MLIP water–gas-shift error (~0.4 eV) is corrected by a uniform shift δ<sub>H₂O</sub> = +0.407 eV before reference construction.
- **BEP corrections** for the substrate-induced d-band shift are obtained from explicit DFT/PBE-D3 calculations on the full bilayer vs free-standing oxide and applied additively: Δ<sub>CuO</sub> = +0.515 eV, Δ<sub>AgO</sub> = +0.402 eV.
- **Molecular correction** Δ<sub>mol</sub> = +0.414 eV is applied uniformly to gas-phase CH₃OH(g) to enforce the experimental ΔG<sub>total</sub> = −0.016 eV.
- **Multi-MLIP cross-validation** is done as a routine reproducibility check: every system is screened with both MACE-MP-0 and CHGNet, and architecture-dependent disagreements (e.g., spontaneous \*COOH dissociation in MACE but not in CHGNet) are reported as diagnostics rather than averaged out.

### 2.4 Headline computational results (Gr/CuO and Gr/AgO)

| Quantity | Gr/CuO | Gr/AgO |
|---|---|---|
| Cell *a*, *b* (Å) | 12.331, 7.398 | 12.331, 7.398 |
| Composition | C₃₀Cu₁₆O₁₆ | C₃₀Ag₁₆O₁₆ |
| Mean M–O bond (Å) | 1.898 | 2.065 |
| Active-site CN | 3.00 | 2.62 |
| ΔE<sub>ads</sub>(\*CO) (eV) | +0.82 | +0.61 |
| *U*<sub>L</sub> (V vs SHE), 7-step | −1.10 | −1.29 |
| *U*<sub>L</sub> (V vs SHE), 6-step | −2.22 | −2.24 |
| Marcus *E*<sub>a</sub> at PDS (eV) | 2.93 | 2.98 |
| *E*<sub>CB</sub> (V vs NHE) | +0.11 | +0.44 |
| *E*<sub>g</sub> (eV) | 2.40 (DRS/Tauc, this work) | 1.70 (Waterhouse 2001) |
| ΔΔG(\*OCHO − \*COOH) (eV) | +0.116 | +0.037 |

**Conclusion:** The canonical \*CO hydrogenation cascade to methanol is thermodynamically unfavourable on both heterostructures. *U*<sub>L</sub> sits 0.7–1.9 V more reducing than the methanol thermodynamic threshold (−0.38 V vs RHE) depending on pathway convention; both points lie far on the weak-CO-binding branch of the Hammer–Nørskov volcano, well past Ag(111) and Au(111). The \*COOH/\*OCHO selectivity prefers \*COOH on both surfaces, so the bottleneck is the C–O cleavage step and the parasitic HER channel competing with it — not the selection of the first PCET branch.

---

## 3. Experimental work

A reproducible **pulsed-laser deposition (PLD)** process was established for the synthesis of oxide–graphene bilayers on fused-silica substrates.

### 3.1 Fabrication

- **Substrate:** fused silica, ultrasonically cleaned, plasma-activated prior to deposition
- **Targets:** WO₃ (proof-of-concept), CuO, AgO; rotating-target geometry
- **Carbon overlayer:** sputter-deposited and reduced to defect-rich graphitic carbon (rGO-like) under controlled atmosphere
- **Film stack so far:** WO₃–carbon bilayers as the lead system; CuO and AgO depositions in progress
- **Adhesion and uniformity:** confirmed visually and by spatially resolved Raman mapping

### 3.2 Characterisation

| Technique | Outcome |
|---|---|
| Raman spectroscopy | D and G bands of carbon overlayer; high I(D)/I(G) ratio consistent with rGO-like defect density |
| FT-IR | Limited oxidation of the carbon overlayer; oxide-mode signatures consistent with crystalline WO₃ |
| UV–Vis | Strong above-bandgap absorption; Tauc analysis used for *E*<sub>g</sub> estimation |
| Photoluminescence | Indicative of mid-gap defect states; data preliminary |

Raw spectra and fitting scripts are stored under `data/experimental/`.

---

## 4. Reproducibility

### 4.1 Reproducing the computational pipeline

```bash
# clone the repo and install Python dependencies
git clone <repo-url>
cd <repo>
pip install -r requirements.txt   # MACE-MP-0, CHGNet, ASE, scikit-learn, etc.

# run the MACE-MP-0 screening (single-GPU, ~2 h for 7 intermediates × 2 systems)
jupyter execute notebooks/CO2_methanol_MACE_v7.ipynb

# run the CHGNet screening
jupyter execute notebooks/CO2_methanol_CHGNet_ML_v6.ipynb

# combine results, generate figures
jupyter execute notebooks/CO2_methanol_analysis_FINAL.ipynb
```

---
## 5. License

Code is released under the MIT License. Data files under `data/` are released under CC-BY-4.0. See `LICENSE` and `LICENSE-data` for the full texts.

---

## 6. Acknowledgements

This work was supported by the **Australia–Japan Innovation Foundation (AJIF) 2024–2026**. Computational resources were provided by the **National Computational Infrastructure (NCI)** at the Australian National University, the **Pawsey Supercomputing Research Centre**, and the **Sydney Informatics Hub** at the University of Sydney. We thank the Materials Project for open access to its DFT database, and the MACE and CHGNet development teams for maintaining the open-source MLIP infrastructure on which this work depends.


