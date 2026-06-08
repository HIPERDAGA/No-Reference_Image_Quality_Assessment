# No-Reference Image Quality Assessment

This repository contains three Google Colab notebooks for understanding and visualizing **No-Reference Image Quality Assessment (NR-IQA)** metrics.

The repository focuses on three widely used NR-IQA approaches:

- **BRISQUE**: Blind/Referenceless Image Spatial Quality Evaluator.
- **NIQE**: Natural Image Quality Evaluator.
- **PIQE**: Perception-based Image Quality Evaluator.

These methods estimate image quality **without requiring a reference or pristine image**, which makes them useful for real-world computer vision applications where the original image is not available.

---

## Repository purpose

The main goal of this repository is to provide educational, step-by-step notebooks that allow users to:

1. Upload an image from their computer.
2. Visualize the preprocessing steps.
3. Understand how each metric analyzes the image.
4. Display intermediate maps, histograms, blocks, or feature representations.
5. Compute the final image quality score.

This repository is useful for students, researchers, and developers working on image preprocessing, computer vision, object detection, adverse weather image analysis, and no-reference image quality assessment.

---

## Repository structure

```text
No-Reference_Image_Quality_Assessment/
│
├── BRISQUE_step_by_step_Colab (1).ipynb
├── NIQE_step_by_step_Colab (1).ipynb
├── PIQE_step_by_step_Colab (1).ipynb
└── README.md
```

> If you rename the notebooks, update the links in this README accordingly.

---

## Open notebooks in Google Colab

### BRISQUE notebook

[Open BRISQUE in Colab](https://colab.research.google.com/github/HIPERDAGA/No-Reference_Image_Quality_Assessment/blob/main/BRISQUE_step_by_step_Colab%20%281%29.ipynb)

### NIQE notebook

[Open NIQE in Colab](https://colab.research.google.com/github/HIPERDAGA/No-Reference_Image_Quality_Assessment/blob/main/NIQE_step_by_step_Colab%20%281%29.ipynb)

### PIQE notebook

[Open PIQE in Colab](https://colab.research.google.com/github/HIPERDAGA/No-Reference_Image_Quality_Assessment/blob/main/PIQE_step_by_step_Colab%20%281%29.ipynb)

---

## Notebook 1: BRISQUE step by step

**File:** `BRISQUE_step_by_step_Colab (1).ipynb`

BRISQUE stands for **Blind/Referenceless Image Spatial Quality Evaluator**. It is a no-reference image quality metric based on **Natural Scene Statistics (NSS)** in the spatial domain.

Unlike methods that transform the image into another domain, such as wavelet or DCT, BRISQUE works directly with spatial luminance information.

### What this notebook does

The BRISQUE notebook performs the following steps:

1. Uploads an image from the local computer.
2. Converts the image to grayscale/luminance.
3. Computes **Mean Subtracted Contrast Normalized (MSCN)** coefficients.
4. Displays the local mean, local standard deviation, and MSCN map.
5. Fits a **Generalized Gaussian Distribution (GGD)** to the MSCN coefficients.
6. Computes pairwise products of neighboring MSCN coefficients in four directions:
   - Horizontal
   - Vertical
   - Main diagonal
   - Secondary diagonal
7. Extracts **Asymmetric Generalized Gaussian Distribution (AGGD)** features.
8. Repeats the feature extraction process at two image scales.
9. Builds the 36-dimensional BRISQUE feature vector.
10. Computes the final BRISQUE score using a pretrained model.

### BRISQUE workflow

```text
Image
→ Grayscale
→ MSCN coefficients
→ GGD features
→ Pairwise products
→ AGGD features
→ Two-scale feature vector
→ Regression model
→ BRISQUE score
```

### Score interpretation

In general:

```text
Lower BRISQUE score  → better perceptual image quality
Higher BRISQUE score → stronger perceptual degradation
```

---

## Notebook 2: NIQE step by step

**File:** `NIQE_step_by_step_Colab (1).ipynb`

NIQE stands for **Natural Image Quality Evaluator**. It is a no-reference and opinion-unaware image quality metric.

Unlike BRISQUE, NIQE does not require training on human opinion scores from distorted images. Instead, it compares the statistical features of the test image with a statistical model built from natural, undistorted images.

### What this notebook does

The NIQE notebook performs the following steps:

1. Uploads an image from the local computer.
2. Converts the image to grayscale/luminance.
3. Computes MSCN coefficients.
4. Displays the MSCN map and histogram.
5. Divides the image into local patches.
6. Selects spatially active patches.
7. Extracts Natural Scene Statistics features from selected patches.
8. Builds a test-image **Multivariate Gaussian (MVG)** model.
9. Compares the test-image model with a natural-image model.
10. Computes the final NIQE score.

### NIQE workflow

```text
Image
→ Grayscale
→ MSCN coefficients
→ Patch selection
→ NSS feature extraction
→ Multivariate Gaussian model
→ Distance from natural-image model
→ NIQE score
```

### Score interpretation

In general:

```text
Lower NIQE score  → better natural perceptual quality
Higher NIQE score → stronger deviation from natural image statistics
```

NIQE is especially useful when the goal is to evaluate how much an image deviates from the statistical behavior of natural images.

---

## Notebook 3: PIQE step by step

**File:** `PIQE_step_by_step_Colab (1).ipynb`

PIQE stands for **Perception-based Image Quality Evaluator**. In the original paper, it also appears as **PIQUE**.

PIQE is a no-reference image quality metric that works at the local block level. It estimates image quality by analyzing perceptually important regions and generating a distortion map.

### What this notebook does

The PIQE notebook performs the following steps:

1. Uploads an image from the local computer.
2. Converts the image to grayscale/luminance.
3. Computes MSCN coefficients.
4. Divides the image into non-overlapping **16 × 16 blocks**.
5. Classifies blocks as:
   - Uniform blocks
   - Spatially active blocks
6. Applies the noticeable distortion criterion.
7. Applies the noise criterion.
8. Assigns local distortion scores to affected blocks.
9. Generates a block-level distortion map.
10. Pools the local scores to compute the final PIQE score.

### PIQE workflow

```text
Image
→ Grayscale
→ MSCN coefficients
→ 16 × 16 blocks
→ Spatial activity detection
→ Distortion detection
→ Distortion map
→ PIQE score
```

### Score interpretation

The PIQE score is commonly interpreted as:

```text
Lower PIQE score  → better perceptual image quality
Higher PIQE score → stronger perceptual degradation
```

In this repository, the score may be displayed both in the original range from **0 to 1** and in a scaled range from **0 to 100**.

---

## Comparison of the three notebooks

| Metric | Full name | Main idea | Uses reference image? | Main output |
|---|---|---|---|---|
| BRISQUE | Blind/Referenceless Image Spatial Quality Evaluator | Uses NSS features from MSCN coefficients and pairwise products | No | BRISQUE score |
| NIQE | Natural Image Quality Evaluator | Compares image statistics with a natural-image model | No | NIQE score |
| PIQE | Perception-based Image Quality Evaluator | Detects local distortions in spatially active blocks | No | PIQE score and distortion map |

---

## Main concepts covered

This repository introduces and visualizes the following concepts:

- No-reference image quality assessment
- Natural Scene Statistics
- MSCN coefficients
- Local mean and local standard deviation
- GGD modeling
- AGGD modeling
- Pairwise product distributions
- Multiscale feature extraction
- Patch-level analysis
- Spatially active blocks
- Distortion maps
- BRISQUE, NIQE, and PIQE scores

---

## Requirements

The notebooks are designed to run in **Google Colab**, so no local installation is required.

The notebooks install the required Python packages automatically when executed. The main libraries used include:

```text
opencv-python-headless
opencv-contrib-python-headless
numpy
scipy
pandas
matplotlib
pillow
torch
pyiqa
```

---

## How to use this repository

1. Open one of the notebooks in Google Colab.
2. Run the installation/import cell.
3. Upload an image from your computer when prompted.
4. Execute the cells in order.
5. Observe the intermediate results:
   - Grayscale image
   - MSCN map
   - Histograms
   - Patch or block maps
   - Feature values
   - Distortion maps
6. Review the final quality score.

---

## Suggested use cases

This repository can be used for:

- Academic explanation of no-reference image quality metrics.
- Research on image preprocessing.
- Image quality analysis under adverse conditions.
- Computer vision pipelines.
- Object detection preprocessing evaluation.
- Comparison between original and enhanced images.
- Teaching BRISQUE, NIQE, and PIQE step by step.

---

## Notes

- These notebooks are intended for educational and research purposes.
- The scores should be interpreted carefully, since image content, resolution, lighting, and environmental conditions can affect the results.
- Lower scores usually indicate better quality, but exact thresholds may vary depending on the metric and dataset.
- BRISQUE, NIQE, and PIQE are not equivalent metrics; each one evaluates image quality using a different strategy.

---

## References

The theoretical basis of the notebooks comes from the following works:

```bibtex
@article{mittal2012brisque,
  author  = {Mittal, Anish and Moorthy, Anush Krishna and Bovik, Alan Conrad},
  title   = {No-Reference Image Quality Assessment in the Spatial Domain},
  journal = {IEEE Transactions on Image Processing},
  volume  = {21},
  number  = {12},
  pages   = {4695--4708},
  year    = {2012},
  doi     = {10.1109/TIP.2012.2214050}
}

@article{mittal2013niqe,
  author  = {Mittal, Anish and Soundararajan, Rajiv and Bovik, Alan C.},
  title   = {Making a Completely Blind Image Quality Analyzer},
  journal = {IEEE Signal Processing Letters},
  volume  = {20},
  number  = {3},
  pages   = {209--212},
  year    = {2013},
  doi     = {10.1109/LSP.2012.2227726}
}

@inproceedings{venkatanath2015pique,
  author    = {Venkatanath, N. and Praneeth, D. and Maruthi Chandrasekhar, Bh. and Channappayya, Sumohana S. and Medasani, Swarup S.},
  title     = {Blind Image Quality Evaluation Using Perception Based Features},
  booktitle = {2015 Twenty First National Conference on Communications},
  pages     = {1--6},
  year      = {2015},
  doi       = {10.1109/NCC.2015.7084843}
}
```

---

## License

This repository is intended for academic and educational purposes.  
You may adapt, modify, and extend the notebooks according to your research needs.
