# Bhatiyani-Astute-Intelligence-assignment
Footfall Counter with ROI Polygon + Trajectories + Heatmap

# Technologies Used:

Object Detection: YOLOv8

Tracking: ByteTrack (via Supervision)

Visualization: OpenCV + Supervision (BoxAnnotator, LabelAnnotator, PolygonZone)

Programming Language: Python (3.10+)

Dependencies: ultralytics, supervision, numpy, opencv-python-headless

# Key Components

Detection (YOLOv8)
The system uses YOLOv8, a powerful object detection model, to spot people in every video frame. It focuses only on the “person” class and filters out other objects using a confidence threshold to keep the detections clean and accurate.

Tracking (ByteTrack with Supervision)
Once people are detected, ByteTrack keeps track of them across frames, giving each person a unique ID. This helps the system follow the same individual as they move around, even if they get partially hidden or the camera view changes slightly.

ROI-Based Counting
A polygonal Region of Interest (ROI) is drawn on the frame — for example, at an entrance or exit. The system keeps an eye on when a person crosses this region. If someone moves inside, it’s counted as an “entry,” and if they move out, it’s counted as an “exit.” A short cooldown time ensures the same person isn’t counted multiple times by mistake.

Centroid Smoothing
To make movements look smoother and prevent jittery detections, a centroid smoother averages the person’s position over time. This creates steady tracking and helps the counting logic stay consistent.

Trajectories and Heatmap Visualization
The system draws movement trails for each person, showing the paths they’ve taken. Alongside this, it builds a heatmap that reveals which areas are most frequently visited, helping visualize crowd flow and density in an intuitive way.

Output and Monitoring
Finally, all this information — detections, ROI, counts, and heatmap — is combined into an output video. The system also logs the current “In,” “Out,” and “Active” (inside) counts in real time, giving a clear overview of how many people are moving through the area.

# link of video
https://drive.google.com/file/d/16Aha-AA3tX9opX99aA0tOsJXtYD_oMQb/view?usp=sharing

# Explanation of Counting Logic

The counting logic revolves around a polygon-shaped Region of Interest (ROI) — basically, a custom-drawn zone on the video where we want to track people entering or leaving, such as a doorway, gate, or hallway.

Every time YOLOv8 detects a person, ByteTrack assigns them a unique ID so the system can recognize and follow the same individual as they move across frames. For each tracked person, we calculate the center of their bounding box (called the centroid) and apply a small amount of smoothing to it. This helps reduce flickering and keeps the movement steady, even if detections jump slightly between frames.

The system constantly checks whether each person’s centroid is inside or outside the ROI.

If a person moves from outside to inside, it increases the “In” count.

If they move from inside to outside, it increases the “Out” count.

To make sure people aren’t accidentally counted twice (for example, due to small detection shifts or someone hovering near the edge), there’s a cooldown timer for each ID. This means a person must move clearly in or out before being counted again.

By keeping track of each person’s last position and movement direction, the system maintains accurate, real-time entry and exit counts — even when multiple people are in the frame at once.

# Dependencies and Setup Instructions

To run this project, you only need a few Python libraries and a video file to test with. The setup is quite straightforward.

1. Install the Required Libraries
First, make sure you’re using Python 3.10 or above.
Then, open your terminal (or Google Colab) and run this command to install all the required packages:

pip install -U ultralytics supervision opencv-python-headless


ultralytics – for YOLOv8 object detection

supervision – for tracking, annotations, and ROI handling

opencv-python-headless – for video processing

2. Prepare Your Files
Place your input video (for example, mall.mp4) in the same folder as your script.
Save your main Python file as something like footfall_counter.py.

3. Run the Script
Now, simply run it:

python footfall_counter.py


Or, if you’re using Google Colab, just run:

!python footfall_counter.py


4. Custom Settings
You can tweak these parameters based on your needs:

source: path to your input video file

output_path: name for the processed output video

model_name: YOLO model to use (yolov8n.pt for faster, yolov8s.pt for more accurate results)

resize_width: resize the video for faster performance

conf: confidence threshold for detections

5. Output
Once the script finishes running, you’ll get a new video (like output_person_roi.avi) with:

Bounding boxes and unique IDs for each person

A blue ROI overlay showing your counting area

Real-time “In” and “Out” counts

Movement trajectories and a heatmap showing where people spent the most time
