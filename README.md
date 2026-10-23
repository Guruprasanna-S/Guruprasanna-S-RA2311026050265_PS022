# Knee Osteoarthritis Severity Grading (PS-22)

**Name:** Guruprasanna S  
**Register No:** RA2311026050265  
**Section:** AIML B  
**Domain:** Healthcare & Medical Imaging

## Problem
Grade knee X-rays on the Kellgren–Lawrence (KL) scale from 0 (healthy) to 4 (severe). Neighbouring grades look very similar, so along with accuracy this project reports how many errors are off by only one grade.

## Dataset
Knee Osteoarthritis Severity Grading Dataset (Mendeley Data, from the OAI cohort)  
Link: https://data.mendeley.com/datasets/56rmx5bjcr  
- 5 classes (KL-0 to KL-4), 224×224 grayscale knee images
- 8,260 images split 70/10/20: 5,778 train, 826 validation, 1,656 test
- Split is by patient ID (seed = 42), so no patient appears in more than one split

## Method
- Pre-processing: CLAHE contrast enhancement, ImageNet normalisation, light augmentation (flip, rotation, zoom)
- Model: DenseNet-161 pretrained on ImageNet, compared with a 4-block CNN trained from scratch
- Loss: cost-sensitive ordinal penalty loss, so far-off errors cost more than one-grade errors
- Training: 25 epochs, batch size 32, AdamW, 2 stages (head warm-up with the backbone frozen, then full fine-tuning)

## Results (test set, N = 1,656)
| Model | Accuracy | Macro F1 | QWK |
|---|---|---|---|
| 4-block CNN (scratch) | 64.25% | 0.5842 | 0.7814 |
| DenseNet-161 + Cross-Entropy | 72.10% | 0.7085 | 0.8540 |
| **DenseNet-161 + CLAHE + Ordinal Loss** | **77.36%** | **0.7650** | **0.8962** |

- Within-1-grade accuracy: **97.64%**
- 336 of 375 errors (89.60%) are off by only one grade, and none are off by 3 or more grades

## How to run
1. Open `YOUR_NOTEBOOK_NAME.ipynb` in Google Colab.
2. Download the dataset from the link above and set the dataset path in the first cells.
3. Run all cells in order. The seed is set to 42 for reproducibility.

## Requirements
Python 3, PyTorch, torchvision, OpenCV, NumPy, pandas, scikit-learn, matplotlib

## Repository
https://github.com/Guruprasanna-S/Guruprasanna-S-RA2311026050265_PS022

## References
- Chen, P. (2018), Knee Osteoarthritis Severity Grading Dataset, Mendeley Data, V1, doi:10.17632/56rmx5bjcr.1
- Pi, S.-W., et al. (2023), Ensemble deep-learning networks for automated OA grading in knee X-ray images, Sci. Rep., 13, 22887
