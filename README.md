# Independent Reanalysis of Atacama Cosmology Telescope Data Release 6: Daytime and Nighttime Power Spectra, Map-Level Null Tests, Beam Systematics, and Differentiable Cosmological Inference

**Preprint repository submitted to [ParallelArxiv](https://papers.parallelscience.org).**

- **Landing Page & Online Reader:** [https://parallelscience.github.io/act-dr6-daynight-reanalysis/](https://parallelscience.github.io/act-dr6-daynight-reanalysis/)
- **Compiled Paper PDF:** [`docs/paper.pdf`](docs/paper.pdf)
- **BibTeX Citations:** [`docs/bibliography.bib`](docs/bibliography.bib)

---

## Abstract

We present an independent end-to-end reanalysis of the Atacama Cosmology Telescope Data Release 6 (ACT DR6) daytime and nighttime sky maps across the PA4 (150, 220 GHz), PA5 (90, 150 GHz), and PA6 (90, 150 GHz) detector arrays, spanning angular scales $600 \le \ell \le 2500$ and combining cross-split power-spectrum reconstruction, map-level time-split null tests, Spin-2 polarization diagnostics, beam-systematic modeling, and GPU-accelerated Bayesian cosmological inference. 

Evaluating mode-decoupled pseudo-$C_\ell$ estimators on the Needlet Internal Linear Combination (NILC) analysis footprint ($f_{\mathrm{sky}} = 0.312$) with point-source-subtracted maps, nominal instrument beams, and exact SACC bandpower window functions, we reproduce the official ACT DR6 nighttime auto-frequency bandpowers to within $0.1\%\text{--}1.4\%$ ($|\Delta D_\ell / D_\ell^{\mathrm{off}}| \le 0.006$ across $800 \le \ell \le 2000$ for PA5 $90\,\mathrm{GHz}$, PA5 $150\,\mathrm{GHz}$, and PA6 $150\,\mathrm{GHz}$) and all six cross-frequency channels to within $0.2\%\text{--}2.0\%$ once co-temporal split pairs ($i = j$) are excluded to eliminate correlated atmospheric noise. 

Map-level four-way time-split null tests $\langle (s_0 - s_1) \times (s_2 - s_3) \rangle$ and $\langle (s_0 - s_2) \times (s_1 - s_3) \rangle$ pass across the full multipole range with probabilities to exceed $\mathrm{PTE} = 0.45$ and $0.30$, respectively. 

For the daytime dataset, we derive unbiased relative gain calibration factors $c_{\mathrm{day}} = \langle D_\ell^{n_0 \times n_1} / D_\ell^{d \times n} \rangle$ from day$\times$night cross-spectra across all six array-frequency bands, obtaining $c_{\mathrm{day}} = 1.030 \pm 0.003$ (PA5 $90\,\mathrm{GHz}$) and $c_{\mathrm{day}} = 0.992 \pm 0.003$ (PA5 $150\,\mathrm{GHz}$) on the NILC footprint, along with unit split-gain invariants $r_{\mathrm{split}}$ confirming phase coherence between the daytime and nighttime sky maps. Direct Spin-2 spherical harmonic analysis of the daytime Stokes $(Q, U)$ splits reveals parity-conserving and parity-violating residuals ($|D_\ell^{BB}| > |D_\ell^{EE}| \sim 10^2\text{--}10^3\,\mu\mathrm{K}^2$, $|D_\ell^{EB}| \neq 0$) two to three orders of magnitude above the cosmological $E$-mode signal and uncorrelated with nighttime polarization, whereas nighttime temperature-to-polarization leakage remains below $0.036\%$, establishing from first principles why daytime cosmological analyses must be restricted to temperature. 

We further investigate a $2\sigma$ high-$\ell$ deficit in PA6 $90\,\mathrm{GHz}$ nighttime auto-spectra, showing that a $1\%$ linear beam-transfer correction above $\ell = 1000$ improves the full-covariance goodness of fit from $\chi^2/\mathrm{ndf} = 2.066$ ($\mathrm{PTE} = 5.3 \times 10^{-4}$) to $\chi^2/\mathrm{ndf} = 0.675$ ($\mathrm{PTE} = 0.910$), and demonstrate via a six-pair split covariance analysis and 14-mode beam principal-component projection of PA5 $90\,\mathrm{GHz}$ that a localized $-6.7\%$ residual at $\ell = 2375.5$ originates from map-level compact-source subtraction rather than smooth beam modes. 

Incorporating our calibrated daytime and nighttime bandpowers into a differentiable neural-emulated $\Lambda\mathrm{CDM}$ likelihood, we find that daytime temperature data tighten $TT$-only parameter posteriors by $11.3\%$ in $H_0$ and $12.1\%$ in $n_s$, while contributing a $\simeq 1.0\%$ tightening in the full $TT+TE+EE$ likelihood where nighttime polarization dominates the Fisher weight. Marginalizing over the 14 PA5 $90\,\mathrm{GHz}$ beam uncertainty modes shifts $n_s$ by $+0.83\sigma$ ($0.9689 \pm 0.0070 \to 0.9765 \pm 0.0092$), highlighting the coupling between high-$\ell$ $90\,\mathrm{GHz}$ beam calibration and the scalar spectral index.

---

## Repository Structure

```
act-dr6-daynight-reanalysis/
├── docs/
│   ├── index.html          # ParallelArxiv landing page with embedded viewer
│   ├── paper.pdf           # 14-page publication-grade compiled PDF (Physical Review D format)
│   └── bibliography.bib    # BibTeX citation database
└── README.md
```
