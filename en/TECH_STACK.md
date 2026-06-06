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
