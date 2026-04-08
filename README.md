# 🫁 Prostate Segmentation from T2-Weighted MRI Images

A deep learning project for automatic prostate gland segmentation in MRI scans using **U-Net** with multiple CNN backbones. Built with TensorFlow/Keras and the `segmentation_models` library.

> **Authors:** Behnoud Shafiezadeh Kenari (ID: 5655740) · Erfan Fathi (ID: 5652154)

---

## 📋 Table of Contents

- [Overview](#overview)
- [Project Structure](#project-structure)
- [Dataset](#dataset)
- [Models & Backbones](#models--backbones)
- [Pipeline](#pipeline)
- [Results](#results)
- [Getting Started](#getting-started)
- [Requirements](#requirements)

---

## Overview

This project tackles **binary semantic segmentation** of the prostate gland from T2-weighted MRI volumes (NIfTI format). The pipeline covers:

1. Loading and normalizing 3D MRI volumes
2. Slicing volumes along the Z-axis into 2D PNG images
3. Training a **U-Net** model with various pretrained CNN backbones
4. Evaluating performance using **IoU Score** and **F-Score (Dice)**
5. Comparing 13 different backbone architectures

---

## 📁 Project Structure

```
Prostat_Segmentation_Project/
│
├── Pre-processing and Visualization.ipynb      # Step 1: NIfTI → PNG slices
├── Evaluation of different Segmentation Models.ipynb  # Step 2: Train & evaluate U-Net
│
├── Prostat Data/
│   ├── img/          # 2D PNG slices of MRI images
│   └── mask/         # 2D PNG slices of segmentation masks
│
├── Different Models Training and Validation Procedure/
│   ├── resnet50.png
│   ├── densenet121.png
│   ├── vgg16.png
│   └── ...           # Training/validation curves for each backbone
│
├── output example/
│   ├── prostat*      # Original MRI slices
│   ├── Groundtruth*  # Ground truth masks
│   └── predicted*    # Model predictions
│
├── Case02_RUNMC.nii              # Sample raw MRI volume
├── Case02_segmentation_RUNMC.nii # Sample segmentation mask
│
├── Prostate segmentation from T2-weighted MRI-images_Requirements of Project.pdf
├── Report of Project.pdf
└── Report of Project_Results.pdf
```

---

## 🗃️ Dataset

- **Format:** NIfTI (`.nii`) 3D MRI volumes → converted to 2D PNG slices
- **Image size:** 384 × 384 pixels
- **Cases:** 30 patients (Cases 00–29), from two centers: **BMC** and **RUNMC**
- **Total slices:** ~1,500+ images with paired masks
- **Mask values:** 0 (background), 1 (peripheral zone), 2 (transition zone) → binarized to 0/1
- **Intensity range:** Normalized to [0, 1] using MinMaxScaler

---

## 🧠 Models & Backbones

U-Net architecture tested with **13 different pretrained backbones**:

| Family | Backbones |
|---|---|
| VGG | VGG16 |
| ResNet | ResNet18, ResNet34, ResNet50 |
| DenseNet | DenseNet121, DenseNet169, DenseNet201 |
| EfficientNet | EfficientNetB0, B1, B2, B3 |
| MobileNet | MobileNet, MobileNetV2 |

---

## ⚙️ Pipeline

### Step 1 — Pre-processing (`Pre-processing and Visualization.ipynb`)

```python
# Load NIfTI volume
img = nib.load('Case02_RUNMC.nii').get_fdata()   # shape: (384, 384, 24)

# Normalize
img = MinMaxScaler().fit_transform(img.reshape(-1, img.shape[-1])).reshape(img.shape)

# Slice and save as PNG
for i in range(dimz):
    cv2.imwrite(f'prostat0-slice{i:03d}_z.png', np.uint8(img[:,:,i] * 255))
```

### Step 2 — Training (`Evaluation of different Segmentation Models.ipynb`)

```python
import segmentation_models as sm

BACKBONE = 'resnet50'   # choose any backbone
model = sm.Unet(BACKBONE, classes=1, activation='sigmoid')

# Loss: Dice + Focal
total_loss = sm.losses.DiceLoss() + sm.losses.BinaryFocalLoss()

# Metrics
metrics = [sm.metrics.IOUScore(threshold=0.5), sm.metrics.FScore(threshold=0.5)]

model.compile(Adam(lr=0.0001), total_loss, metrics)
model.fit(x_train, y_train, batch_size=4, epochs=20, validation_data=(x_val, y_val))
```

---

## 📊 Results

Training and validation curves for all 13 backbones are saved in `Different Models Training and Validation Procedure/`. Example output comparisons (original / ground truth / prediction) are in `output example/`.

| Metric | Description |
|---|---|
| **IoU Score** | Intersection over Union (threshold = 0.5) |
| **F-Score** | Dice coefficient (threshold = 0.5) |
| **Loss** | Dice Loss + Binary Focal Loss |

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/Prostat_Segmentation_Project.git
cd Prostat_Segmentation_Project
```

### 2. Install dependencies

```bash
pip install nibabel opencv-python scikit-learn segmentation-models tensorflow
```

### 3. Run Pre-processing

Open and run all cells in:
```
Pre-processing and Visualization.ipynb
```
> ⚠️ Update the data paths at the top of the notebook to match your local directory.

### 4. Train a model

Open and run:
```
Evaluation of different Segmentation Models.ipynb
```

To switch backbone, change this line:
```python
BACKBONE = 'resnet50'   # options: vgg16, resnet18, densenet121, efficientnetb0, mobilenet, etc.
```

---

## 📦 Requirements

```
python >= 3.8
tensorflow >= 2.x
keras
segmentation-models
nibabel
opencv-python
numpy
scikit-learn
matplotlib
```

Install all at once:
```bash
pip install tensorflow segmentation-models nibabel opencv-python numpy scikit-learn matplotlib
```

> ⚠️ `segmentation-models` requires setting the framework before import:
> ```python
> import os
> os.environ["SM_FRAMEWORK"] = "tf.keras"
> import segmentation_models as sm
> ```

---

## 📄 License

This project was developed as part of a university course assignment.
