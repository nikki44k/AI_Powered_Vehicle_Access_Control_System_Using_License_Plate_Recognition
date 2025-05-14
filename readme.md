# AI-Powered Vehicle Access Control System Using License Plate Recognition

**Author:** Nikhitha
**Course:** CS 455 Spring 2025
**Date:** May 2025

---

## 1. Overview

This project implements an end-to-end vehicle access control system using computer vision and deep learning. It leverages custom-trained YOLOv8 models for license plate detection and Tesseract OCR for character recognition, integrates access-control logic against a predefined allowlist, and provides an interactive Streamlit UI for demonstration.

## 2. Project Structure

```
project455/
├── data/                            # input data and configs
│   ├── allowed_plates.csv           # CSV of authorized plate numbers
│   ├── kaggle/                      # original dataset zip + folders
│   │   ├── indian-license-plates-with-labels.zip
│   │   ├── images/                  # raw images extracted from zip
│   │   └── labels/                  # YOLO-style txt annotations extracted from zip
│   ├── plate.yaml                   # YOLOv8 dataset configuration
│   └── test_images/                 # additional images for manual testing
│
├── datasets/                        # train/valid/test splits for YOLO
│   ├── train/
│   ├── valid/
│   └── test/
│
├── model/                           # saved model weights
│   └── yolov8_license_plate_model.pt
│
├── notebooks/                       # Jupyter notebooks for each phase
│   ├── 1_data_preprocessing.ipynb          # explore & raw OCR on full images
│   ├── 2_yolov8_training_ocr_integration.ipynb  # split data, train YOLOv8, integrate OCR
│   └── 3_access_control_logic.ipynb       # verify extracted plates against allowlist
│
├── ui/                              # Streamlit web application
│   └── app.py                        # interactive demo of detection → OCR → access logic
│
├── requirements.txt                 # project dependencies
└── README.md                        # this file
```

## 3. Setup

1. Clone or copy this repository and navigate to its root:

   ```bash
   cd project455
   ```

2. Extract the Kaggle dataset ZIP into `data/kaggle/` so you have:

   ```
   data/kaggle/images/    # .jpg/.png images
   data/kaggle/labels/    # YOLO .txt annotation files
   ```

3. Create and activate a Python 3.7+ virtual environment:

   ```bash
   python -m venv venv
   # Linux/Mac
   source venv/bin/activate
   # Windows PowerShell
   .\venv\Scripts\Activate.ps1
   ```

4. Install dependencies:

   ```bash
   pip install -r requirements.txt
   ```

5. Install Tesseract OCR engine on your system and ensure it’s on your PATH (or set `pytesseract.pytesseract.tesseract_cmd` in code to its executable path).

## 4. Notebooks

1. **Data Preprocessing** (`notebooks/1_data_preprocessing.ipynb`)

   * Lists and displays sample images.
   * Runs raw Tesseract OCR on full images to establish a baseline.

2. **YOLOv8 Training & OCR Integration** (`notebooks/2_yolov8_training_ocr_integration.ipynb`)

   * Copies/extracts Kaggle data into `datasets/` splits (train/valid/test).
   * Generates `data/plate.yaml` for YOLOv8.
   * Trains a YOLOv8 model (base `yolov8n.pt`) and saves weights to `model/yolov8_license_plate_model.pt`.
   * Demonstrates detection → crop → OCR for sample test images.

3. **Access-Control Logic** (`notebooks/3_access_control_logic.ipynb`)

   * Loads an allowlist from `data/allowed_plates.csv`.
   * Defines `verify_plate()` to compare cleaned OCR output against the allowlist.
   * Runs end-to-end: displays each cropped plate, OCR text, and a ✅ Access Granted or ❌ Access Denied decision.

## 5. Streamlit UI

The Streamlit app (`ui/app.py`) provides an interactive web interface:

* Upload a vehicle image.
* View detected plate crops and OCR results.
* See immediate grant/deny verdicts.

Run the app:

```bash
streamlit run ui/app.py
```

## 6. Sources

* **Dataset:** "Indian License Plates with Labels" by kedarsai on Kaggle.
* **Model Framework:** YOLOv8 by Ultralytics ([https://ultralytics.com/](https://ultralytics.com/)).
* **OCR Engine:** Tesseract OCR by Google ([https://github.com/tesseract-ocr/tesseract](https://github.com/tesseract-ocr/tesseract)).
* **Visualization & UI:** Streamlit ([https://streamlit.io/](https://streamlit.io/)).
* **Development Assistance:** ChatGPT (OpenAI) helped refine the pipeline and code structure.
