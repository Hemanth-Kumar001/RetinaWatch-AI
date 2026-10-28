# RetinaWatch AI

RetinaWatch AI is a student research prototype for classifying retinal fundus images into five diabetic-retinopathy severity categories. It provides class scores, a Grad-CAM-style visual overlay, basic image-quality warnings, and an uncertainty flag.

## Research results

The source dataset contained 3,662 labeled images. After duplicate review, 3,504 unique non-conflicting images were retained; 30 conflicting-label duplicate groups were excluded and 96 same-label copies collapsed. On a held-out set of 526 images, the compact CNN reached 70.15% accuracy, 56.40% balanced accuracy, 0.516 macro F1, and 0.717 quadratic weighted kappa. Performance varies by severity class and is weak on advanced classes. Detailed metrics are in [docs/metrics.json](docs/metrics.json).

## Run locally

```bash
pip install -r backend/requirements.txt
python bootstrap.py
gunicorn --chdir backend app:app --bind 0.0.0.0:5000 --workers 1 --timeout 180
```

Open http://127.0.0.1:5000 after the server starts.

## Deploy on Render

This repository includes `render.yaml`. To configure the service manually:

- **Build command:** `pip install -r backend/requirements.txt && python bootstrap.py`
- **Start command:** `gunicorn --chdir backend app:app --bind 0.0.0.0:$PORT --workers 1 --timeout 180`
- **Runtime:** Python 3.12.10
- **Region / plan:** Singapore / Free

The bootstrap script reconstructs the compressed checkpoint from the text-safe files in `model/`. The original image dataset is not included in this repository.

## Safety and limitations

**Not for medical use.** This is an educational research prototype, not a medical device. Do not use its output for diagnosis, screening, or treatment decisions. Model scores are not calibrated clinical probabilities, and the heatmap is not validated lesion localization. Consult a qualified eye-care professional for health concerns.

## Project structure

- `backend/` — Flask API and model inference code
- `frontend/` — web interface
- `model/` — text-safe checkpoint chunks used by the bootstrap script
- `docs/metrics.json` — held-out evaluation metrics
- `bootstrap.py` — reconstructs the checkpoint before launch
- `render.yaml` — Render service configuration
