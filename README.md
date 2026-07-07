# Trustworthy Machine Learning – Assignment 4
## Watermark Forgery Attack


The objective of this assignment is to perform a **watermark forgery attack**. Given are multiple sets of watermarked source images and clean target images, the goal is to find out the hidden watermark signal and transfer it onto unrelated clean images without access to the original watermarking algorithm or detector. The project shows that how invisible watermarking systems can be vulnerable to statistical and optimization-based attacks.

---

## Methodology
 
We took a black-box forgery approach for this problem: instead of trying to identify the watermarking scheme, we use a pretrained ConvNeXt-V2 based image preference model as a proxy for 'if this looks watermarked' / low quality, and extract each group's watermark from it.
 
- Loading the pretrained ConvNeXt-V2 preference model, which scores how clean or high-quality an image looks.
- For each watermarked source image, optimize an additive perturbation `delta` via SGD (`lr=0.05`, `50` steps, image resized to `768x768`) to maximize the preference model's score on `image + delta` — pushing the image towards what the model considers clean.
- Average the deltas from the last `10` optimization steps instead of using only the final one, to reduce noise.
- Taking the difference between the original image and the cleaned version as that image's estimate of the watermark signal.
- Within each of the 8 source groups (25 images each), start each image's optimization from an exponential moving average of the previous images' deltas (`0.7 * old + 0.3 * new`), so the 25 estimates converge toward a shared signal instead of staying noisy.
- Average the 25 per-image estimates to get one watermark per group.
- Add each group's averaged watermark directly onto the corresponding batch of clean target images, clipping pixel values to `[0, 255]`.


---
## Hyperparameters

| Parameter | Value |
|-----------|------:|
| Optimizer | SGD |
| Learning Rate | 0.05 |
| Optimization Steps | 50 |
| Images per Watermark Group | 25 |
| Tail Averaging | Last 10 iterations |
| Warm Start | 0.7 previous + 0.3 current |
| Optimization Resolution | 768 × 768 |



---

## Reference
 
The model architecture code we use to load the checkpoint  follows the architecture from:
 
Soucek, Rebuffi, Fernandez, Jovanovic, Elsahar, Lacatusu, Tran, Mourachko. "Transferable Black-Box One-Shot Forging of Watermarks via Image Preference Models." NeurIPS 2025.
Their code: https://github.com/facebookresearch/videoseal/tree/main/wmforger
 

---




**Team ID:** LV

- Ananya Bhardwaz
- Aryan
