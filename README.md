# X-Ray Image Enhancement Pipelines

A comparative implementation of three image enhancement pipelines for medical X-ray images, spanning spatial-domain, frequency-domain, and hybrid multi-domain approaches. This project explores how different enhancement strategies affect diagnostic visibility, noise handling, and computational cost.

## Overview

Medical X-ray images often suffer from low contrast, noise, and uneven brightness across anatomical regions. This project implements and compares three distinct enhancement pipelines, each optimized for different use cases — from fast bone-fracture visualization to gold-standard radiologist-ready output.

## Pipelines

### 1. Spatial Domain Enhancement Pipeline
Enhances the image by operating directly on pixel values and their local neighborhoods.

**Steps:**
1. Convert image to grayscale
2. Apply mean (averaging) filtering to reduce noise
3. Apply CLAHE (Contrast Limited Adaptive Histogram Equalization) for local contrast enhancement
4. Apply adaptive gamma correction to brighten underexposed regions
5. Sharpen using a Gaussian-based unsharp mask
6. Normalize final output

**Strengths:** Fast, computationally cheap, excellent for low-contrast scans and visualizing fractures, joint spaces, and cortical bone.
**Limitations:** CLAHE can introduce halos or unrealistic shadows under intense noise.

### 2. Frequency Domain Enhancement Pipeline
Enhances the image by transforming it into the frequency domain to isolate and amplify structural detail.

**Steps:**
1. Convert image to grayscale
2. Apply Fourier Transform (FFT)
3. Shift the frequency spectrum to center low frequencies
4. Apply a Butterworth high-pass filter to enhance edges and fine structures
5. Apply frequency-domain contrast enhancement to amplify structural detail bands
6. Perform inverse FFT to return to the spatial domain
7. Normalize final output

**Strengths:** Effective at extracting organ/bone outlines, and removing periodic noise or electrical interference.
**Limitations:** Results can look flat and dark to the human eye; more computationally intensive than spatial filtering.

### 3. Hybrid Multi-Domain Enhancement Pipeline
Combines spatial and frequency-domain techniques through weighted fusion for balanced, radiologist-grade output.

**Steps:**
1. Convert image to grayscale
2. Apply spatial smoothing to reduce minor noise
3. Apply CLAHE for local contrast enhancement
4. Apply adaptive gamma correction
5. Apply spatial adaptive sharpening
6. Normalize spatial enhancement output
7. Perform Fourier Transform
8. Apply Butterworth high-pass frequency enhancement
9. Reconstruct via inverse Fourier Transform
10. Normalize frequency enhancement output
11. Fuse spatial and frequency components via weighted fusion
12. Generate the final hybrid enhanced image

**Strengths:** Best for pre-processing data for deep learning models and producing a "final view" for radiologists.
**Limitations:** Computationally expensive; overkill for simple thumbnail generation; requires careful tuning of fusion weights.

## Pipeline Comparison

| Aspect | Spatial Pipeline | Frequency Pipeline | Hybrid Pipeline |
|---|---|---|---|
| **How it works** | Operates on local pixel neighborhoods | Operates on image frequencies (FFT) | Combines spatial + frequency methods |
| **Main methods** | Blur, CLAHE, Sharpening | FFT, High-pass filter | Weighted fusion of both pipelines |
| **Result look** | Smooth but clearer image | Shows fine edges and structures | Balanced sharp + detailed image |
| **Speed** | Fast | Medium | Slow (runs both methods) |
| **Noise effect** | Reduces some noise | Selective noise handling | Balanced noise control |
| **Best use case** | General enhancement | Fine structures (vessels, patterns) | Mixed medical detail (lungs, complex X-rays) |
| **Known issues** | Can blur fine details | Can create ringing artifacts | Needs careful tuning |

## Choosing a Pipeline

- **Use the Spatial Pipeline** for fast, general-purpose enhancement and fracture/joint visualization.
- **Use the Frequency Pipeline** when the goal is isolating structural outlines or removing periodic/electrical noise.
- **Use the Hybrid Pipeline** when preparing data for deep learning models or producing final diagnostic images for radiologist review.

## References

1. *Optimizing Medical X-ray Image Quality: A Comparative Study of Enhancement Techniques* — [TIJER Paper](https://tijer.org/tijer/papers/TIJERE001007.pdf)
2. *Exploiting Hybrid Methods for Enhancing Digital X-Ray Images* — [IAJIT Paper](https://www.ccis2k.org/iajit/PDF/vol.10,no.1/3014-8.pdf)

## Contributing

Contributions, issues, and feature requests are welcome. Feel free to open a pull request or issue on this repository.
