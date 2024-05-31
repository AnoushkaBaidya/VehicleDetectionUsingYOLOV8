# Track and count vehicles with YoloV8

This repository contains code for a vehicle counting system implemented in Python. The system detects vehicles in a video stream, tracks them, and counts the number of vehicles passing a specified line.

## Requirements

- Python 3.x
- [numpy](https://numpy.org/) (version >= 1.0)
- [supervision](https://github.com/supervision-toolbox/supervision) (version >= 1.0)
- [ultralytics](https://github.com/ultralytics/yolov5) (version >= 1.0)
- [tqdm](https://github.com/tqdm/tqdm) (version >= 4.0)

## Installation

You can install the required dependencies using pip:

```bash
pip install numpy supervision ultralytics tqdm
```

## Usage
1. Set up the environment by installing the required dependencies.
2. Configure the system settings such as the model file path, source video path, line start and end points, etc.
3. Run the main script to process the video and count vehicles.

This code processes each frame of the video stream, performs object detection, tracks objects, filters out unwanted detections, updates a line counter for vehicle counting, annotates frames with bounding boxes and labels, and writes the annotated frames to a target video file.

https://github.com/AnoushkaBaidya/VehicleDetectionUsingYOLOV8/assets/115124698/90e489ee-3050-4b0c-8eb6-58f2d886e917

