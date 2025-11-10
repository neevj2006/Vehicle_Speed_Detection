# 🚗 Vehicle Speed Detection System

This project implements a **Vehicle Speed Detection System** that detects, tracks, and estimates the speed of multiple vehicles from recorded traffic footage using deep learning and computer vision.  
It combines **YOLOv8** for vehicle detection, **DeepSORT** for multi-object tracking, and **OpenCV** for visualization and speed estimation.

---

## 🎯 Project Overview

Modern traffic monitoring systems rely heavily on visual data.  
This project demonstrates a **data-driven computer vision pipeline** capable of:

- Detecting moving vehicles in a given video
- Tracking each vehicle with a unique ID
- Estimating and displaying the real-world speed (km/h) on bounding boxes

Built for **Google Colab**, this version provides a visual, reproducible workflow that integrates deep learning, tracking, and perspective-aware motion analysis.

---

## 🧠 Key Features

✅ **Vehicle Detection** — Uses pretrained [YOLOv8](https://github.com/ultralytics/ultralytics) (by Ultralytics) for high-accuracy detection of cars, buses, trucks, and bikes.  
✅ **Multi-Object Tracking** — Integrates [DeepSORT](https://github.com/nwojke/deep_sort) for consistent ID assignment across frames.  
✅ **Speed Estimation** — Calculates speed using pixel displacement, video FPS, and a perspective-based pixel-to-meter conversion gradient.  
✅ **Confidence Filtering** — Ignores weak detections below a configurable confidence threshold.  
✅ **Perspective Correction** — Reduces speed distortion as vehicles move toward or away from the camera.  
✅ **Visualization** — Displays bounding boxes, track IDs, and live speed (km/h) overlays in the output video.

---

## 🧩 Technologies Used

| Component | Purpose |
|------------|----------|
| **Python 3.10+** | Core language |
| **YOLOv8 (Ultralytics)** | Vehicle detection |
| **DeepSORT** | Object tracking |
| **OpenCV** | Frame processing and visualization |
| **NumPy** | Numerical operations |
| **Google Colab** | Cloud execution environment |

---

## ⚙️ System Architecture

```plaintext
Input Video
   │
   ▼
YOLOv8 Detector → Bounding Boxes
   │
   ▼
DeepSORT Tracker → Vehicle IDs & Paths
   │
   ▼
Speed Estimation (Pixel → Meter → km/h)
   │
   ▼
OpenCV Visualization → Bounding Boxes + Speed Overlay
   │
   ▼
Output Video (output_fixed.avi)
```

---

## 🧪 Implementation Steps

The Colab implementation is structured into three main stages:

### 1️⃣ Vehicle Detection
- Load a pretrained YOLOv8 model (`yolov8n.pt`)
- Run detection on each frame of the uploaded traffic video
- Apply confidence filtering to reduce noise

### 2️⃣ Multi-Object Tracking
- Initialize a DeepSORT tracker
- Track multiple vehicles across frames while maintaining unique IDs

### 3️⃣ Speed Estimation
- Compute pixel displacement between consecutive frames
- Apply a perspective-aware conversion (pixel → meter)
- Calculate real-world speeds using frame rate (FPS)
- Overlay smoothed speed values on each bounding box

---

## 🧮 Speed Estimation Model

The pixel-to-meter ratio varies with depth in the scene:

\[
\text{meters_per_pixel}(y) = m_{far} + (m_{near} - m_{far}) \times \frac{y}{H}
\]

where  
- \( y \): vertical pixel coordinate  
- \( H \): frame height  
- \( m_{far}, m_{near} \): conversion factors for far and near regions

Speed is then computed as:

\[
v_{km/h} = \text{distance}_{pixels} \times \text{meters\_per\_pixel} \times FPS \times 3.6
\]

---

## 📈 Results

- Each detected vehicle is assigned a **unique ID**.
- The bounding box displays **smoothed real-world speed** in km/h.
- Perspective correction eliminates drastic speed fluctuations when vehicles approach or recede from the camera.
- The processed video is saved as **`output_fixed.avi`**.

Example output frame:  

![Sample Output Frame](https://via.placeholder.com/800x450?text=Vehicle+Speed+Detection+Example)

---

## 🧰 How to Run (on Google Colab)

1. Open a new Colab notebook  
2. Copy and paste the project code  
3. Run all cells sequentially  
4. Upload your input video when prompted  
5. The processed video (`output_fixed.avi`) will be generated and displayed inline

---

## ⚙️ Adjustable Parameters

| Variable | Description | Typical Value |
|-----------|--------------|----------------|
| `CONF_THRESH` | Minimum confidence for detection | 0.4 – 0.6 |
| `p_far` | Meters per pixel at top of frame | 0.05 – 0.08 |
| `p_near` | Meters per pixel at bottom of frame | 0.01 – 0.03 |
| `SMOOTH_FRAMES` | Frames to average speed | 3 – 7 |

---

## 🔮 Future Enhancements

- Automatic **camera calibration** using homography or known lane markers  
- Integration with **ByteTrack** for more stable ID tracking  
- Deployment via **Streamlit** for real-time visualization  
- Improved accuracy using **YOLOv8m/l** models

---

**License:** MIT  
© 2025 Neev Jain. All rights reserved.
