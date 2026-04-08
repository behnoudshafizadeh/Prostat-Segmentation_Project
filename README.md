# 🫁 Prostate Segmentation from T2-Weighted MRI Images

A deep learning project for **automatic prostate gland segmentation from T2-weighted MRI scans** using **U-Net with multiple pretrained CNN backbones**.

This project includes:
- MRI volume preprocessing from NIfTI to PNG slices
- binary semantic segmentation masks
- training with 13 different pretrained backbones
- model comparison using IoU and Dice score
- qualitative prediction visualization
- training/validation performance charts

---

## 📋 Table of Contents
- [Overview](#overview)
- [Project Structure](#project-structure)
- [Dataset](#dataset)
- [Dataset Samples](#dataset-samples)
- [Models & Backbones](#models--backbones)
- [Pipeline](#pipeline)
- [Results](#results)
- [Training Procedure Example](#training-procedure-example)
- [Getting Started](#getting-started)
- [Requirements](#requirements)

---

## 📖 Overview

This project performs **binary semantic segmentation of the prostate gland** from **T2-weighted MRI volumes**.

The workflow:
1. Load 3D MRI NIfTI volumes
2. Normalize intensities
3. Convert Z-axis slices into 2D PNG images
4. Train U-Net with multiple pretrained encoders
5. Compare performance across different backbones
6. Visualize segmentation outputs

---

## 📁 Project Structure

```text
Prostat_Segmentation_Project/
│
├── Pre-processing and Visualization.ipynb
├── Evaluation of different Segmentation Models.ipynb
│
├── Prostat Data/
│   ├── img/
│   └── mask/
│
├── Different Models Training and Validation Procedure/
│   ├── densenet121.png
│   ├── resnet50.png
│   └── ...
│
├── output example/
│   ├── prostat0-slice007_z.png
│   ├── Groundtruth0-slice007_z.png
│   ├── predicted0-slice007_z.png
│   └── ...
│
├── Case02_RUNMC.nii
├── Case02_segmentation_RUNMC.nii
└── README.md
```

---

## 🗃️ Dataset

- **Input:** T2-weighted prostate MRI scans
- **Format:** `.nii` → converted to `.png`
- **Slice size:** 384 × 384
- **Mask type:** binary segmentation
- **Dataset structure:** paired `img/` and `mask/` folders
- **Preprocessing:** MinMax normalization
- **Output:** 2D slices along Z-axis

---

## 🖼️ Dataset Samples

### MRI Slice Example
<img src="Prostat Data/img/prostat0-slice007_z.png" width="260"/>

### Corresponding Mask
<img src="Prostat Data/mask/Groundtruth0-slice007_z.png" width="260"/>

These images show one original MRI slice and its corresponding segmentation mask from the dataset.

---

## 🧠 Models & Backbones

The project benchmarks **U-Net** using **13 pretrained encoders**:

### Tested backbones
- VGG16
- ResNet18
- ResNet34
- ResNet50
- DenseNet121
- DenseNet169
- DenseNet201
- EfficientNetB0
- EfficientNetB1
- EfficientNetB2
- EfficientNetB3
- MobileNet
- MobileNetV2

---

## ⚙️ Pipeline

### 1) Preprocessing
- Load NIfTI MRI volumes
- normalize intensity
- convert each slice into PNG
- prepare paired image-mask dataset

### 2) Model Training
- U-Net + pretrained encoder
- Binary Focal Loss + Dice Loss
- IoU and Dice metrics
- train/validation split

### 3) Evaluation
- compare backbones
- visualize predictions
- inspect convergence curves

---

## 📊 Results

### Output Prediction Example

| Original MRI | Ground Truth | Predicted Mask |
|---|---|---|
| <img src="output example/prostat0-slice007_z.png" width="220"/> | <img src="output example/Groundtruth0-slice007_z.png" width="220"/> | <img src="output example/predicted0-slice007_z.png" width="220"/> |

The prediction closely matches the annotated prostate region, showing strong segmentation quality.

---
## 🏆 Backbone Performance Comparison

The project compares multiple pretrained CNN encoders used as **U-Net backbones** for prostate MRI segmentation.

### 📋 Quantitative Comparison

| Backbone | Loss | IoU Score | F1 Score |
|---|---:|---:|---:|
| ResNet18 | 0.1295 | 0.8802 | 0.9363 |
| ResNet34 | 0.1340 | 0.8799 | 0.9361 |
| ResNet50 | 0.1755 | 0.8809 | 0.9367 |
| DenseNet121 | 0.1330 | 0.8905 | 0.9421 |
| DenseNet169 | 0.1346 | 0.8866 | 0.9399 |
| DenseNet201 | **0.0837** | 0.8911 | 0.9424 |
| EfficientNetB0 | 0.1038 | 0.8593 | 0.9243 |
| EfficientNetB1 | 0.0874 | **0.8963** | **0.9453** |
| EfficientNetB2 | **0.0812** | 0.8889 | 0.9412 |
| EfficientNetB3 | 0.1740 | 0.7918 | 0.8838 |
| MobileNet | 0.0912 | 0.8741 | 0.9328 |
| MobileNetV2 | 0.1205 | 0.8383 | 0.9121 |

### 🥇 Best Performing Backbone
Based on the evaluation:

- **Best IoU Score:** EfficientNetB1 → **0.8963**
- **Best F1 Score:** EfficientNetB1 → **0.9453**
- **Lowest Loss:** EfficientNetB2 → **0.0812**

This suggests that **EfficientNetB1 provides the best overall balance between segmentation accuracy and generalization performance** for this dataset.

---
## 📈 Training Procedure Example

Example convergence curve using **DenseNet121 backbone**:

<img src="Different Models Training and Validation Procedure/densenet121.png" width="750"/>

This chart compares:
- training IoU
- validation IoU
- training loss
- validation loss

to evaluate learning stability and generalization.

---

## 🚀 Getting Started

### Clone the project
```bash
git clone https://github.com/YOUR-USERNAME/Prostat_Segmentation_Project.git
cd Prostat_Segmentation_Project
```

### Install requirements
```bash
pip install tensorflow segmentation-models nibabel opencv-python numpy scikit-learn matplotlib
```

### Run preprocessing
Open:
```bash
Pre-processing and Visualization.ipynb
```

### Run model training
Open:
```bash
Evaluation of different Segmentation Models.ipynb
```

---

## 📦 Requirements

- Python 3.8+
- TensorFlow / Keras
- segmentation-models
- nibabel
- OpenCV
- NumPy
- scikit-learn
- matplotlib

---

## 🎯 Key Skills Demonstrated

- Medical image segmentation
- MRI preprocessing
- NIfTI handling
- U-Net architecture
- transfer learning
- pretrained CNN encoders
- IoU / Dice evaluation
- qualitative visualization
- deep learning experimentation
- model benchmarking

---

## 📄 License

This project was developed as part of an academic medical imaging assignment.
