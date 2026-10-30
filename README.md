# PolyMark Studio

**A local polygon labeling and dataset creation studio.** Click an object and get a polygon, or describe it in words
and label a whole dataset at once. SAM 2.1 and SAM 3 run on your own GPU: no cloud, no account, no credits, and
images never leave the computer.

> This repository is a project showcase. The software is not distributed publicly.

![Labeling view](docs/labeling.jpg)

*Demo dataset of buoy photos.*

## Highlights

- **Click-to-polygon labeling with SAM 2.1.** Add or remove points, drag a box prompt and cycle through mask
  candidates with `Tab`.
- **Text-prompt auto-labeling with SAM 3.** Type `yellow buoy` and every matching object in the dataset is masked.
- **Polygons with holes**, for rings, wheels and parts with cut-outs.
- **Augmentation:** flips, rotations, crops, color jitter, blur, noise and cutout. Image and labels are transformed
  by the same matrix, so labels never drift.
- **9 export formats:** YOLOv8-11 segmentation and detection, COCO segmentation and detection, Pascal VOC,
  Apple CreateML, TensorFlow CSV, semantic mask PNG, and a full native backup.
- **Train with Beyaz.** Train the author's own from-scratch detector on the labeled images, then let it pre-label
  new images.

![Text-prompt auto-labeling](docs/auto-labeling.jpg)

## Engineering

- **Frontend:** plain JavaScript with ES modules and no build step. Images live in IndexedDB, with metadata and
  raw blobs in separate stores so a library of thousands of images does not load into memory.
- **Mask-to-polygon pipeline:** Moore contour tracing, Douglas-Peucker simplification with a size-aware tolerance,
  and hole extraction. Compared with the model's raw mask: 0.9 % area difference, 1 px bounding-box difference.
- **AI engine:** a local Python server built on Hugging Face `transformers` loads and unloads SAM 2.1, SAM 3 and
  Beyaz on demand.
- **Measured on an RTX 4070 Laptop GPU (8 GB):**

| Model | VRAM | Inference |
|---|---|---|
| SAM 2.1 | 0.43 GB | ~0.5 s per click |
| SAM 3 | 2.7 GB | ~0.4 s per text prompt once warm |
| Both loaded | 3.2 GB total | — |

- **Desktop app:** a C# launcher serves the UI, starts the bundled engine without a console window and keeps
  training running in the tray when the window is closed.

![Export formats](docs/export.jpg)

## Author

© 2026 Ayberk KOCABUĞA. All rights reserved.
SAM 2.1 and SAM 3 are models by Meta, used under their respective licenses.
