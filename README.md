# Traffic Flow Analysis

This project implements a traffic flow analysis system that detects, tracks, and counts vehicles across **four lanes** in a traffic video using computer vision. The pipeline is optimized for Google Colab (T4 GPU) and satisfies all requirements from the assignment.

---

## Objective
Analyze traffic flow by:
- Detecting vehicles with a pre-trained model
- Tracking vehicles across frames
- Assigning them to lanes using defined polygons
- Counting vehicles per lane without duplicates
- Exporting results and showing a visual overlay

---

## Video Dataset
- Source video: [YouTube Traffic Video](https://www.youtube.com/watch?v=MNn9qKG2UFI)
- Downloaded automatically using `yt-dlp` inside the script.

---

## Core Features
- **Vehicle Detection:** YOLOv8 (pre-trained on COCO)
- **Lane Definition:** 4 lane polygons hardcoded for this video
- **Tracking:** SORT tracker (Kalman + Hungarian) to persist IDs
- **Counting Logic:** Vehicle counted once when crossing lane’s counting line
- **Optimization:** Tuned for smooth performance on Colab T4 GPU

---

## Requirements
Tested on **Python 3.12 (Colab)**

```txt
numpy>=2.0,<3
scipy>=1.13.1
pandas>=2.2.2
opencv-python-headless==4.10.0.84
ultralytics==8.3.1
lapx==0.5.9
shapely==2.0.4
yt-dlp
ffmpeg-python
```

## Usage

### Run the script
```bash
python New_Assessment.py --youtube "https://www.youtube.com/watch?v=MNn9qKG2UFI"
```

## Demo Video

Source video: [YouTube Traffic Video](https://drive.google.com/file/d/1-NeiQaIPdWBn3WRbJ_-S4RI7IWs-WD3g/view?usp=share_link)
