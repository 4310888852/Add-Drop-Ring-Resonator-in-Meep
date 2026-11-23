# Add-Drop Ring Resonator in Meep

Open-source FDTD simulation of a silicon-photonic add-drop ring resonator using [Meep](https://meep.readthedocs.io). Runs in Google Colab.

## Features
- Parametric ring + two bus waveguides (radius, gap, width)
- Mode source and eigenmode monitors: through, drop, reflection
- Extraction of FSR, FWHM, loaded Q, extinction ratio
- Add-drop model fit with lossy couplers (through amplitude r, cross power κ², round-trip amplitude a)
- Group index from the waveguide mode solver, compared with the fitted FSR
- Optional coupling-gap sweep, resonance field plot, `.npz` and `.gds` export

## Parameters
| Parameter | Value |
|---|---|
| Core / cladding | Si (n = 3.476) / SiO2 (n = 1.444), 220 nm Si |
| Model | 2D, effective index of the slab, TE-like |
| Waveguide width | 450 nm |
| Ring radius (centre line) | 5 µm (circumference 31.4 µm) |
| Coupling gap | 100 nm |
| Window | 1.53–1.57 µm, 401 wavelength points |

## Results (2D; some values read from output)
| Quantity | Value |
|---|---|
| Resonances | 1.5324 µm, 1.5560 µm |
| FSR | 23.5 nm (peaks), 23.96 nm (fit), 24.02 nm (expected) |
| Group index | 3.217 (fit) vs 3.182 (mode solver) |
| Loaded Q | 982 / 897 (FWHM 1.56 / 1.74 nm) |
| Drop peak | -0.98 dB / -0.79 dB |
| Through extinction | 25.4 dB |
| Fit | r² = 0.835, κ² = 0.167, round-trip loss ≈ 0.12 dB |

The fit gives r² + κ² ≈ 1.0015, i.e. an essentially lossless coupler; the small negative "coupler loss" is the fit hitting that limit, not gain.

## Convergence note
Ring resonators ring down slowly. Use a tight decay tolerance (`DECAY_TOL = 1e-8`, `T_MIN = 1500`, `T_MAX = 8000`). The notebook prints `max(T+D+R)`; for a passive device it must be ≤ 1. In the run reported above it was 1.051, so numbers carry a few percent uncertainty. Values above ~1.02 mean the spectrum is not fully converged or normalization is noisy.

## Limitations
2D effective-index model; confirm any design with 3D FDTD. Spectral sampling (~0.1 nm) limits the accuracy of narrow-resonance Q values (resonance FWHM ≈ 1.6 nm here).

## Usage
1. Open the notebook in Colab and run the install cells (the runtime restarts once).
2. Set `QUICK_TEST = True` first to check the pipeline.
3. Run the remaining cells in order.

## Requirements
`pymeep`, `numpy`, `scipy`, `matplotlib`, `gdstk`.
