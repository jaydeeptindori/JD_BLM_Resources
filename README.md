# Boundary-Layer Meteorology (BLM) Resources ✅

This repository collects documentation, formula sheets, case descriptions, parameterization summaries, and references for Boundary-Layer Meteorology (BLM). It is intended as a living resource for researchers, students, and model developers working on surface layers, boundary-layer processes, turbulence parameterizations, and cloud-topped boundary layers.

---

## Table of Contents

- [Overview](#overview)
- [Key Concepts & Scales](#key-concepts--scales)
- [Formula Sheet (Essential Equations)](#formula-sheet-essential-equations) 🔧
- [Surface Layer Parameterizations](#surface-layer-parameterizations)
- [Boundary Layer & Turbulence Parameterizations](#boundary-layer--turbulence-parameterizations)
- [Cloud & Shallow Convection in the BLM](#cloud--shallow-convection-in-the-blm)
- [Benchmark Case Studies](#benchmark-case-studies) 📚
  - ARM
  - BOMEX
  - GABLS
  - RICO
  - DYCOMS-II RF01 & RF02
- [Datasets, Metrics & Best Practices](#datasets-metrics--best-practices)
- [Recent Advances & Research Directions](#recent-advances--research-directions) ✨
- [Repository Organization & Contribution Guide](#repository-organization--contribution-guide)
- [References & Suggested Reading](#references--suggested-reading)

---

## Overview

Boundary-layer meteorology concerns the lowest portion of the atmosphere directly influenced by the surface over time scales of minutes to days. This includes the surface layer (lowest ~10% of the boundary layer), the mixed layer (convective cases), stable nocturnal boundary layers, and cloud-topped boundary layers (stratocumulus, shallow cumulus).

This README provides concise theory, practical formulae, parameterization summaries, and canonical case descriptions (ARM, BOMEX, GABLS, RICO, DYCOMS-II) for model testing and documentation.

---

## Key Concepts & Scales

- Surface layer vs. boundary layer vs. free troposphere
- Monin–Obukhov similarity theory (MOST)
- Friction velocity u* and Obukhov length L
- Turbulent kinetic energy (TKE) and closure levels (0th, 1st, 1.5-order, 2nd-order)
- Entrainment at the inversion, entrainment velocity we
- Convective boundary layer (CBL) vs. stable boundary layer (SBL)
- Cloud-topped boundary layer: stratocumulus vs. shallow cumulus

---

## Formula Sheet (Essential Equations) 🔧

Below are commonly used relations and definitions used in BLM.

- Friction velocity:

  u* = (|τ|/ρ)^(1/2), where τ is surface stress and ρ is air density.

- Monin–Obukhov length (L):

  L = - (u*^3 T0) / (κ g H_s / (ρ cp)), where H_s is surface sensible heat flux, κ is von Kármán constant, g is gravity, T0 is reference temperature.

- Gradient Richardson number (Ri):

  Ri = (g/θ) (∂θ/∂z) / ( (∂u/∂z)^2 + (∂v/∂z)^2 )

- Bulk flux formulation (surface sensible heat flux):

  H_s = ρ c_p C_h U (θ_s - θ_a)

- Turbulent kinetic energy (TKE) budget (schematic):

  ∂e/∂t + U·∇e = production (shear + buoyancy) - dissipation - divergence of fluxes + pressure correlations

- Eddy-diffusivity (K-theory):

  F_φ ≈ -K_φ ∂φ/∂z

- Typical closure relationships: k–ε, 1.5-order (TKE-based), Reynolds stress models

> Note: Users should consult detailed references for units and sign conventions when implementing these in code.

---

## Surface Layer Parameterizations

- Monin–Obukhov similarity: wind and scalar profiles as functions of z/L and stability correction functions (ψm, ψh).
- Roughness length z0 and displacement heights for vegetated/canopy surfaces.
- Bulk aerodynamic formulas used by many models and land-surface schemes.
- Parameterization tips: ensure consistency between z0, vegetation schemes, and surface flux computations; treat snow/ice and very smooth surfaces separately.

---

## Boundary Layer & Turbulence Parameterizations

- First-order local (K-theory) schemes: simple, cheap, can fail near inversion/entrainment boundaries.
- Non-local/Countergradient schemes (e.g., Holtslag): capture large-scale transport in convective boundary layers.
- 1.5-order (TKE) closures (Mellor–Yamada, Bougeault & Lacarrère): prognose TKE and compute eddy diffusivities from length scales.
- Higher-order closures (Reynolds stress): more accurate but computationally expensive.
- Mass-flux and combined schemes for shallow convection and mixing (useful for cloud-topped mixed layers).

---

## Cloud & Shallow Convection in the BLM

- Important processes: entrainment at cloud top, sub-grid variability of thermodynamic variables, cloud fraction and liquid water content parameterizations, drizzle and microphysics for maritime clouds.
- Shallow cumulus parameterizations: mass-flux and stochastic entrainment-based approaches for sub-grid clouds.
- Stratocumulus: strong coupling of turbulence, radiation, and microphysics; entrainment-limited growth and decoupling are key phenomena.

---

## Benchmark Case Studies 📚

Below are canonical case studies often used to test models and parameterizations.

### ARM (Atmospheric Radiation Measurement)
- Typical focus: continental shallow cumulus and boundary-layer cloud cases observed at ARM SGP or ARM Mobile Facility deployments.
- Data: radiosondes, surface fluxes, cloud radar, lidar, surface energy balance.
- Typical use: evaluate cloud fraction, liquid water path (LWP), diurnal cycle, radiation coupling.

### BOMEX (Barbados Oceanographic and Meteorological Experiment)
- Classic tropical oceanic shallow cumulus case used for LES intercomparisons (Siebesma et al. style setups).
- Typical initial condition: warm SST, weak large-scale ascent/descent, trade-wind profiles.
- Metrics: cloud fraction, mean profiles of θ, q, buoyancy flux, entrainment.

### GABLS (GEWEX Atmospheric Boundary-Layer Study)
- Focus: stable boundary layer (nocturnal SBL) intercomparisons.
- GABLS cases define idealized SBL initial and forcing conditions used to test SBL parameterizations.
- Metrics: wind profiles, turbulence decay, temperature inversion strength.

### RICO (Rain in Cumulus over the Ocean)
- Observationally rich field campaign focusing on maritime shallow cumulus with precipitation.
- Useful for testing microphysics + shallow convection coupling, cloud lifetime, and precipitation statistics.

### DYCOMS-II RF01 & RF02 (Dynamics and Chemistry of Marine Stratocumulus)
- RF01: (often) stratocumulus case with weak synoptic forcing—used widely in LES and SCM intercomparisons.
- RF02: variant with different large-scale forcing and decoupling tendencies.
- Focus: entrainment, cloud-top radiative cooling, drizzle effects, turbulence-radiation-microphysics coupling.

---

## Datasets, Metrics & Best Practices

- Use observational datasets: ARM SGP, RICO flight datasets, radiosonde archives, ship-based flux measurements.
- Key evaluation metrics: cloud fraction, LWP, cloud-top height, entrainment rate, TKE, surface fluxes, RMSE of mean profiles.
- LES best practices: domain size adequate for largest eddies, grid resolution sensitivity, long enough spinup, stochastic seeding for shallow cumulus where needed.

---

## Recent Advances & Research Directions ✨

- Scale-aware parameterizations (coupling across grid spacings)
- Machine-learning closures for turbulence and subgrid clouds
- Stochastic parameterizations to represent subgrid variability and intermittency
- Better coupling between land-surface, canopy, and boundary-layer schemes
- Improved representations of aerosol-cloud interactions and drizzle processes in stratocumulus

---

## Repository Organization & Contribution Guide 🔧

Suggested top-level structure:

- `docs/` — detailed write-ups and formula sheets (e.g., `docs/formula_sheet.md`, `docs/parameterizations.md`)
- `cases/` — canonical case setups (scripts, input profiles, LES/SCM configs) for ARM, BOMEX, GABLS, RICO, DYCOMS-II
- `notebooks/` — analysis notebooks for diagnostics and intercomparison plots
- `scripts/` — utilities to process observational datasets and compute metrics
- `references/` — curated bibliography and downloadable datasets (or links)

Contribution checklist:
1. Add or update a subdoc in `docs/` describing the algorithm, references, and typical tuning parameters.
2. Add example code in `cases/` with a README describing input files & expected outputs.
3. Add tests/notebooks that demonstrate the expected behavior and metrics.

---

## References & Suggested Reading 📚

Below are classic and widely cited references (non-exhaustive):
- Stull, R. B., 1988: An Introduction to Boundary Layer Meteorology.
- Wyngaard, J. C., 2010: Turbulence in the Atmosphere.
- Siebesma et al., BOMEX LES intercomparison, J. Atmos. Sci.
- Large Eddy Simulation intercomparisons for DYCOMS-II and RICO (see literature by Stevens, Bretherton, etc.)
- GABLS intercomparison papers and case descriptions

---

## Quick Start: Adding a New Case

1. Create a folder in `cases/<CASE_NAME>/` with the initial profiles and forcing files.
2. Add a `README.md` describing observational references and the typical metrics to compute.
3. Add an example notebook in `notebooks/` showing how to compute cloud fraction, LWP, entrainment rate.

---

## License & Citation

Include a `LICENSE` file at the repo root that suits your group's policies (e.g., MIT or CC-BY). When using/quoting materials, please cite the original observational or model intercomparison references.

---

## Contact / Maintainers

For contributions and questions, open an issue or PR. Include tests or diagnostics demonstrating behavior when adding new parameterizations or case configurations.

---

Thank you for using this resource — contributions welcome! ✅