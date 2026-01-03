
# FastAPI Video Stream (MJPEG) with OpenCV

A minimal, production-ready example of real-time webcam streaming using **FastAPI** and **OpenCV**, packaged with **Poetry** for Python 3.12.

- Streams MJPEG at `GET /video` using `multipart/x-mixed-replace`
- Works out of the box with a local webcam: `cv2.VideoCapture(0)`
- Managed with Poetry

---

## What you get

- **FastAPI** app exposing `/video` that continuously yields JPEG frames
- **OpenCV** capture/encode pipeline (`VideoCapture` → `imencode(".jpg")`)
- **Poetry** project configuration in `pyproject.toml` for clean dependency management

---

##  Requirements

- Python **3.12+**
- A webcam available to OpenCV (e.g., `/dev/video0` on Linux)
- OS support for camera drivers (Video4Linux on Linux, DirectShow/MediaFoundation on Windows, AVFoundation on macOS)

> If you are on Windows or need GUI/high-level codecs, you may prefer `opencv-python` over `opencv-python-headless`. This repo uses `opencv-python-headless` for lightweight server environments.

---

## Installation (Poetry)

1) Install Poetry
```bash
pipx install poetry
```

2) Install dependencies from `pyproject.toml`:
```bash
poetry install
```

3) Activate the virtual environment
```bash
poetry shell
```

## Running the server
1) Start Uvicorn:
```bash
poetry run uvicorn main:app --reload
```

2) Open your browser at:
```bash
http://localhost:8000/video
```
---

-   **Camera index**: Change `cv2.VideoCapture(0)` to another index (1, 2, …) if you have multiple cameras.
    
-   **Performance**:
    
    -   Resize frames before encoding, e.g. `cv2.resize(frame, (640, 480))`.
        
    -   Reduce JPEG size: `cv2.imencode(".jpg", frame, [int(cv2.IMWRITE_JPEG_QUALITY), 70])`.
        
.

