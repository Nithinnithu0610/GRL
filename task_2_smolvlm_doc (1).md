# Task 2 – Motion Detection with smolVLM

## 1. Overview

This project implements **real-time hand motion detection** using the SmolVLM model via webcam feed.  
It detects:

- Hand presence  
- Hand side (Left/Right)  
- Finger count (0–5)  
- Simple gestures (`Thumbs Up` / `Thumbs Down`)  

It can be **integrated with an attendance system** for gesture-based presence confirmation.

The implementation leverages Python or C++ for capturing frames and processing them with SmolVLM.

---

## 2. Core Architecture of smolVLM Realtime Webcam Repo

| Component | Responsibility |
|-----------|----------------|
| `llama-server` | Hosts SmolVLM model and performs inference on webcam frames. |
| SmolVLM Model | Performs visual-language understanding on input frames. |
| Frame Capture Module (Python/C++) | Captures live webcam frames using OpenCV or native C++ libraries. |
| Hand Detection Module | Detects bounding boxes and landmarks for hands using OpenCV / MediaPipe. |
| Hand Side Identification | Determines whether the detected hand is left or right using landmark analysis. |
| Finger Counting | Counts extended fingers using landmark positions. |
| Gesture Recognition | Recognizes predefined gestures such as Thumbs Up or Thumbs Down. |
| Logger | Records detection events and system performance metrics (CPU, memory). |

**Design Pattern**: Client-server architecture with modular hand detection and inference components.

---

## 3. Motion Detection Workflow Using Python/C++ and SmolVLM

### 3.1 Hand Detection
- Capture frames from the webcam.
- Detect hand bounding boxes and landmarks.
- Output: `"Hand Detected"` + coordinates.

### 3.2 Hand Side Identification
- Analyze landmarks to classify as Left or Right hand.

### 3.3 Finger Counting
- Determine which fingers are extended.
- Output: `"Finger Count: N"`

### 3.4 Gesture Recognition
- Thumbs Up: Thumb extended, other fingers folded.
- Thumbs Down: Thumb down, other fingers folded.
- Output: `"Gesture: Thumbs Up / Thumbs Down / Other"`

### 3.5 Logging and Monitoring
- Record gesture events.
- Track system metrics (CPU, memory) for performance analysis.

---

## 4. Component Interaction Diagram

```mermaid
flowchart TD
    A[Webcam Frame] --> B[Frame Capture Module (Python/C++)]
    B --> C[llama-server: SmolVLM]
    C --> D[Hand Detection Module]
    D --> E[Hand Side Identification]
    E --> F[Finger Counting]
    F --> G[Gesture Recognition]
    G --> H[Logger / Storage]
```

---

## 5. Deployment Architecture

- **Dependencies**: Python 3.x / C++ compiler, OpenCV, MediaPipe, llama.cpp, SmolVLM 500M model.
- **Build Steps**:
  1. Clone `llama.cpp` repository and build.
  2. Start the `llama-server` with SmolVLM model.
- **Server Command Example**:
```bash
llama-server -hf ggml-org/SmolVLM-500M-Instruct-GGUF -ngl 99
```
- Other compatible models can be used by changing the `-hf` argument.
- **Environments**: Can run on dev, staging, or prod. Supports Python/C++ clients via HTTP/WebSocket.

---

## 6. Runtime Behavior

- Application initializes `llama-server` and loads SmolVLM model.
- Python/C++ client streams frames to the server continuously.
- Server performs inference, detecting hand presence, hand side, finger count, and gestures.
- Results are sent back to the client and optionally logged.
- Errors (e.g., missing frames) are handled gracefully; background tasks monitor performance.

---

## 7. Extending or Integrating with Attendance Repo

- **Integration Point**: Replace or complement face recognition frames with hand detection frames.
- **Use Cases**:
  - Hand gestures as attendance confirmation.
  - Combine face + gesture for dual-factor presence validation.
- **Implementation**:
  - Add a module in the attendance repo to query SmolVLM server from Python or C++.
  - Log gesture events alongside face recognition logs.
  - Use real-time streaming or batch inference for efficiency.

---

## 8. Inputs & Outputs

| Input | Output |
|-------|--------|
| Live webcam frames | Hand Detected / No Hand |
| Hand landmarks | Left Hand / Right Hand |
| Finger positions | Finger Count (0–5) |
| Gesture detection | Thumbs Up / Thumbs Down / Other |

---

*End of Task 2 Documentation*

