# SignBridge
<img width="1308" height="488" alt="Screenshot 2026-08-19 000507" src="https://github.com/user-attachments/assets/63dd079e-9156-42bf-a82e-58eb1d73ce6f" />

[![Python 3.11+](https://img.shields.io/badge/python-3.11+-blue.svg)](https://www.python.org/downloads/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.141-009688.svg?style=flat&logo=fastapi)](https://fastapi.tiangolo.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**SignBridge** is a bidirectional sign language communication research prototype. It bridges communication between signers and non-signers through two optimized, real-time translation pipelines.

The system supports hand-based text entry through the Sign-to-Text keyboard and translation of voice or text into sign language concepts. All processing is performed locally to support privacy.

---

## Key Features

- **Sign-to-Text Keyboard**: A real-time, conservative alphabet recognizer using a verified MediaPipe and Random Forest pipeline. It includes a one-second cooldown, custom motion detection for dynamic letters (`J`, `Z`), and distinct audio feedback.
- **Autocomplete**: A local quantized DistilGPT2 neural model predicts the next words as the user signs, augmented by Google Web 1T n-gram statistics.
- **Voice and Text to Sign**: Users may speak into a microphone, transcribed locally via Whisper, or type text to generate an ordered sequence of sign concepts. The system retrieves native American Sign Language (ASL) video clips from online dictionaries and falls back to ASL alphabet cartoon images for spelled letters.
- **Neon UI**: A responsive frontend overlays neon, glowing, and pulsating skeletal keypoints directly over the live camera feed.
- **Local and Private**: No cloud dependencies. All models, from computer vision to Whisper speech recognition, run locally on the user's machine.

---

## Getting Started (Local Setup Guide)

Follow these steps to run SignBridge locally.

### Prerequisites

1. **Python 3.11+**: Ensure Python is installed and added to the system PATH.
2. **Git**: Required to clone the repository.
3. **FFmpeg** *(Optional but recommended)*: Required for the Voice-to-Sign microphone feature with Whisper. Install FFmpeg and add it to the system PATH.

### 1. Clone the Repository

Open a terminal or PowerShell and run:

```bash
git clone https://github.com/Hadi-Mostafa/SignBridge.git
cd SignBridge
```

*Note: If the repository is inside a larger folder, navigate to the `sign_language_translation` directory.*

### 2. Create a Virtual Environment

Using a virtual environment is recommended to avoid dependency conflicts.

```powershell
# Create the virtual environment
python -m venv venv

# Activate it (Windows)
.\venv\Scripts\activate

# Or on Mac/Linux:
# source venv/bin/activate
```

### 3. Install Dependencies

```powershell
pip install -r requirements.txt
```

### 4. Fetch the AI Models

SignBridge uses multiple local models. A provided script downloads and verifies them automatically using SHA-256 hashes.

```powershell
python scripts\fetch_models.py

# Verify that all required models loaded successfully
python scripts\doctor.py --load-models
```

*(Optional)* To use the **Voice-to-Sign** feature, download the local Whisper speech model:

```powershell
python scripts\fetch_speech_model.py
```

### 5. Start the Application

Run the FastAPI backend server:

```powershell
python -m uvicorn backend.main:app --host 127.0.0.1 --port 8000
```

Open a browser and navigate to:

[http://127.0.0.1:8000](http://127.0.0.1:8000)

---

## Usage

### Sign Keyboard (Alphabet Mode)

1. Select **Alphabet Mode** and click **Start Camera** (allow camera permissions), or click **Import Video** to upload a pre-recorded video for translation.
2. Present one handshape at a time to the camera or video. The neon keypoints will map to the hand.
3. The letter will lock in, a short tone will play, and the system will wait one second before accepting the next letter.
4. Lower the hand to re-arm repeated letters, such as spelling `LL`.
5. Use the arrow keys or click to select autocomplete suggestions.

### Voice/Text to Sign

1. Navigate to the second page of the application.
2. Type a sentence or click **Start Recording** to speak into the microphone.
3. The system processes the input and displays an ordered sequence of sign concepts. It dynamically retrieves high-quality native ASL video clips from online dictionaries for translated words. If a word is unavailable, it falls back to displaying ASL alphabet cartoon images for each spelled letter.

---

## Configuration

Set environment variables before starting the server. Useful variables include:

| Variable | Default | Purpose |
|---|---|---|
| `SIGNBRIDGE_OFFLINE` | `false` | Set to `1` to strictly prevent runtime downloads. |
| `ALPHABET_MIN_CONFIDENCE` | `0.65` | Adjust how strict the model is before accepting a static letter. |
| `WHISPER_MODEL_SIZE` | `base` | Select the local speech-recognition model to use. |

---

## Running Tests

To verify installation integrity and run regression tests:

```powershell
python -m unittest -v
```

---

## Disclaimer

**SignBridge is an academic research prototype.** It is not a certified continuous ASL translator and must not be used as a substitute for a qualified human sign language interpreter in legal, medical, or emergency situations.

For an in-depth review of evaluation metrics, domain shifts, and scientific promotion gates, consult `DIAGNOSTIC_REPORT.md` and `academic_report.md` in the repository.
```
