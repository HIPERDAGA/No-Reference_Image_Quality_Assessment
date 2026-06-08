# BRISQUE Step-by-Step Image Quality Assessment

This repository contains a Google Colab notebook that explains and implements the step-by-step process of the **BRISQUE** no-reference image quality assessment metric.

BRISQUE stands for **Blind/Referenceless Image Spatial Quality Evaluator**. It is a no-reference image quality metric based on **Natural Scene Statistics (NSS)** in the spatial domain. Unlike full-reference methods, BRISQUE does not require a pristine reference image to estimate perceptual quality.

The notebook allows users to upload an image from their computer and visualize each stage of the BRISQUE feature extraction process until obtaining the final BRISQUE quality score.

---

## Open in Google Colab

Replace `USER` and `REPOSITORY` with your GitHub username and repository name:

```markdown
[Open in Colab](https://colab.research.google.com/github/USER/REPOSITORY/blob/main/BRISQUE_step_by_step_Colab.ipynb)
```

---

## Repository contents

```text
.
├── BRISQUE_step_by_step_Colab.ipynb
├── README.md
└── figures/
```

The main file is:

```text
BRISQUE_step_by_step_Colab.ipynb
```

This notebook performs the complete BRISQUE workflow and displays the intermediate images, maps, histograms, features, and final quality score.

---

## Main objective

The objective of this notebook is to provide a clear and visual explanation of how BRISQUE estimates image quality without using a reference image.

The notebook shows how an input image is transformed into statistical features based on natural image behavior. These features are then used to estimate the perceptual quality of the image.

---

## BRISQUE workflow

The complete process can be summarized as follows:

```text
Image
→ Grayscale conversion
→ MSCN coefficients
→ GGD feature extraction
→ Pairwise products
→ AGGD feature extraction
→ Two-scale feature extraction
→ 36-dimensional feature vector
→ Regression model
→ Final BRISQUE score
```

---

## What the notebook does

The notebook includes the following steps:

1. Upload an image from the local computer.
2. Display the original input image.
3. Convert the image to grayscale/luminance.
4. Compute **Mean Subtracted Contrast Normalized (MSCN)** coefficients.
5. Display the local mean, local standard deviation, and MSCN map.
6. Fit a **Generalized Gaussian Distribution (GGD)** to the MSCN coefficients.
7. Plot the MSCN histogram and the fitted GGD curve.
8. Compute pairwise products of neighboring MSCN coefficients in four directions:

   * Horizontal
   * Vertical
   * Main diagonal
   * Secondary diagonal
9. Extract **AGGD** statistical features from the pairwise products.
10. Repeat the process at two image scales.
11. Generate the final **36-dimensional BRISQUE feature vector**.
12. Compute the final BRISQUE score using OpenCV's pretrained BRISQUE model.

---

## MSCN coefficients

One of the most important preprocessing steps in BRISQUE is the computation of MSCN coefficients:

```latex
\hat{I}(i,j)=\frac{I(i,j)-\mu(i,j)}{\sigma(i,j)+C}
```

where:

* `I(i,j)` is the luminance value of the pixel.
* `μ(i,j)` is the local mean.
* `σ(i,j)` is the local standard deviation.
* `C` is a constant used to avoid division by zero.

This normalization reduces local luminance and contrast variations, making it easier to analyze the statistical regularities of the image.

---

## BRISQUE features

BRISQUE extracts statistical features from:

1. The distribution of MSCN coefficients.
2. The pairwise products of neighboring MSCN coefficients.

At one image scale, BRISQUE extracts:

```text
2 GGD features + 16 AGGD features = 18 features
```

Since the process is repeated at two scales:

```text
18 features × 2 scales = 36 features
```

These 36 features are used by a trained regression model to estimate the perceptual quality score.

---

## BRISQUE score interpretation

The final output is a BRISQUE score.

In general:

```text
Lower BRISQUE score  → better perceptual image quality
Higher BRISQUE score → stronger perceptual degradation
```

Approximate interpretation:

| BRISQUE score | Interpretation             |
| ------------- | -------------------------- |
| 0 – 20        | Very good quality          |
| 20 – 40       | Good or acceptable quality |
| 40 – 60       | Moderate degradation       |
| 60 – 100      | Strong degradation         |

These ranges are approximate and may vary depending on the dataset, image content, and environmental conditions.

---

## Requirements

The notebook installs the required packages automatically in Google Colab:

```python
opencv-contrib-python-headless
scipy
scikit-image
pandas
matplotlib
numpy
```

No local installation is required if the notebook is executed in Google Colab.

---

## How to use

1. Open the notebook in Google Colab.
2. Run the first cell to install the dependencies.
3. Upload an image from your computer.
4. Run the notebook cells in order.
5. Review the intermediate visualizations:

   * Grayscale image
   * MSCN map
   * GGD histogram
   * Pairwise product maps
   * AGGD features
   * 36-feature vector
6. Check the final BRISQUE score.

---

## Applications

This notebook can be useful for:

* Understanding no-reference image quality assessment.
* Explaining BRISQUE in academic projects.
* Evaluating image degradation without a reference image.
* Comparing image quality before and after preprocessing.
* Supporting computer vision pipelines under adverse conditions such as fog, rain, sandstorm, blur, noise, or low illumination.

---

## Reference

The BRISQUE method is based on the following paper:

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
```

---

## License

This repository is intended for academic and educational purposes.
You may adapt and extend the notebook according to your research needs.
