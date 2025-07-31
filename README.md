# One-Shot Facial Recognition using Siamese Neural Networks on LFW Dataset

This project implements a one-shot facial recognition system using **Siamese Neural Networks**, trained and evaluated on the **LFW-a (Labeled Faces in the Wild - aligned)** dataset. The network is inspired by Koch et al.'s 2015 paper: *"Siamese Neural Networks for One-shot Image Recognition"*, and is designed to verify whether two facial images depict the same person—even when only one example per identity is available.

---

## 📂 Project Overview

-  **One-shot Learning**: Works with extremely limited training samples per identity.
-  **Siamese Network Architecture**: Learns similarity between image pairs instead of categorical classification.
-  **Hyperparameter Tuning**: Utilizes [Optuna](https://optuna.org/) for automated, large-scale search.
-  **Data Augmentation**: Improves model robustness using controlled transforms.
-  **Real-world Evaluation**: Tested on LFW, a challenging facial dataset with high variation.

---

## 📊 Dataset: LFW-a

- **Source**: Aligned version of the LFW dataset.
- **Samples**: 13,233 facial images.
- **Pairs**: CSV-based file structure defining image pairs labeled as *same* (1) or *different* (0).
- **Train/Validation/Test Split**: Stratified and balanced, with 20% held out for validation.

---

## 🏗️ Model Architecture

| Component           | Description                                                                 |
|--------------------|-----------------------------------------------------------------------------|
| Input              | Grayscale images (105x105, 160x160, or 200x200)                             |
| Conv Layers        | 4 convolutional blocks with ReLU + MaxPooling (kernels: 10, 7, 4, 4)         |
| Fully Connected    | 4096-unit dense layer with optional Dropout followed by a scalar output     |
| Output             | Absolute difference between embeddings → sigmoid → similarity score         |

---

## 🛠️ Preprocessing & Augmentation

- **Cropping**: Removes 30px border to reduce noise.
- **Resizing**: Evaluated across 105×105, 160×160, and 200×200.
- **Normalization**: Pixel values scaled to [-1, 1].
- **Augmentations (Train only)**:
  - `RandomHorizontalFlip`
  - `RandomRotation(±10°)`

---

## ⚙️ Hyperparameter Tuning (via Optuna)

| Parameter         | Range / Choices                           |
|------------------|-------------------------------------------|
| Image Size       | 105x105, 160x160, 200x200                  |
| Learning Rate    | Log-uniform [1e-6, 1e-3]                   |
| Weight Decay     | Log-uniform [1e-6, 1e-2]                   |
| Dropout Rate     | Uniform [0.0, 0.5]                         |
| Optimizer        | Adam, AdamW, RMSprop, SGD                 |
| Batch Size       | 32                                        |
| Loss Function    | Binary Cross Entropy                      |
| Regularization   | L1                                        |

---

## 🧪 Results

| Resize | Train Acc | Val Acc | Test Acc | Train Loss | Val Loss | Epochs |
|--------|-----------|---------|----------|------------|----------|--------|
|105×105 | 64.66%    | 63.64%  | 65.80%   | 0.6194     | 0.6242   | 36     |
|160×160 | 75.91%    | 67.73%  | 68.00%   | 0.5065     | 0.5681   | 25     |
|200×200 | 82.10%    | 71.82%  | 68.50%   | 0.4185     | 0.5637   | 25     |

- ⏱️ Early stopping patience: 20 epochs
- 📉 Best performance obtained at 200×200 input size, with fastest convergence and highest generalization.

---

## 🔍 Comparison with Koch et al. (2015)

| Feature                | Koch et al.              | Our Implementation             |
|------------------------|--------------------------|--------------------------------|
| Dataset                | Omniglot                 | LFW-a                          |
| Input Image Size       | 105×105 RGB              | 105×105 to 200×200 grayscale   |
| Optimizer              | SGD                      | Adam, AdamW, RMSprop, SGD      |
| Pretrained Models      | Yes (in later works)     | No (trained from scratch)      |
| Accuracy (Best)        | ~92% (Omniglot)          | ~68.5% (LFW-a)                 |

---

## 🧠 Future Improvements

- ✅ Use pretrained backbones like **FaceNet** or **VGGFace**.
- 🔄 Add **Batch Normalization** between Conv layers.
- 🎛️ Explore **stochastic data augmentation** for variability.
- 🧪 Tune **dropout and regularization** more aggressively for high-res inputs.

---

## 📁 Files

- `code.ipynb` — Full implementation and training pipeline.
- `report.pdf` — Formal write-up with detailed experiments, results, and analysis.

---

## 👥 Authors

- Gil Ari Agmon 
- Shir Rozenfeld 

Project submitted as part of **Deep Learning – Assignment 2**, Ben-Gurion University of the Negev.

---

## 🛡️ License

This project is for academic use only.
