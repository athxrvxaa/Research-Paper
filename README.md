# Softmax Degradation in Photonic Attention Accelerators: A Noise-Budget Analysis

Code and simulations for the paper **"Softmax Degradation in Photonic Attention Accelerators: A Noise-Budget Analysis"** by Atharva Raskar (Fergusson College, Pune).

## Overview

Photonic transformer accelerators do matrix multiplication in the optical domain, but they all offload softmax to digital electronics. That creates an ADC bottleneck whose noise budget had never been formally characterized. This work derives how much photon shot noise (Standard Quantum Limit model of Hamerly et al.) the attention softmax can tolerate.

## Key Results

- **Closed-form KL bound:** `E[KL] ≈ (1 − E[‖p‖²]) / (2·n_mac)`, within 2% error for `n_mac ≥ 10`.
- **d_k invariance:** KL is independent of head dimension (CV ≤ 2% across d_k ∈ {16 … 512}, ANOVA p = 0.90).
- **Weak sequence-length dependence:** a 64× increase in `n_seq` raises KL by only 1.30×.
- **Power law:** KL vs. `n_mac` exponent is −0.99 ± 0.02 (theory: −1).
- **Photon budget:** `n*_mac ≈ 100 photons/MAC` gives KL ≤ 0.005 for practical (d_k, n_seq).
- **Secondary observations:** shot noise ≈ temperature scaling `T_eff = √(1 + σ²)`. Phase noise adds independently (`n†_mac ≈ 0.6/σ_Φ²`).

## Repository Structure

```
.
├── paper/          # Manuscript PDF / LaTeX source
├── src/            # Simulation code (PyTorch)
├── experiments/    # Sweeps A–D (n_mac, n_seq, d_k, temperature)
├── figures/        # Fig. 1–3 and Table I outputs
├── requirements.txt
└── README.md
```

## Setup

```bash
git clone https://github.com/athxrvxaa/Research-Paper.git
cd Research-Paper
pip install -r requirements.txt
```

Tested with PyTorch 2.2 on an NVIDIA A100 (a GPU is optional but speeds things up).

## Reproducing the Experiments

Seed `42`; B = 1024 score vectors × T = 10 trials per config; 95% bootstrap CIs.

| Sweep | Varies | Fixed | Validates |
|---|---|---|---|
| A | n_mac ∈ {1 … 2000} | n_seq = d_k = 64 | 1/n_mac scaling (Fig. 1) |
| B | n_seq ∈ {4 … 1024} | n_mac ∈ {10, 50, 100}, d_k = 64 | n_seq saturation (Fig. 3, Table I) |
| C | d_k ∈ {16 … 512} | n_mac ∈ {10, 50, 100}, n_seq = 64 | d_k invariance (Fig. 2) |
| D | σ | n_mac ≥ 10 | Temperature equivalence |

```bash
python experiments/sweep_a.py   # adjust script names to match your repo
```

## Citation

```bibtex
@misc{raskar2026softmax,
  title  = {Softmax Degradation in Photonic Attention Accelerators: A Noise-Budget Analysis},
  author = {Raskar, Atharva},
  year   = {2026},
  url    = {https://github.com/athxrvxaa/Research-Paper}
}
```

## Contact

Atharva Raskar — atharvaraskar44@gmail.com
