# Automatic Image Colorization

This project implements an automatic image colorization system based on the approach described in our project report.

The system takes a grayscale landscape image as input and tries to restore its colors using Support Vector Regression (SVR) and Markov Random Field (MRF) based smoothing.

## Methodology

The overall process is:

**RGB image → YUV → Y channel → SLIC superpixels → 10×10 patches → 2D FFT features → SVR → MRF/ICM → RGB image**

The image is first converted from RGB to YUV. The Y channel contains the luminance information, which is used as the input to the model.

SLIC is then used to divide the image into superpixels. For each superpixel, a 10×10 patch is taken around its centroid and 2D FFT features are extracted from it.

Two separate SVR models are used to predict the U and V chrominance values. Finally, MRF-based smoothing using ICM is applied to make the predicted colors more consistent between neighboring superpixels before converting the result back to RGB.

## Project Structure

```text
automatic_image_colorization/
│
├── data/
│   ├── train/              # Color training images
│   ├── test/               # Grayscale test images
│   └── test_original/      # Original color images for comparison
│
├── models/
│   ├── u_svr.joblib        # U chrominance SVR model
│   └── v_svr.joblib        # V chrominance SVR model
│
├── outputs/
│   └── evaluation/         # Colorized test images
│
├── results/
│   └── metrics.csv         # Evaluation results
│
├── colorize.py             # Colorizes a grayscale image
├── train.py                # Trains the SVR models
├── evaluate.py             # Evaluates the colorized images
├── utils.py                # Image processing and feature extraction
├── download_dataset.py     # Downloads and prepares the dataset
├── requirements.txt        # Required Python packages
└── README.md

## Results

The final model was evaluated on 150 test images.

The average results obtained were:

| Metric | Result |
|---|---:|
| MAE | 0.0463 |
| MSE | 0.0058 |
| PSNR | 25.18 dB |
| SSIM | 0.9496 |

The results show that the model is able to recover a significant amount of the color and visual structure from the grayscale input. However, the predicted colors are not always identical to the original image, especially in regions with more complex or less common colors.