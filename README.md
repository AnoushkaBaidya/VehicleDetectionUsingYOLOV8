# Vehicle Counting System

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



## Code Snippets Explanation
1. Model Initialization and Configuration
# Define the model file path
MODEL = "yolov8x.pt"

# Import the YOLO class from the ultralytics package
from ultralytics import YOLO

# Create an instance of the YOLO object detection model using the specified MODEL
model = YOLO(MODEL)

# Fuse model for faster inference speed (if supported)
model.fuse()

This code initializes a YOLO object detection model using the specified model file path.

2. Frame Processing Pipeline
# Loop over video frames
for frame in tqdm(generator, total=video_info.total_frames):
    # Perform object detection on the frame and convert results to Detections format
    results = model(frame)
    detections = Detections(
        xyxy=results[0].boxes.xyxy.cpu().numpy(),
        confidence=results[0].boxes.conf.cpu().numpy(),
        class_id=results[0].boxes.cls.cpu().numpy().astype(int)
    )
    # Filter out unwanted detections
    mask = np.array([class_id in CLASS_ID for class_id in detections.class_id], dtype=bool)
    detections.filter(mask=mask, inplace=True)
    
    # Track objects
    tracks = byte_tracker.update(
        output_results=detections2boxes(detections=detections),
        img_info=frame.shape,
        img_size=frame.shape
    )
    tracker_id = match_detections_with_tracks(detections=detections, tracks=tracks)
    detections.tracker_id = np.array(tracker_id)

    # Filter out detections without trackers
    mask = np.array([tracker_id is not None for tracker_id in detections.tracker_id], dtype=bool)
    detections.filter(mask=mask, inplace=True)

    # Update the line counter with the filtered detections
    line_counter.update(detections=detections)

    # Annotate the frame with bounding boxes and labels
    frame = box_annotator.annotate(frame=frame, detections=detections, labels=labels)
    line_annotator.annotate(frame=frame, line_counter=line_counter)

    # Write the annotated frame to the target video file
    sink.write_frame(frame)

This code processes each frame of the video stream, performs object detection, tracks objects, filters out unwanted detections, updates a line counter for vehicle counting, annotates frames with bounding boxes and labels, and writes the annotated frames to a target video file.

## License
This project is licensed under the MIT License - see the LICENSE file for details.
