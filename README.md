# EN3160 – Assignment 1  
**Intensity Transformations & Neighborhood Filtering**

This repo contains my code and report for **EN3160 Image Processing and Machine Vision** (University of Moratuwa).

---

## What’s inside

```
.
├─ a1images/                 # input images given with the assignment
│  ├─ brain_proton_density_slice.png
│  ├─ highlights_and_shadows.jpg
│  ├─ spider.png
│  ├─ shells.tif
│  ├─ einstein.png
│  └─ a1q5images/            # small/large pairs for Q8
│     ├─ im01.png, im01small.png, ...
├─ Output_Images/            # all results saved here by the scripts
├─ code/
│  ├─ q1_intensity_lut.py
│  ├─ q2_brain_gaussian_lut.py
│  ├─ q3_gamma_L_in_LAB.py
│  ├─ q4_vibrance_hsv.py
│  ├─ q5_hist_equalization.py
│  ├─ q6_fg_hist_eq.py
│  ├─ q7_sobel.py
│  └─ q8_zoom_and_nssd.py
├─ report/
│  └─ 220528X_a01.pdf        # final report
├─ requirements.txt
└─ README.md                 # this file
```

> If you don’t see the `code/` folder in your copy, the snippets for each question are the same as in the report; you can paste them into a Python file or a Jupyter cell and run.

---

## Setup

1. **Python**: 3.9+ (tested on 3.10)  
2. **Install packages**
   ```bash
   python -m venv .venv
   # Windows
   .venv\\Scripts\\activate
   # macOS/Linux
   # source .venv/bin/activate
   pip install -r requirements.txt
   ```
   `requirements.txt` contains: `opencv-python`, `numpy`, `matplotlib`.

3. Make sure the **input images** are in `a1images/` as shown above.

---

## How to run

Each question has a small script under `code/`. Running a script will:

- read the required input image(s) from `a1images/`
- display results
- save all figures to `Output_Images/` with clear file names

Example:

```bash
python code/q1_intensity_lut.py
```

### Scripts and their main outputs

| Question | Script | Key inputs | Main outputs (in `Output_Images/`) |
|---|---|---|---|
| Q1 – Piecewise LUT | `q1_intensity_lut.py` | `a1images/emma.jpg` | `q1_lut.png`, `q1_out.png` |
| Q2 – Brain, Gaussian LUTs | `q2_brain_gaussian_lut.py` | `a1images/brain_proton_density_slice.png` | `q2_gray.png`, `q2_white.png`, `q2_lut_gray.png`, `q2_lut_white.png` |
| Q3 – Gamma on L* (LAB) | `q3_gamma_L_in_LAB.py` | `a1images/highlights_and_shadows.jpg` | `q3_gamma_corrected.png`, `q3_L_hist_original.png`, `q3_L_hist_gamma_0.8.png` |
| Q4 – Vibrance (HSV S-plane) | `q4_vibrance_hsv.py` | `a1images/spider.png` | `q4_vibrance_enhanced.png`, `q4_vibrance_lut.png`, `q4_HSV.png` |
| Q5 – Histogram Equalization | `q5_hist_equalization.py` | `a1images/shells.tif` | `q5_original_gray.png`, `q5_equalized.png`, `q5_hist_before.png`, `q5_hist_after.png` |
| Q6 – **Foreground-only** Hist. Eq. | `q6_fg_hist_eq.py` | `a1images/jeniffer.jpg` (or your Fig. 6) | `q6_hsv_planes.png`, `q6_mask.png`, `q6_original.png`, `q6_result_strict.png`, FG hist/CDF plots |
| Q7 – Sobel (3 ways) | `q7_sobel.py` | `a1images/einstein.png` | `q7_gx_filter2d.png`, `q7_gy_filter2d.png`, `q7_gx_manual.png`, `q7_gy_manual.png`, `q7_out.png` |
| Q8 – Zoom & nSSD | `q8_zoom_and_nssd.py` | `a1images/a1q5images/*` | upscaled images + `q8_nssd_results.csv` |
| Q9 – GrabCut & Background Blur | (in report) | `a1images/daisy.jpg` (or flower) | `q9_mask_bigflower.png`, `q9_fg_bigflower.png`, `q9_bg_bigflower.png`, `q9_enhanced_bigflower.png` |

> Note: Some file names in the assignment (e.g., “jennifer”, “daisy”) may differ. Update paths in the scripts if yours are different.

---

## Short notes per question

- **Q1**: Piecewise LUT (with discontinuities) boosts mid-tones; may cause slight banding.
- **Q2**: Gaussian “bump” LUTs smoothly highlight **gray** vs **white** matter (choose μ,σ).
- **Q3**: Gamma on **L*** only (in LAB) brightens/darkens without changing chroma. I used γ = 0.8.
- **Q4**: Vibrance modifies only **S** in HSV using a Gaussian bump; I used `a≈0.65`, `σ=70`.
- **Q5**: Custom histogram equalization via CDF-based LUT; handles 16‑bit TIFF by mapping to 8‑bit first.
- **Q6**: Foreground-only equalization on **V**. Mask from **S** (Otsu), equalize only FG, then recombine FG+BG.
- **Q7**: Sobel via (a) `filter2D`, (b) manual 2‑D conv, (c) separable `[1,2,1]^T * [1,0,-1]`. Same response; separable is faster.
- **Q8**: Implemented **nearest-neighbour** and **bilinear** zoom. Compared small→large using **normalized SSD (nSSD)**. Bilinear gives lower nSSD.
- **Q9**: GrabCut segmentation to keep only the big flower; blurred background; feathered alpha reduces dark halo along edges.

---

## Reproducibility tips

- Run each script from the repo root so relative paths work.
- For grayscale plots, set `vmin=0, vmax=255` in matplotlib to avoid washed-out displays.
- If `shells.tif` is 16‑bit, the script normalizes to 8‑bit before equalization.

---

## License / Credits

- Images belong to the course materials / original sources noted by the assignment.
- Code is for educational use for EN3160.

---

## Contact

- **Index**: 220528X  
- **Name**: Rathnayake R M T N B

If something doesn’t run, please open an issue or share the script name and full error message.
