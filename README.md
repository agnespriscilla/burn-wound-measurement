# Burn Wound Detection & Measurement

> Medical image processing application for automated burn wound area measurement · Machine Learning & Computer Vision · Forked from [nadyasaraswati/burn-wound-measurement](https://github.com/nadyasaraswati/burn-wound-measurement)

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=flat&logo=streamlit&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-5C3EE8?style=flat&logo=opencv&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat&logo=numpy&logoColor=white)

---

## Overview

A web-based application built with Streamlit that automatically detects and measures burn wound area from medical images. The system uses a **coin as a physical reference** for real-world scale calibration, enabling accurate measurements in cm².

The app supports three processing modes from traditional image processing pipelines to AI-powered detection via the Roboflow API making it flexible for both research and clinical use cases.

---

## Features

### Three Processing Modes

**Full Pipeline** : Upload a single image with a reference coin. Automatically segments the wound using selectable methods and outputs measurements in real-time.

**Exact Batch** : Upload the original image + pre-generated mask separately. Produces results identical to batch processing in Google Colab.

**Roboflow AI** : Connect your custom Roboflow model via API for AI-powered wound detection and segmentation.

### Segmentation Methods
| Method | Description |
|---|---|
| Hole Filled *(Recommended)* | HSV + Otsu + morphological ops + flood fill |
| Binary HSV | HSV-based Otsu thresholding |
| Binary Gray | Grayscale Otsu thresholding |
| Opening | Morphological opening (noise removal) |
| Closing | Morphological closing (gap filling) |

### Measurement Output
- **Area** (cm²) : wound surface area
- **Perimeter** (cm) : wound boundary length
- **Bounding box** : width × height in cm
- **Ratio** : wound area relative to the reference coin area

### Export Options
- Annotated result image (PNG download)
- Measurement data as JSON
- Measurement data as CSV

---

## How It Works

```
Input Image (with coin in corner)
          ↓
  Resize to 512×512
          ↓
  Coin Detection (HSV color → circular detection → pixels/cm calibration)
          ↓
  Wound Segmentation
  ├── HSV Thresholding (Otsu)
  ├── Morphological Processing (Opening → Closing → Hole Fill)
  └── Contour Extraction
          ↓
  Area & Measurement Calculation
          ↓
  Annotated Visualization + Export
```

**Coin calibration:** The app automatically detects a gold-colored coin placed at any corner of the image using HSV color segmentation and circularity detection, then computes a `pixels/cm` ratio for accurate real-world measurement.

---

## Repository Structure

```
burn-wound-measurement/
│
├── app.py                          ← Main Streamlit application (964 lines)
├── requirements.txt                ← Python dependencies
│
├── [FIX]step_1_preprocessing.ipynb ← Image preprocessing notebook
├── step_2.ipynb                    ← Segmentation pipeline notebook
└── step_3.ipynb                    ← Measurement & evaluation notebook
```

---

## Getting Started

### Prerequisites
- Python 3.8+
- pip

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/agnespriscilla/burn-wound-measurement.git
cd burn-wound-measurement

# 2. Install dependencies
pip install -r requirements.txt

# 3. Run the app
streamlit run app.py
```

The app will open in your browser at `http://localhost:8501`.

### Dependencies

```
streamlit==1.52.0
opencv-python-headless==4.12.0.88
numpy==2.2.6
pandas==2.3.3
Pillow==12.0.0
requests
```

---

## Usage Guide

### Full Pipeline Mode
1. Upload an image with a **gold coin placed at any corner**
2. Select a segmentation method (Hole Filled is recommended)
3. Click **Process Image**
4. Download results as PNG / JSON / CSV

### Exact Batch Mode
1. Upload the **original image** (with coin)
2. Upload the corresponding **pre-generated mask** (`hole_filled_*.jpg`)
3. Click **Calculate** results will match batch processing output exactly

### Roboflow AI Mode
1. Configure your Roboflow credentials in the sidebar (API Key, Workspace, Project, Version)
2. Upload an image with a coin reference
3. Click **Detect with Roboflow AI**

---

## Tech Stack

| Category | Tools |
|---|---|
| Web App | Streamlit |
| Image Processing | OpenCV, NumPy, Pillow |
| Data Handling | Pandas |
| AI Integration | Roboflow API |
| Notebooks | Jupyter Notebook |

---

## Original Project

This repository is forked from [nadyasaraswati/burn-wound-measurement](https://github.com/nadyasaraswati/burn-wound-measurement). Contributions and improvements have been made to the application layer, including the multi-mode Streamlit interface and Roboflow API integration.
