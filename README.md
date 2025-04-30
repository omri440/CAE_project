# 🧠 CAN Bus Anomaly Detection using CAE (Convolutional Autoencoder)

This project implements a full pipeline to preprocess CAN bus logs, transform them into image-like inputs, train a simple autoencoder to reconstruct normal behavior, and detect anomalies based on reconstruction error.

---

## 📁 Dataset Structure

This project uses CSV-formatted CAN bus logs for several attack types:

- DoS
- Fuzzy
- RPM
- Gear
- Parsed (mixed)

Each CSV file is converted into a windowed structure, then serialized into TensorFlow `.tfrecord` format with binary labels:
- `0` = Normal
- `1` = Attack

---

## 🚀 Pipeline Overview

### 1. Preprocessing
- Extract CAN ID bits and Data bytes
- Reshape last row bits into `8x8` and resize to `29x29`
- Stack with CAN ID bits into `2-channel` pseudo-image

### 2. TFRecord Generation
- Windows of 29 messages
- Binary labels (attack if any row in window is attack)
- Data saved into `*_train.tfrecord`, `*_val.tfrecord`, `*_test.tfrecord`

### 3. Model
- Simple feedforward autoencoder
- Trained only on normal data
- Trained with early stopping and batch normalization

### 4. Evaluation
- Reconstruction error used as anomaly score
- ROC curve and Youden's J statistic used for optimal threshold
- Final metrics: confusion matrix + classification report

---

## 🔧 Requirements

- Python 3.8+
- TensorFlow 2.x
- NumPy, Pandas, OpenCV, scikit-learn, tqdm

---

## 📊 Outputs

- ROC AUC
- Optimal threshold
- Confusion Matrix
- Precision / Recall / F1 report

---

## 📎 Notes

- No convolution is used (images treated as flattened features)
- Training only on Normal behavior
- All attack types tested separately during evaluation

---

## 🧪 How to Use

1. Place all your CSV datasets in the current working directory
2. Run the notebook or convert to `.py`
3. Check for generated `*.tfrecord` files
4. Results will be printed in the final cell

---

## 📁 File Breakdown

- `cae_last_26_04.ipynb` — full preprocessing, training and evaluation pipeline
- `*.csv` — original CAN logs
- `*.tfrecord` — TF serialized training data
- `simple_autoencoder.keras` — trained model file

---

## 🧠 Author

Developed in Google Colab with full GPU acceleration.

---

## 📅 Last updated: April 26, 2025
