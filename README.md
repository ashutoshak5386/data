# 🎭 2D Avatar Gautam

This project extracts and processes **visemes** (mouth shapes for lip-sync) from a video, applies post-processing, and generates reusable video clips for **2D avatars**.

---

## 📂 Project Structure

2D-AVATAR-GAUTAM/
│── viseme_1/ # Example output folder for viseme images
│── visemes/ # Auto-generated folder for viseme crops
│── .gitignore
│── annotation.py # Main script: extracts visemes from video
│── postprocessing.py # Fades edges of viseme images
│── repeat_video.py # Loops idle video for smooth playback
│── Avatar IV Video.mp4 # Input video (face with speech)
│── Avatar IV Video.wav # Input audio for lip-sync cues
│── coords.json # Saved mouth coordinates + angles
│── data.json # Example lip-sync timing data
│── idle.mp4 # Idle animation clip
│── idle_repeated.mp4 # Auto-generated repeated idle loop
│── output_with_landmarks.mp4 # Debug video with facial landmarks drawn
│── requirements.txt # Dependencies


---

## ⚙️ Dependencies

Install Python libraries (**Python 3.8–3.11 recommended**):


pip install -r requirements.txt
Contents of requirements.txt:


opencv-python
mediapipe
numpy
moviepy
🚀 How It Works
1. Viseme Extraction (annotation.py)
Loads a video and lip-sync data (mouthCues with start/end times).

Uses MediaPipe FaceMesh to detect face + mouth landmarks.

Crops the mouth region for each unique viseme and saves as PNG.

Saves bounding boxes, angles, and timestamps in coords.json.

Creates a debug video output_with_landmarks.mp4 showing:

Face mesh landmarks

Mouth bounding box

Current viseme label

Run:


python annotation.py
2. Post-Processing (postprocessing.py)
Applies a fade effect on edges of viseme images for smoother blending.

Saves processed images with _processed.png suffix.




python postprocessing.py
3. Idle Video Looping (repeat_video.py)
Doubles the length of idle.mp4 by concatenating it with itself.

Output is idle_repeated.mp4.


python repeat_video.py
📊 Outputs
Viseme Images → Cropped mouth PNGs stored in /visemes

Landmarked Video → output_with_landmarks.mp4

Mouth Coordinates → coords.json (bounding boxes, angles)

Processed PNGs → _processed.png files with transparency

Idle Loops → idle_repeated.mp4

🏗️ System Architecture

                   ┌─────────────────────┐
                   │  Input Video (MP4)  │
                   └─────────┬───────────┘
                             │
                   ┌─────────▼───────────┐
                   │  annotation.py       │
                   │  (MouthCropper)      │
                   ├─────────────────────┤
                   │ Uses MediaPipe Face │
                   │ Mesh for landmarks  │
                   └─────────┬───────────┘
                             │
         ┌───────────────────┼───────────────────┐
         │                   │                   │
 ┌───────▼────────┐   ┌──────▼───────┐   ┌───────▼────────┐
 │  Viseme Images │   │ coords.json  │   │ output_with_   │
 │   (cropped)    │   │  (angles,    │   │ landmarks.mp4  │
 │                │   │  bounding box)│   │ (debug video) │
 └────────────────┘   └──────────────┘   └────────────────┘

Viseme Images ──► postprocessing.py ──► Processed PNGs (alpha faded)

idle.mp4 ──► repeat_video.py ──► idle_repeated.mp4
🔄 Flow Diagram (Data Pipeline)


[Start]
   │
   ▼
Load Input Video + Lip Sync Data (mouthCues)
   │
   ▼
Run MediaPipe FaceMesh → Detect face + mouth landmarks
   │
   ▼
Extract mouth bounding box
   │
   ├──► Crop mouth region → Save as Viseme.png
   │
   ├──► Calculate mouth angle + bbox coords → Save in coords.json
   │
   └──► Draw landmarks + label on frame → Save debug video
   │
   ▼
[End of annotation.py]
   │
   ├──► (Optional) postprocessing.py → fade viseme PNG edges
   │
   └──► (Optional) repeat_video.py → extend idle animation
🔮 Possible Extensions
Support multiple viseme sets (different speakers)

Replace cropped visemes directly in avatar templates

Improve idle blending with crossfade instead of hard repeat

Integrate with real-time speech-to-viseme mapping
