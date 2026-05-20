# Programmatic Investigation: Pixel-Level Matrix Subtraction Limits in MRI Segmentation
**Author:** Isra Bouchentouf  
**Context:** Independent Neuro-Technology Research | May 2026  

This repository marks the first phase of my autonomous computational neuroscience project. Before adopting complex deep learning frameworks, I wanted to test the physical and mathematical boundaries of deterministic pixel arithmetic (`cv2.absdiff`) using real patient datasets on a cloud-based GPU environment via Google Colab. 

The baseline script was calibrated successfully using a custom-generated 8-bit integer zero-matrix array ($512 \times 512$ pixels) containing a hyper-intense synthetic tumor focus. However, when I transitioned the pipeline to ingest real clinical clinical datasets, two systemic engineering bottlenecks emerged:

1. **The Cross-Subject Anatomical Variance Trap:** 
When evaluating a healthy brain slice from one individual against a glioblastoma slice from a different patient (even after resizing matrices to $512 \times 512$ and normalising contrast via Global Histogram Equalization), the absolute subtraction loop generated catastrophic noise. The algorithm failed to isolate the oncological mass because it processed normal structural variations—such as skull contours and individual cortical sulci—as pathological anomalies. 

2. **The Contrast Inversion Conflict (T1 vs. T2 Sequence Physics):** 
To eliminate cross-subject biological variance, I executed differential matrix computation on structural "digital twins"—paired MRI slices belonging to the exact same patient. However, a profound imaging sequence conflict emerged: the healthy baseline was captured via a T2-weighted sequence, while the pathological slice was a post-contrast T1-weighted sequence. Even after deploying a manual matrix inversion layer ($255 - I$), the algorithm failed. Pixel manipulation cannot normalize the underlying quantum physics differences between T1 and T2 relaxation times. The scanner parameters remained statistically mismatched, burying the tumor under systemic artifacts.

### Computational Trajectory
These failure points establish an unshakeable computational fact: **Deterministic mathematics and basic pixel arithmetic are fundamentally unsafe for automated tumor segmentation due to structural and sequence variances.**

To bypass human anatomical registration issues and cross-scanner parameters, I am abandoning rigid pixel subtraction. The next phase of this project will transition kly toward training a specialized **U-Net architecture on PyTorch** to recognize the generalized geometric and textural patterns of neuro-oncological tissues autonomously.
