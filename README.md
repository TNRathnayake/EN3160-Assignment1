# EN3160 — Assignment 1  
_Image Processing & Machine Vision_

**Student:** Rathnayake R M T N B (220528X)  
**Course:** EN3160 Image Processing and Machine Vision  
**University:** University of Moratuwa

---

## Overview

This repository contains my solutions for Assignment 1 on intensity transformations and neighbourhood filtering. All experiments, figures, and plots were generated from a **single Jupyter notebook** and exported into the `Output_Images/` folder for inclusion in the report.

The tasks span point-wise intensity mappings, colour‑space manipulations, histogram equalization (global and masked), spatial filtering with Sobel, image zooming/interpolation with error analysis, and foreground segmentation with GrabCut.


## What’s inside

- **`A01.ipynb`** — the single notebook used to produce every figure and result.
- **`a1images/`** — input images supplied with the assignment.
- **`Output_Images/`** — all generated figures used in the report (PNG).
- **`README.md`** — this file.
- **`report/`** — LaTeX sources for the final PDF, if present.


## Methods & Highlights (by question)

### Q1 — Piecewise Intensity Transformation
A 256‑entry LUT implements the given piecewise mapping. Shadows (0–50) and highlights (151–255) remain mostly unchanged, while midtones (51–150) are amplified with a slope of ~1.55 and a deliberate jump at 150.  
**Figures:** `q1_lut.png`, `q1_out.png`

### Q2 — Accentuating Gray/White Matter (MRI PD slice)
A smooth Gaussian pulse is used as the intensity transform:
\(
f(x)=b+A\exp\!\big(-\frac{(x-\mu)^2}{2\sigma^2}\big)
\).
\(\mu\) and \(\sigma\) are chosen to brighten narrow, tissue‑specific bands (GM ≈150, WM ≈200; \(\sigma=15\)).  
**Figures:** `q2_gray.png`, `q2_white.png`, and LUT plots `q2_lut_gray.png`, `q2_lut_white.png`.

### Q3 — Gamma on L\* (CIELAB)
The image is converted to L\*a\*b\*, and a power law is applied to the **L\*** channel with \(\gamma=0.8\). This brightens shadows while preserving chroma (a\*, b\* unchanged). Histograms for the L\* channel are shown before/after.  
**Figures:** `q3_gamma_corrected.png`, `q3_L_hist_original.png`, `q3_L_hist_gamma_0.8.png`.

### Q4 — “Vibrance” via Saturation Bump (HSV)
Only the **S** channel is modified using  
\(
f(x)=\min\{x+a\cdot 128\,e^{-(x-128)^2/(2\sigma^2)},\,255\}
\)
with \(a=0.65\), \(\sigma=70\). This selectively increases mid‑range saturation; very low/high saturation is minimally altered.  
**Figures:** `q4_HSV.png`, `q4_vibrance_enhanced.png`, `q4_vibrance_lut.png`.

### Q5 — Custom Histogram Equalization (Grayscale)
A hand‑written implementation computes the histogram, CDF, and LUT  
\(
\mathrm{LUT}(k)=\big(\frac{\mathrm{CDF}(k)-\mathrm{CDF}_{\min}}{N-\mathrm{CDF}_{\min}}\big)\cdot 255
\)
and applies it to a grayscale image (with safe handling for 16‑bit inputs).  
**Figures:** `q5_original_gray.png`, `q5_equalized.png`, `q5_hist_before.png`, `q5_hist_after.png`.

### Q6 — Foreground‑Only Histogram Equalization
The image is split to HSV. A binary **mask** is obtained from **S** (Otsu thresholding + light morphology). The histogram equalization LUT is built **only from the foreground pixels of V**, then the equalized foreground is recomposed with the original background.  
**Figures:** `q6_hsv_planes.png`, `q6_mask.png`, `q6_original.png`, `q6_result_strict.png`, plus foreground histograms/CDFs `q6_fg_*`.

### Q7 — Sobel Gradient (Three Ways)
Sobel \(3\times 3\) filters are applied via:
1) `cv2.filter2D` (reference),  
2) a custom 2D convolution,  
3) a separable implementation \(K_x=[1,2,1]^T[\,1,0,-1\,]\), \(K_y=K_x^T\).  
Signed responses are linearly mapped to 0–255 (0→128) for visualization.  
**Figures:** `q7_gx_filter2d.png`, `q7_gy_filter2d.png`, `q7_gx_manual.png`, `q7_gy_manual.png`, and the four‑panel separable output `q7_out.png`.

### Q8 — Zoom (Nearest vs. Bilinear) + nSSD
Small images are upscaled exactly to their large counterparts using nearest‑neighbour and bilinear interpolation. Quality is summarized by **normalized SSD** (lower is better).
| Pair | nSSD (NN) | nSSD (Bilinear) |
|---|---:|---:|
| im01small → im01 | 0.012058 | **0.010187** |
| im02small → im02 | 0.004190 | **0.002915** |
| im03small → im03 | 0.007530 | **0.005634** |

### Q9 — GrabCut Segmentation + Background Blur
GrabCut is initialized with a mask: borders as sure background, a loose ellipse as probable foreground (big flower), and a small rectangle excluding the bud. The final mask yields separated foreground/background. A strongly blurred background is blended with a feathered alpha to avoid hard cut‑outs.  
**Figures:** `q9_mask_bigflower.png`, `q9_fg_bigflower.png`, `q9_bg_bigflower.png`, `q9_enhanced_bigflower.png`.

---

## Notes & Limitations

- Piecewise mappings with jumps (Q1) can introduce visible banding in smooth regions; continuous LUTs avoid this.  
- Point‑wise transforms (Q2, Q3, Q4, Q5) do not denoise; neighbourhood filters would be required for that.  
- Foreground equalization (Q6) depends on the mask quality; soft blending can reduce edge artifacts.  
- For Q8, bilinear consistently reduced nSSD versus nearest‑neighbour, as expected.  
- GrabCut seeds (Q9) were chosen to segment only the main flower; different seeds change the outcome.


## Acknowledgements

Starter images are provided with the assignment. OpenCV and NumPy were used for image operations, and Matplotlib for visualizations.
