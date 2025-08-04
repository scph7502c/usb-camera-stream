
# FastAPI Video Stream (MJPEG) with OpenCV

A minimal, production-ready example of real-time webcam streaming using **FastAPI** and **OpenCV**, packaged with **Poetry** for Python 3.12.

- Streams MJPEG at `GET /video` using `multipart/x-mixed-replace`
- Works out of the box with a local webcam: `cv2.VideoCapture(0)`
- Managed with Poetry

---

## ✨ What you get

- **FastAPI** app exposing `/video` that continuously yields JPEG frames
- **OpenCV** capture/encode pipeline (`VideoCapture` → `imencode(".jpg")`)
- **Poetry** project configuration in `pyproject.toml` for clean dependency management

---

## 🧱 Requirements

- Python **3.12+**
- A webcam available to OpenCV (e.g., `/dev/video0` on Linux)
- OS support for camera drivers (Video4Linux on Linux, DirectShow/MediaFoundation on Windows, AVFoundation on macOS)

> If you are on Windows or need GUI/high-level codecs, you may prefer `opencv-python` over `opencv-python-headless`. This repo uses `opencv-python-headless` for lightweight server environments.

---

## 📦 Installation (Poetry)

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

## ▶️ Running the server
1) Start Uvicorn:
```bash
poetry run uvicorn main:app --reload
```

2) Open your browser at:
```bash
http://localhost:8000/video
```
## ⚙️ Configuration notes

-   **Camera index**: Change `cv2.VideoCapture(0)` to another index (1, 2, …) if you have multiple cameras.
    
-   **Performance**:
    
    -   Resize frames before encoding, e.g. `cv2.resize(frame, (640, 480))`.
        
    -   Reduce JPEG size: `cv2.imencode(".jpg", frame, [int(cv2.IMWRITE_JPEG_QUALITY), 70])`.
        
-   **Cleanup**: On shutdown you may want to release the camera:
    
    python
    
    CopyEdit
    
    `import atexit
    atexit.register(lambda: camera.release())` 
    
    Or use FastAPI lifespan events for structured startup/shutdown.

## 🩺 Troubleshooting

-   **Permission denied / device busy**: Ensure no other app uses the camera. On Linux, check `/dev/video*` and group permissions (`video`).
    
-   **Black/empty frames**: Try another index (1, 2) or a different backend:
```bash
cv2.VideoCapture(0, cv2.CAP_V4L2)  # Linux
```
**Windows**: If capture fails, switch to `opencv-python` and/or try MediaFoundation:
```bash
cv2.VideoCapture(0, cv2.CAP_MSMF)
```
