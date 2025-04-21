# Segmentation and Tumor Detection in Medical Imaging

This project implements a three‑stage Python pipeline for nuclei segmentation and tumor detection in high‑resolution histopathology images. All steps rely on classical computer‑vision and machine‑learning libraries, without using deep‑learning models.

---

## 1. Data Pipeline

In this stage, raw RGB images, corresponding mask images, and annotation CSV files are organized and inspected. A few sample images are plotted alongside their ground‑truth masks and overlaid bounding‑box labels. The pipeline then enumerates unique patient IDs from filenames and splits them into training (80 %) and validation (20 %) sets, saving these lists for downstream processing.

---

## 2. Image Segmentation

### 2.1 Unsupervised Segmentation

Each training image is clustered into background and nuclei regions using K‑means and Mean‑Shift. Different values of K (e.g. 3 and 5) are tested, and the Dice coefficient between the predicted mask and ground truth guides the choice of optimal K. The original image, the best‑clustered output, and the true binary mask are visualized side by side for qualitative comparison.

### 2.2 Supervised Segmentation

Images are converted to grayscale and cast as a pixel‑classification problem. Per‑pixel intensity and texture features are extracted, and a Random Forest classifier is trained to assign each pixel to its correct nuclei class. Performance metrics (accuracy and Dice score) are reported on both training and validation splits, and supervised results are compared against the unsupervised masks on a few sample images.

---

## 3. Tumor Detection & Counting

### 3.1 Feature Extraction

For each segmented nucleus region, background noise is removed and histogram‑of‑oriented‑gradient (HOG) descriptors are computed. This transforms each candidate blob into a fixed‑length feature vector representing local shape and texture.

### 3.2 Classification Framework

A Support Vector Machine (SVM) is trained on the extracted HOG features to distinguish tumor nuclei from non‑tumor. The trained model is applied to validation images to detect tumors and count their instances. Detection accuracy metrics and the root‑mean‑squared error (RMSE) of tumor counts are reported to quantify performance.
