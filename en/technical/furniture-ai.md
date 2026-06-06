<p align="right">
  <a href="../../en/technical/furniture-ai.md">US English</a> &nbsp;|&nbsp;
  <a href="../../ko/technical/furniture-ai.md">KR 한국어</a> &nbsp;|&nbsp;
  <a href="../../ja/technical/furniture-ai.md">JP 日本語</a>
</p>


# Furniture AI

The Furniture AI project is a Python-based pipeline for generating datasets, annotating scenes, and training an autoregressive Transformer model for furniture placement.

<p align="center">
  <img src="../../assets/screenshots/technical/03-furniture-ai.png" width="860" alt="Screenshot slot: furniture AI dataset and training">
</p>
<p align="center"><sub>Screenshot slot: furniture AI dataset and training</sub></p>

> Place screenshots of the PyQt6 annotator, generated scene JSON, training logs, and prediction visualization here.

---

## Goal

This module supports automatic furniture placement in Planterior AI.

It provides:

- scene dataset generation
- PyQt6-based manual annotation
- synthetic room layout generation
- reference-based variation generation
- Transformer-based autoregressive furniture placement training
- prediction visualization

---

## Data Pipeline

```text
PyQt6 Annotator
	-> scene_*.json
	-> manifest.json
	-> FurnitureSceneDataset
	-> Scene Tokens
	-> Raster Tensor
	-> Furniture Token Sequence
	-> FurnitureTransformer Training
	-> Checkpoint / Visualization
```

---

## Inputs

The model uses both structured tokens and raster input.

| Input | Description |
|---|---|
| Scene Tokens | Structured tokens for walls, doors, and windows |
| Raster Tensor | 2D channels for wall, door, window, and interior |
| Room Metadata | Room type, size, and pyeong group |

Example scene token:

```text
[type, x1, y1, x2, y2, thickness]
```

---

## Outputs

The model generates furniture as an autoregressive token sequence.

```text
[category, cx, cy, width, depth, rotation]
```

Each furniture item is represented by 6 tokens, with a maximum number of furniture items for stable generation.

---

## FurnitureTransformer

```text
Raster 4ch Image
	-> Raster CNN
	-> CLS Embedding

Scene Tokens
	-> Scene Token Embedding

CLS + Scene Embeddings
	-> Transformer Encoder

Furniture Tokens
	-> Transformer Decoder
	-> Next Furniture Token Prediction
```

Default characteristics:

| Item | Value |
|---|---|
| Model | Transformer Encoder-Decoder |
| Raster Size | 256 x 256 |
| Input Channels | wall / door / window / interior |
| Coordinate Bins | 256 |
| Rotation Bins | 24 |
| Max Furniture | 32 |
| Training | AdamW, cosine warmup, teacher forcing |

---

## Dataset Generation

### Synthetic Generator

- creates rectangular rooms
- creates L-shaped rooms
- applies room recipes for living room, bedroom, kitchen, office, and dining room
- uses category-based placement rules for sofa, bed, wardrobe, desk, TV, etc.

### Variation Generator

- starts from a human-made reference scene
- generates different pyeong-size variations
- scales room geometry
- preserves wall attachment relationships
- applies position and rotation jitter
- manages approve/reject states through review metadata

This allows the dataset to be expanded even when manually labeled data is limited.

---

## Connection to Planterior AI

```text
Confirmed Room Structure
	-> Furniture AI Input
	-> Category / Position / Size / Rotation Prediction
	-> UE5 AutoPlace Manager
	-> Furniture Actor Spawn
```

For this public portfolio document, the scope is dataset generation, annotation, training, and sample prediction. Production inference API integration is listed as a future extension.

---

## Future Work

- production inference CLI
- FastAPI inference endpoint
- direct UE5 AutoPlaceManager integration
- collision and accessibility constraints
- user preference based style recommendation
- real apartment floorplan dataset expansion
