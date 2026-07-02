# 🖼️ CIFAR-100 Image Classification — Deep Vision

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/christianirshool-glitch/cifar100-image-classification/blob/main/ET_4.ipynb)
![TensorFlow](https://img.shields.io/badge/Engine-TensorFlow%2FKeras-orange?style=flat-square&logo=tensorflow)
![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter&logoColor=white)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

Deep learning project focused on image classification using the **CIFAR-100** dataset (100 classes, 60,000 images). Two strategies are designed, trained and compared: **Transfer Learning + Fine-Tuning** (EfficientNetB0 and ConvNeXtTiny) and a **CNN trained from scratch**. The main notebook `ET_4.ipynb` contains the code and results.

---

## 📌 Table of contents

- [Context](#-context)
- [Objective](#-objective)
- [Dataset](#-dataset)
- [Methodology](#-methodology)
- [Developed models](#-developed-models)
- [Results](#-results)
- [Tech Stack](#-tech-stack)
- [How to run](#-how-to-run)
- [Project structure](#-project-structure)
- [Author](#-author)
- [License](#-license)

---

## 📌 Context

CIFAR-100 is a widely used benchmark in computer vision. With 100 classes grouped into 20 superclasses and only 600 images per class (32×32 px), it poses a realistic generalization challenge for deep learning architectures.

The low spatial resolution imposes architectural constraints: some networks (e.g., Xception, InceptionV3) were discarded due to minimum input size requirements.

---

## 🎯 Objective

Design and compare two image classification strategies:

- Leverage pretrained ImageNet models with Transfer Learning and Fine-Tuning.
- Design and train a custom CNN from scratch with empirical justification for architectural choices.
- Evaluate and compare both approaches on the CIFAR-100 test set.

---

## 📊 Dataset

| Feature | Detail |
|---|---|
| Dataset | [CIFAR-100](https://keras.io/api/datasets/cifar100/) |
| Number of classes | 100 (grouped in 20 superclasses) |
| Training images | 50,000 |
| Test images | 10,000 |
| Resolution | 32×32 pixels · RGB |

---

## 🔧 Methodology

### 1. Data preparation
- Load and normalize the dataset from `tf.keras.datasets`.
- One-hot encode labels.
- Apply architecture-specific preprocessing (`preprocess_input`).

### 2. Exploratory analysis
- Visualize samples per class.
- Analyze class distribution.

### 3. Modeling

| Notebook | Approach |
|---|---|
| `ET_4.ipynb` — Strategy 1 | Transfer Learning + Fine-Tuning (EfficientNetB0, ConvNeXtTiny) |
| `ET_4.ipynb` — Strategy 2 | Custom CNN trained from scratch |

### 4. Optimization
Common techniques applied:
- Weight regularization (L2)
- Progressive dropout (0.25 in conv layers, 0.5 in dense layers)
- Batch normalization
- Data augmentation
- Label smoothing (strategy 2)
- Callbacks: `ReduceLROnPlateau`, `EarlyStopping`

### 5. Evaluation
- Training and validation accuracy / loss.
- `classification_report` on test set.
- Learning curves (loss/accuracy per epoch).

---

## 🧠 Developed models

### 🔹 Strategy 1: Transfer Learning + Fine-Tuning

Several ImageNet pretrained architectures were evaluated. Xception and InceptionV3 were discarded due to input size constraints. The two finalists were:

**EfficientNetB0** (robust choice):
- Good balance with less overfitting.
- Train/val gap: ~42% / ~34%.
- Top model: `GlobalAveragePooling2D → Dense(512, relu) → Dense(100, softmax)`.

**ConvNeXtTiny** (best final accuracy):
- Modern convolutional design inspired by Transformers.
- Achieved ~61.90% train accuracy and ~52.70% validation accuracy after 20 epochs.

### 🔹 Strategy 2: Custom CNN from scratch
- Sequential architecture with blocks `Conv2D(3×3) → BatchNorm → Activation → MaxPooling → Dropout`.
- Increasing filters: 32 → 64 → 128.
- `GlobalAveragePooling2D` used instead of `Flatten` to reduce parameters.
- Achieved a ceiling around ~61-62% accuracy.

---

## 📈 Results

| Model | Train Accuracy | Val Accuracy |
|---|---|---|
| EfficientNetB0 (Transfer Learning) | ~42% | ~34% |
| ConvNeXtTiny (Transfer Learning) | ~61.90% | ~52.70% |
| Custom CNN (from scratch) | ~61-62% | — |

> **Conclusion:** Transfer Learning with ConvNeXtTiny yielded the best absolute accuracy. EfficientNetB0 provided better balance and less overfitting. The custom CNN demonstrated the value of a compact design adapted to low-resolution images.

---

## 🛠️ Tech Stack

| Tool | Use |
|---|---|
| `TensorFlow / Keras` | Model definition, training and evaluation |
| `scikit-learn` | Evaluation metrics (`classification_report`) |
| `NumPy` | Numeric operations and arrays |
| `Matplotlib` | Visualizations |
| `Google Colab` | Execution environment with GPU |

---

## 🚀 How to run

The notebook is ready to run on **Google Colaboratory**.

```bash
# Option 1: Open in Colab using the badge above

# Option 2: Clone the repository

git clone https://github.com/christianirshool-glitch/cifar100-image-classification.git
cd cifar100-image-classification

# Install dependencies
pip install tensorflow scikit-learn matplotlib

# Run the notebook locally
jupyter notebook ET_4.ipynb
```

> ⚠️ GPU recommended (e.g., Colab) to reduce training time.

---

## 📁 Project structure
cifar100-image-classification/
├── ET_4.ipynb        # Main notebook (both strategies)
├── LICENSE           # MIT License
├── README.md

---

## 👤 Author

**Christian Méndez Giraldo**  
Data Scientist · MSc in Data Science & AI  
[GitHub](https://github.com/christianirshool-glitch)

---

## 📄 License

This project is licensed under the **MIT License** — see [LICENSE](LICENSE) for details.
