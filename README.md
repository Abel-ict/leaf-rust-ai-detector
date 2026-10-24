# 🌿 LeafRust AI

**AI-Powered Plant Leaf Rust Detection Using YOLO11**  
*INSA Summer Camp · Computer Vision · Python · Google Colab*

LeafRust AI is an experimental computer-vision project that identifies **suspected rust-affected regions** in plant leaf photos. A fine-tuned **YOLO11 nano** object-detection model draws bounding boxes around potential infections and returns a simple detection result.

> **Status:** Working prototype — not a validated plant-disease diagnostic tool.

## ✨ Key Features

- Detect potential rust-affected regions in uploaded plant leaf photos.
- Draw bounding boxes around suspected infection spots.
- Adjust the prediction-confidence threshold.
- Display **Rust detected** or **No rust detected** in a Gradio demo.
- Train and evaluate using Google Colab.

## 🖼️ Example Prediction

![Model-predicted regions on a leaf](Screenshot%20from%202026-10-24%2017-58-00.png)

*Example from the actual prototype. Some boxes overlap or extend beyond the leaf; these predictions need further refinement.*

## 📊 Dataset and Model

| Detail | Value |
| --- | --- |
| Architecture | YOLO11 nano (object detection) |
| Annotated images | 48 |
| Rust bounding boxes | 229 |
| Classes | `rust` (one class) |
| Training images | 34 |
| Validation images | 8 |
| Held-out test images | 6 |
| Training environment | Google Colab with Tesla T4 GPU |

The images were manually annotated in MakeSense.ai and exported in YOLO format. Pretrained YOLO11 nano weights were fine-tuned on this small dataset.

## 📁 Repository Contents

| File | Purpose |
| --- | --- |
| [INSA_Leaf_Rust_Professional (1).ipynb](INSA_Leaf_Rust_Professional%20%281%29.ipynb) | Colab workflow for training, evaluation and Gradio testing |
| [best (3).pt](best%20%283%29.pt) | Trained detection model |
| [Prediction screenshot](Screenshot%20from%202026-10-24%2017-58-00.png) | Example output |
| `README.md` | Documentation |

## 🚀 How to Use

### Run the notebook in Colab

1. Open the [notebook](INSA_Leaf_Rust_Professional%20%281%29.ipynb) in [Google Colab](https://colab.research.google.com/).
2. Select **Runtime → Change runtime type → T4 GPU** if available.
3. Run the notebook cells in order.
4. When prompted, upload the separately prepared `INSA_leaf_rust_48_ready.zip` dataset. **This dataset ZIP is not included in the repository.**
5. Train and evaluate the model, then test uploaded images using the Gradio interface.

The Gradio URL created in Colab is temporary and requires the app session to remain running.

### Run the downloaded model locally

Install the dependency:

```bash
pip install ultralytics
```

Place the trained weights and a test image in your working directory:

```python
from ultralytics import YOLO

model = YOLO('best (3).pt')
results = model.predict('your_leaf.jpg', conf=0.55, save=True)

if any(len(result.boxes) for result in results):
    print('Rust detected')
else:
    print('No rust detected')
```

The prediction output image is saved by Ultralytics in its runs directory. **No rust detected** only means that no detection passed the chosen threshold; it does not prove the leaf is healthy.

## 🔍 Limitations

- The dataset is small and currently consists of rust-annotated leaves, without dedicated healthy-leaf examples.
- Some visible rust spots are unlabeled, and some boxes are incorrectly placed.
- The detector sometimes produces overlapping boxes or false detections on the background.
- A detection confidence of 99% is **not** 99% overall model accuracy.
- Reliable performance on unseen leaves has not yet been established.

## 🛠️ Future Improvements

1. Correct annotations and add more rust-infected images.
2. Include verified healthy leaves and diverse backgrounds.
3. Evaluate on a larger independent dataset using precision, recall and mAP.
4. Improve detection filtering and the user interface.
5. Publish a reproducible inference application.

## 🎓 About the Project

Created as an **INSA Summer Camp** machine-learning prototype to explore computer vision for identifying visible leaf-rust symptoms.

---

*For demonstration and research only; not a substitute for expert plant-disease assessment.*
