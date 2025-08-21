# Technical Summary

## Approach
- **Detection:** Used **YOLOv8 (Ultralytics)** pre-trained on the COCO dataset to detect vehicles (car, bus, truck, motorcycle).  
- **Tracking:** Implemented **SORT (Simple Online and Realtime Tracking)** to assign unique IDs and maintain them across frames, avoiding duplicate counts.  
- **Lane Assignment:** Defined 4 lane polygons manually for the video. Each detected vehicle’s bounding box is checked against these polygons to determine its lane.  
- **Counting Logic:** Introduced a horizontal counting line per lane. A vehicle is counted once when crossing the line in the upward direction. This ensures one-time counting.  
- **Optimization:** Tuned YOLO model size (`yolov8n.pt` for speed, `yolov8s.pt` for better accuracy) and SORT hyperparameters to balance accuracy and real-time performance on Colab T4 GPU.

## Challenges
1. **Stopped or Slow Traffic:** Vehicles that stop near the counting line were not always registered.  
2. **ID Churn:** SORT tracker sometimes assigned new IDs when detections flickered, leading to inflated ID numbers.  
3. **Edge Cases in Lane Assignment:** Vehicles on polygon borders or already inside a lane when the video started were not always counted.  
4. **Colab Environment Issues:** ABI mismatches between NumPy, SciPy, and OpenCV caused runtime errors during development.

## Solutions
- Added **tolerance and intersection checks** for counting lines (±3px margin and segment intersection), ensuring vehicles are counted even if they cross slowly or partially.  
- Implemented an **inside-streak fallback**: if a vehicle remains above the line in a lane for several consecutive frames, it gets counted.  
- Introduced **alias IDs** for display and CSV, keeping them clean and sequential even if SORT creates many short-lived tracks.  
- Filtered detections to **lane ROIs only**, preventing spurious detections from burning through IDs.  
- Pinned compatible versions of **NumPy 2.x, SciPy ≥1.13.1, OpenCV 4.10, and Ultralytics 8.3.1** with a `numpy.rec` shim, resolving environment issues on Colab.

## Outputs
- **CSV file** (`counts.csv`) with `vehicle_id, lane, frame, timestamp`.  
- **Annotated video** (`output_annotated.mp4`) showing polygons, IDs, lane labels, and live counts.  
- **Final Summary** printed in console: total vehicle counts per lane.  
