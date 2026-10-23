# 🐱 Cat Left-Eye Locator

A lightweight Streamlit app powered by a custom YOLOv8 model that detects a cat’s left eye in any uploaded photo and returns its bounding-box and center coordinates.

## 🚀 Features

- **Real-time inference** on CPU/GPU using Ultralytics YOLOv8-nano  
- Draws a red box around the detected left eye and marks its center  
- Returns precise `(x, y)` pixel coordinates of the eye center  
- Simple, one-click web interface via [Streamlit](https://streamlit.io)

## 🛠️ Installation

1. **Clone** this repository  
```bash
git clone https://github.com/your-username/cat_left_eye_locator.git
cd cat_left_eye_locator
```

2.	Create a virtual environment (highly recommended)
```python
python3 -m venv .venv
source .venv/bin/activate
```
3.	Install dependencies
```bash
pip install -r requirements.txt
```

### Streamlit Cloud runtime note

This repository pins Streamlit Cloud to Python `3.11` via `runtime.txt` (`python-3.11`) to match the pinned `torch`/`torchvision`/OpenCV wheels used by Ultralytics YOLO.

## 🧠 Model & Data
	•	Architecture: YOLOv8-nano (pretrained on COCO, finetuned for one class: left_eye)
	•	Training set: ~1,400 manually-annotated images
	•	Validation set: ~600 held-out images
	•	Metrics (on test split, IoU ≥ 0.5):
	•	Precision: 0.60
	•	Recall:    0.80
	•	mAP@0.5:   0.60

See Ultralytics docs for more on YOLOv8: https://docs.ultralytics.com


## 📖 How It Works
1.	User uploads an image.
2.	Streamlit passes it to the YOLOv8 model:
```python
from ultralytics import YOLO
model = YOLO('best.pt')
results = model.predict(source=img_array, conf=0.25)
```
3.	First detected bounding box is extracted: (x1, y1, x2, y2).
4.	Center computed as ((x1+x2)/2, (y1+y2)/2) and drawn on the image.
