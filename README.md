# Trustworthy Machine Learning – Assignment 4
## Watermark Forgery Attack


The objective of this assignment is to perform a **watermark forgery attack**. Given multiple sets of watermarked source images and clean target images, the goal is to find out the hidden watermark signal and transfer it onto unrelated clean images without access to the original watermarking algorithm or detector. The project shows that how invisible watermarking systems can be vulnerable to statistical and optimization-based attacks.

---

## Project Overview

We explored several approaches for recovering reusable watermark residuals from watermarked images.

### 1. Alpha-Blending Baseline
The provided baseline blends a watermarked source image with a clean target image. While simple, this method transfers visible image content instead of only the watermark, resulting in noticeable visual artifacts.

### 2. Statistical Averaging Attack
Assuming that images within the same watermark group share a common watermark pattern, we averaged the 25 source images and estimated the watermark residual by subtracting an estimate of the clean image content. This approach is computationally inexpensive and effective for watermark groups with stable additive watermarks.

### 3. Split-Half Diagnostic
To evaluate the consistency of the recovered watermark, each watermark group was divided into two subsets. Watermarks estimated from both halves were compared to determine whether the recovered residual represented a true shared watermark or merely image-specific noise.

### 4. Preference Model Gradient Extraction
For more challenging watermark groups, we employed a pretrained ConvNeXt-based image preference model as the optimization objective. Instead of directly estimating the watermark, an additive perturbation was optimized through gradient descent. The recovered perturbation served as the estimated watermark residual, providing significantly better performance on content-adaptive watermarking schemes.

---

## Optimization Techniques

Our implementation includes several techniques to improve stability and efficiency:

- Gradient-based optimization
- Warm-start initialization
- Tail averaging of optimization steps
- Exponential Moving Average (EMA)
- Group watermark averaging
- GPU acceleration using PyTorch

---

## Settings we used for our best submission
 
- num_steps = 50 (gradient steps per image)
- n_samples = 25 (used all 25 source images per group)
- last 10 steps averaged together at the end of each image's optimization to reduce noise
- warm start between images: 0.7 old estimate + 0.3 new estimate
- texture mask range: roughly 0.4x to 1.6x depending on how textured that part of the image is
## Attribution
 
The model architecture code we use to load the checkpoint  follows the architecture from:
 
Soucek, Rebuffi, Fernandez, Jovanovic, Elsahar, Lacatusu, Tran, Mourachko. "Transferable Black-Box One-Shot Forging of Watermarks via Image Preference Models." NeurIPS 2025.
Their code: https://github.com/facebookresearch/videoseal/tree/main/wmforger
 

---


## Results

Our experiments showed that:

- Alpha blending produced poor visual quality.
- Statistical averaging successfully recovered watermark patterns for simpler watermark groups.
- The split-half diagnostic confirmed that some watermark groups contained reusable watermark signals while others were content-adaptive.
- Preference-model optimization achieved the strongest overall performance by recovering watermark-like residuals even when statistical averaging was ineffective.

---



**Team ID:** LV

- Ananya Bhardwaz
- Aryan
