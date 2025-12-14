# YOLOv8 Object Detection Training

โปรเจกต์นี้สำหรับฝึกสอนโมเดล **YOLOv8** เพื่อใช้ตรวจจับวัตถุ (Object Detection) โดยใช้ dataset ของคุณเอง สคริปต์นี้ถูกเขียนให้รองรับการทำงานบน **GPU (CUDA)** เพื่อความรวดเร็วในการเทรน

---

## สิ่งที่ต้องเตรียม (Prerequisites)

ติดตั้ง Library ที่จำเป็นก่อนเริ่มใช้งาน:

```bash
pip install ultralytics torch
```

---

## การเตรียมข้อมูล (Dataset Setup)

ตรวจสอบให้แน่ใจว่าโฟลเดอร์ Dataset ของคุณมีการจัดเรียงตามรูปแบบ YOLO ดังนี้:

```
dataset/
├── data.yaml           # ไฟล์ config หลัก
├── train/
│   ├── images/        # รูปภาพสำหรับ train
│   └── labels/        # ไฟล์ .txt annotation
└── val/
    ├── images/        # รูปภาพสำหรับ validate
    └── labels/        # ไฟล์ .txt annotation
```

### ตัวอย่างเนื้อหาไฟล์ `data.yaml`

```yaml
path: /content/dataset    # path หลัก
train: train/images
val: val/images

nc: 2                     # จำนวน classes
names:                    # ชื่อ classes
  0: person
  1: bicycle
```

---

## วิธีใช้งาน (Usage)

### 1. แก้ไข Path

เปลี่ยน `dataset_path` ในโค้ดให้ตรงกับตำแหน่งโฟลเดอร์ของคุณ

### 2. ตั้งค่า Training Config

```python
from ultralytics import YOLO

# โหลดโมเดล
model = YOLO('yolov8n.pt')

# เริ่มการเทรน
model.train(
    data='dataset/data.yaml',     # path ไฟล์ config
    epochs=50,                     # จำนวน epoch
    batch=16,                      # batch size
    imgsz=640,                     # ขนาด input image
    device=0,                      # GPU device (0 = GPU แรก, 'cpu' = CPU)
    name='my_custom_yolo_model'    # ชื่อ project
)
```

### 3. รันสคริปต์

```bash
python train.py
```

---

## ผลลัพธ์ (Results)

หลังจากเทรนเสร็จ ไฟล์โมเดลและกราฟผลลัพธ์จะถูกบันทึกไว้ที่:

```
runs/detect/my_custom_yolo_model/
├── weights/
│   ├── best.pt          (ใช้ไฟล์นี้ในการ Deploy)
│   └── last.pt
├── results.png          (กราฟผลลัพธ์)
└── ...
```

---

## หมายเหตุและเคล็ดลับ

- GPU Acceleration: สคริปต์จะตรวจสอบ GPU ให้อัตโนมัติ หากมี GPU จะทำงานได้เร็วกว่า CPU มากขึ้น
- เปลี่ยนขนาดโมเดล: 
  - yolov8n.pt (Nano - เร็ว, เบา)
  - yolov8s.pt (Small)
  - yolov8m.pt (Medium)
  - yolov8l.pt (Large)
  - yolov8x.pt (Extra Large - แม่นยำสูง)
- Hyperparameter Tuning: ปรับ `epochs`, `batch`, `imgsz` ตามความสามารถของ GPU
- Data Augmentation: YOLOv8 ใช้ augmentation อัตโนมัติ ซึ่งช่วยเพิ่มความแม่นยำ

---

## แหล่งอ้างอิง

- [Ultralytics YOLOv8 Documentation](https://docs.ultralytics.com/)
- [YOLOv8 GitHub Repository](https://github.com/ultralytics/ultralytics)

---
