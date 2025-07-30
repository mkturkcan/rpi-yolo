# Running YOLO Models on Raspberry Pi Models

```bash
sudo apt install imx500-all
pip install ultralytics --break-system-packages
```

## Conda Environment Setup (Optional)
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-aarch64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
source ~/miniconda3/bin/activate
conda init --all
conda create -n ai python=3.11
conda activate ai
pip install ultralytics
```

## Run YOLO

```python
from picamera2 import Picamera2, Preview
import numpy as np
import cv2
from ultralytics import YOLO

model = YOLO('yolov8n.pt')
picam2 = Picamera2()
picam2.configure(picam2.create_preview_configuration ({'size': (640,640)}))
picam2.start()

while True:
  request = picam2.capture_request()
  request.save( 'main', 'test.jpg')
  request.release()
  result = model.predict('test.jpg')
  annotated_frame = result[0].plot()
  cv2.imshow('YOLO Detection', annotated_frame)
  cv2.waitKey(5000)
```
