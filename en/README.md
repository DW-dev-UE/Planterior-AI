<p align="right">
  <a href="../en/README.md">US English</a> &nbsp;|&nbsp;
  <a href="../ko/README.md">KR 한국어</a> &nbsp;|&nbsp;
  <a href="../ja/README.md">JP 日本語</a>
</p>


# FloorplanSP

**FloorplanSP** is an integrated pipeline that converts a 2D apartment floorplan image into an AI-reviewed and user-verified 3D interior space generated at runtime in Unreal Engine 5.

> This is a public portfolio documentation repository. The production source code is private and not included.  
> FloorplanSP uses AWS as the cloud infrastructure layer for SaaS backend operation, API access, storage expansion, and future AI inference worker separation.

> This repository is a public portfolio documentation repository. Source code is private. The project is presented through screenshots, architecture notes, and technical explanations.


---

## Tech Stack

> The stack below is organized by the actual role each technology plays in FloorplanSP.

### ENGINE

<p>
	<img src="https://skillicons.dev/icons?i=unreal&theme=dark" height="48" alt="Unreal Engine" />
</p>

| Technology | Role in the project |
|---|---|
| Unreal Engine 5.5 | Runtime 3D interior generation, real-time preview, UI and input |
| UE Procedural Mesh / Runtime CSG | Procedural walls, floors, ceilings, fillers, and interior finishes |
| UE UMG / Slate | Review UI, wall/opening review, final annotation editor |
| Dynamic Lighting | SkyLight, DirectionalLight, Point/RectLight for runtime-spawned actors |
| DLSS / Frame Generation Support | High-resolution real-time interior preview optimization |

### CLIENT / LANGUAGE

<p>
	<img src="https://skillicons.dev/icons?i=cpp,python,html,css&theme=dark" height="48" alt="C++ Python HTML CSS" />
</p>

| Technology | Role in the project |
|---|---|
| C++ | UE5 runtime core, WallGenerator, DoorSpawner, AutoPlaceManager, API subsystems |
| Blueprint | Visual assets and editor-configured actors for windows, doors, lights, and furniture |
| Python | AI backend, FurnitureAI dataset generation/training, CV pipeline |
| PyQt6 | Manual annotator for furniture placement datasets |
| HTML / CSS | Lightweight FastAPI dashboard and documentation UI |

### BACKEND / SAAS

<p>
	<img src="https://skillicons.dev/icons?i=fastapi,sqlite,docker&theme=dark" height="48" alt="FastAPI SQLite Docker" />
	<img src="https://img.shields.io/badge/JWT-Auth-111827?style=for-the-badge" alt="JWT" />
	<img src="https://img.shields.io/badge/Refresh%20Token-Rotation-111827?style=for-the-badge" alt="Refresh Token Rotation" />
</p>

| Technology | Role in the project |
|---|---|
| FastAPI | SaaS API server for UE5 client integration |
| JWT Access Token | API authentication after login |
| Refresh Token Rotation | Session persistence and refresh token reuse detection |
| SQLite | Initial SaaS/local storage for users, sessions, projects, and jobs |
| Rate Limit / Audit Log | API abuse prevention and user activity history |
| Multipart Upload | Floorplan image upload |
| OpenAPI / Swagger | API testing and documentation |
| Docker | Deployment candidate and reproducible runtime environment |

### AI / COMPUTER VISION

<p>
	<img src="https://skillicons.dev/icons?i=pytorch,tensorflow,opencv&theme=dark" height="48" alt="PyTorch TensorFlow OpenCV" />
	<img src="https://img.shields.io/badge/YOLO-Detection%20%2F%20Segmentation-FF6F00?style=for-the-badge" alt="YOLO" />
	<img src="https://img.shields.io/badge/EasyOCR-Text%20Detection-4B5563?style=for-the-badge" alt="EasyOCR" />
</p>

| Technology | Role in the project |
|---|---|
| YOLO / Ultralytics Hook | Candidate detection for walls, doors, and windows |
| Instance Segmentation | Mask-based recognition of structural elements |
| OpenCV | Crop, deskew, Canny, Hough, CLAHE, inpaint, contour processing |
| EasyOCR | Dimension text, text masks, and scale candidate extraction |
| Geometry Solver | Wall vectorization, junction merge, opening-wall binding, room closure |
| Confidence Aggregator | Combines model, snap, angle, binding, and OCR scores |
| Validation Gate | Decides AUTO_OK, NEEDS_REVIEW, FAILED, etc. |
| Florence-2 / SAM2 / Grounded-SAM2 | Offline labeling assistance, not runtime API calls |

### FURNITURE AI

<p>
	<img src="https://skillicons.dev/icons?i=pytorch,python&theme=dark" height="48" alt="PyTorch Python" />
	<img src="https://img.shields.io/badge/Transformer-Furniture%20Placement-7C3AED?style=for-the-badge" alt="Transformer" />
	<img src="https://img.shields.io/badge/PyQt6-Annotator-2563EB?style=for-the-badge" alt="PyQt6" />
</p>

| Technology | Role in the project |
|---|---|
| PyQt6 Annotator | Manual furniture scene annotation |
| Synthetic Dataset Generator | Rectangular/L-shaped rooms and recipe-based scene generation |
| Variation Generator | Pyeong-size variations from reference scenes |
| Raster + Token Encoding | wall/door/window/interior raster and structure tokens |
| Transformer Encoder-Decoder | Furniture token sequence generation conditioned by room structure |
| AdamW / Cosine Warmup | Training optimization |
| Visualization Output | GT vs prediction PNG visualization |

### GPU / ACCELERATION

<p>
	<img src="https://skillicons.dev/icons?i=cuda&theme=dark" height="48" alt="CUDA" />
	<img src="https://img.shields.io/badge/cuDNN-DL%20Primitives-76B900?style=for-the-badge" alt="cuDNN" />
	<img src="https://img.shields.io/badge/cuBLAS-Matrix%20Ops-76B900?style=for-the-badge" alt="cuBLAS" />
	<img src="https://img.shields.io/badge/DALI-Data%20Pipeline-76B900?style=for-the-badge" alt="DALI" />
	<img src="https://img.shields.io/badge/TensorRT-FP16%20Inference-76B900?style=for-the-badge" alt="TensorRT" />
</p>

| Technology | Role in the project |
|---|---|
| CUDA | GPU parallel processing for training and inference |
| cuDNN / cuBLAS | Acceleration for convolution, matrix multiplication, normalization, activation |
| AMP FP16/FP32 | Mixed precision for training speed and accuracy balance |
| NVIDIA DALI | GPU pipeline for YOLO data loading and preprocessing |
| TensorRT FP16 | Inference optimization, kernel fusion, memory reuse |
| NPP | GPU image resizing, denoising, and contour refinement |
| DLSS | High-resolution rendering optimization for the UE5 viewer |

### DATA / STORAGE

<p>
	<img src="https://skillicons.dev/icons?i=sqlite&theme=dark" height="48" alt="SQLite" />
	<img src="https://img.shields.io/badge/JSON-Review%20Payload-374151?style=for-the-badge" alt="JSON" />
	<img src="https://img.shields.io/badge/SVG%2FPNG-CAD%20Export-374151?style=for-the-badge" alt="SVG PNG" />
</p>

| Technology | Role in the project |
|---|---|
| JSON | Reviewed generation payloads, project state, annotation save files |
| SQLite | SaaS user/session/project/job metadata |
| Local User Storage | user_id-based uploads, preprocess, structure, project artifacts |
| SVG / PNG Export | CAD-style export of reviewed floorplans |
| Manifest Files | Tracking preprocess, dataset, and training outputs |

### ETC

<p>
	<img src="https://skillicons.dev/icons?i=git,github,vscode,visualstudio,windows&theme=dark" height="48" alt="Git GitHub VSCode Visual Studio Windows" />
</p>

| Technology | Role in the project |
|---|---|
| Git / GitHub | Portfolio documentation and version control |
| Visual Studio | UE5 C++ development |
| VS Code | Python/FastAPI/FurnitureAI development |
| Windows x64 | UE5 client target platform |
| pytest | Backend and pipeline testing |


---

## Project Overview

FloorplanSP allows a user to upload a floorplan image, review AI-detected walls, doors, windows, and rooms, and generate a 3D interior space in UE5 from the verified data.

<p align="center">
  <img src="../assets/screenshots/overview/01-service-flow.png" width="860" alt="Screenshot slot: full service flow">
</p>
<p align="center"><sub>Screenshot slot: full service flow</sub></p>

> Place a collage or sequence showing login, project selection, image upload, AI review, and UE5 generation.

### Main Features

| Feature | Description | Details |
|---|---|---|
| AI Floorplan Recognition | Detects walls, doors, windows, dimension lines, and room candidates | [AI Floorplan Recognition](./technical/ai-floorplan.md) |
| SaaS Backend | Auth, sessions, projects, jobs, scale, and review state management | [SaaS Backend](./technical/backend-saas.md) |
| UE5 Runtime Builder | Generates walls, floors, ceilings, doors, windows, lights, and furniture at runtime | [UE5 Runtime Builder](./technical/ue5-runtime.md) |
| Furniture AI | Dataset generation, annotation, and Transformer-based furniture placement training | [Furniture AI](./technical/furniture-ai.md) |
| CAD Export | Exports reviewed drawings as SVG/PNG | [UE5 Runtime Builder](./technical/ue5-runtime.md#cad-export) |

---

## Runtime Flow

```text
User Login
	-> Project Create / Load
	-> Floorplan Image Upload
	-> Scale Confirm
	-> AI Structure Detection
	-> Wall Review
	-> Opening Review
	-> Final Annotation
	-> UE5 Runtime 3D Generation
	-> Furniture Auto Placement
	-> CAD Export
```

<p align="center">
  <img src="../assets/screenshots/overview/02-ai-review.png" width="860" alt="Screenshot slot: AI review UI">
</p>
<p align="center"><sub>Screenshot slot: AI review UI</sub></p>

> Place screenshots for scale setup, wall review, and door/window review.

<p align="center">
  <img src="../assets/screenshots/overview/03-ue5-result.png" width="860" alt="Screenshot slot: UE5 3D result">
</p>
<p align="center"><sub>Screenshot slot: UE5 3D result</sub></p>

> Place screenshots showing generated walls, floors, ceilings, doors, windows, lighting, materials, and furniture.

---

## Architecture

```text
2D Floorplan Image
	-> FastAPI SaaS Backend
		-> Preprocess
		-> Scale Manager
		-> YOLO / Recognition Backend
		-> Geometry Solver
		-> Validation Gate
		-> Review JSON
	-> UE5 Client
		-> Annotation Widget
		-> Wall Generator
		-> Runtime CSG Actor
		-> Door / Window / Furniture Spawner
		-> CAD Export
	-> Furniture AI
		-> Dataset Generator
		-> PyQt Annotator
		-> Transformer Training
```

---

## Repository Policy

This public repository does not include source code. It includes:

- Project overview
- System architecture
- Technical explanations
- Screenshot placeholders
- Portfolio-oriented tech stack summary
- Future extension plan

---

## Technical Documents

- [AI Floorplan Recognition](./technical/ai-floorplan.md)
- [SaaS Backend](./technical/backend-saas.md)
- [UE5 Runtime Builder](./technical/ue5-runtime.md)
- [Furniture AI](./technical/furniture-ai.md)
