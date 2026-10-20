# Hand Gesture Recognition on ESP32-CAM with TinyML ✋

**An end-to-end TinyML system that recognizes hand gestures on a microcontroller and plays matching audio feedback.**

🎥 **Demo video and write-up:** https://lnkd.in/p/dEYHnhwG



> **Note:** This was a university project.The source code is not currently included in this repository. This page documents the project, its design decisions, and its results.

---

## Overview

This project covers the full TinyML pipeline: collecting a dataset, designing and training a custom CNN, quantizing it to INT8, and deploying it on an **ESP32-CAM** using **TensorFlow Lite Micro**. The ESP32-CAM runs inference on camera frames and sends predictions over serial to a **Python application**, which plays the corresponding audio.

## Recognized gestures

Six gestures plus a "no gesture" class (7 classes in total):

| Gesture | Gesture | Gesture |
|---|---|---|
| Hello | Stop ✊ | Yes ✌️ |
| Help ☝️ | Good | OK |
| *No gesture* | | |

## System architecture

```
Camera (ESP32-CAM)
      ↓
INT8-quantized CNN (TensorFlow Lite Micro, on-device)
      ↓  prediction over serial
Python application
      ↓
Audio feedback for the recognized gesture
```

## Results

| Metric | Value |
|---|---|
| Validation accuracy | ~90.8% |
| Number of classes | 7 (6 gestures + no gesture) |
| Quantization | INT8 |
| Dataset | 7,000+ images captured, 2,000 curated for the final dataset |
| Time to prediction | ~40 seconds |

## Development journey

The whole project took around five months:

- **~1 month:** learning TinyML, computer vision, and embedded deployment.
- **~2 months:** building the dataset. Images were captured in three collection phases. The first two datasets were discarded entirely because they did not produce reliable results. The final curated dataset of 2,000 images did.
- **~2 months:** optimizing, debugging, retraining, and testing multiple CNN architectures against the hardware limits of the ESP32-CAM.

## Key lessons

**Validation accuracy is not the whole story.** Some architectures reached 94–96% validation accuracy but performed worse on the real hardware than the final deployed model. Real-world testing on the device mattered more than the validation score.

**Data quality beat data quantity.** Two full datasets were thrown away. Careful curation of a smaller dataset gave better results than more images.

## Limitations and trade-offs

- **Slow prediction time (~40 s).** This was the biggest compromise. I tried to speed it up by reducing the input size from 96×96 to 48×48, using fewer neurons per layer, and using fewer layers. All of these attempts lowered accuracy in real-world testing, so I chose to prioritize accuracy over speed.
- The system was tested under the conditions of the data collection setup, and performance may drop with different lighting, backgrounds, or users.

## Future improvements

- Reduce latency while keeping accuracy, for example with more efficient architectures or hardware with more compute.
- Collect a more diverse dataset (more users, lighting, and backgrounds) to improve robustness.
- Publish the firmware, training code, and Python application in this repository.

## Tech stack

`TinyML` · `TensorFlow Lite Micro` · `ESP32-CAM` · `Python` · `CNN` · `INT8 quantization`


## Author

**Elena Zoghby**: Telecommunications & Computer Engineering student, Lebanese University
[GitHub](https://github.com/Elena-Zoghby)
