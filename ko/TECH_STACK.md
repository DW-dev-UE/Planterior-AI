## Tech Stack

> 아래 스택은 단순 나열이 아니라, Planterior AI 프로젝트에서 각 기술이 담당한 역할 기준으로 정리했습니다.

### ENGINE

<p>
	<img src="https://skillicons.dev/icons?i=unreal&theme=dark" height="48" alt="Unreal Engine" />
</p>

| 기술 | 프로젝트 내 역할 |
|---|---|
| Unreal Engine 5.5 | 3D 실내 공간 런타임 생성, 실시간 프리뷰, UI/입력 처리 |
| UE Procedural Mesh / Runtime CSG | 벽, 바닥, 천장, filler, 인테리어 마감재 절차적 생성 |
| UE UMG / Slate | 도면 검수 UI, wall/opening review, final annotation editor |
| Dynamic Lighting | 런타임 생성 액터에 맞춘 SkyLight, DirectionalLight, Point/RectLight 처리 |
| DLSS / Frame Generation 대응 | 고해상도 3D 실내 공간 실시간 확인 최적화 |

### CLIENT / LANGUAGE

<p>
	<img src="https://skillicons.dev/icons?i=cpp,python,html,css&theme=dark" height="48" alt="C++ Python HTML CSS" />
</p>

| 기술 | 프로젝트 내 역할 |
|---|---|
| C++ | UE5 핵심 런타임 로직, WallGenerator, DoorSpawner, AutoPlaceManager, API subsystem |
| Blueprint | 창문, 문, 조명, 가구 등 시각 자산과 UE 에디터 설정 |
| Python | AI 도면 분석 서버, FurnitureAI 데이터셋 생성/학습, CV 파이프라인 |
| PyQt6 | AI 가구 배치용 수동 어노테이터 |
| HTML / CSS | FastAPI lightweight dashboard 및 문서용 UI 구성 |

### BACKEND / SAAS

<p>
	<img src="https://skillicons.dev/icons?i=fastapi,sqlite,docker&theme=dark" height="48" alt="FastAPI SQLite Docker" />
	<img src="https://img.shields.io/badge/JWT-Auth-111827?style=for-the-badge" alt="JWT" />
	<img src="https://img.shields.io/badge/Refresh%20Token-Rotation-111827?style=for-the-badge" alt="Refresh Token Rotation" />
</p>

| 기술 | 프로젝트 내 역할 |
|---|---|
| FastAPI | UE5 클라이언트와 통신하는 SaaS API 서버 |
| JWT Access Token | 로그인 후 API 인증 |
| Refresh Token Rotation | 세션 유지 및 refresh token 재사용 감지 |
| SQLite | 초기 SaaS/로컬 운영용 user, session, project, job 저장 |
| Rate Limit / Audit Log | API 남용 방지 및 사용자 행위 기록 |
| Multipart Upload | 도면 이미지 업로드 처리 |
| OpenAPI / Swagger | API 테스트 및 문서화 |
| Docker | 배포 환경 구성 후보 및 실행 환경 고정 |

### AI / COMPUTER VISION

<p>
	<img src="https://skillicons.dev/icons?i=pytorch,tensorflow,opencv&theme=dark" height="48" alt="PyTorch TensorFlow OpenCV" />
	<img src="https://img.shields.io/badge/YOLO-Detection%20%2F%20Segmentation-FF6F00?style=for-the-badge" alt="YOLO" />
	<img src="https://img.shields.io/badge/EasyOCR-Text%20Detection-4B5563?style=for-the-badge" alt="EasyOCR" />
</p>

| 기술 | 프로젝트 내 역할 |
|---|---|
| YOLO / Ultralytics Hook | 벽, 문, 창문 등 구조 요소 후보 검출 |
| Instance Segmentation | 도면 내 구조 요소 mask 기반 분석 |
| OpenCV | crop, deskew, Canny, Hough, CLAHE, inpaint, contour 처리 |
| EasyOCR | 치수선 숫자, 텍스트 mask, scale 후보 추출 |
| Geometry Solver | wall centerline vectorization, junction merge, opening-wall binding, room closure |
| Confidence Aggregator | model, snap, angle, binding, OCR 점수 통합 |
| Validation Gate | AUTO_OK, NEEDS_REVIEW, FAILED 등 검수 필요 여부 판단 |
| Florence-2 / SAM2 / Grounded-SAM2 | runtime이 아닌 학습/라벨링 보조용 offline tool |

### FURNITURE AI

<p>
	<img src="https://skillicons.dev/icons?i=pytorch,python&theme=dark" height="48" alt="PyTorch Python" />
	<img src="https://img.shields.io/badge/Transformer-Furniture%20Placement-7C3AED?style=for-the-badge" alt="Transformer" />
	<img src="https://img.shields.io/badge/PyQt6-Annotator-2563EB?style=for-the-badge" alt="PyQt6" />
</p>

| 기술 | 프로젝트 내 역할 |
|---|---|
| PyQt6 Annotator | 가구 배치 scene 수동 어노테이션 |
| Synthetic Dataset Generator | rectangular / L-shape room과 room recipe 기반 데이터 생성 |
| Variation Generator | reference scene 기반 평수별 변형 데이터 생성 |
| Raster + Token Encoding | wall/door/window/interior raster와 구조 token을 모델 입력으로 구성 |
| Transformer Encoder-Decoder | 방 구조를 조건으로 furniture token sequence 생성 |
| AdamW / Cosine Warmup | 가구 배치 모델 학습 최적화 |
| Visualization Output | GT vs prediction 시각화 PNG 생성 |

### GPU / ACCELERATION

<p>
	<img src="https://skillicons.dev/icons?i=cuda&theme=dark" height="48" alt="CUDA" />
	<img src="https://img.shields.io/badge/cuDNN-DL%20Primitives-76B900?style=for-the-badge" alt="cuDNN" />
	<img src="https://img.shields.io/badge/cuBLAS-Matrix%20Ops-76B900?style=for-the-badge" alt="cuBLAS" />
	<img src="https://img.shields.io/badge/DALI-Data%20Pipeline-76B900?style=for-the-badge" alt="DALI" />
	<img src="https://img.shields.io/badge/TensorRT-FP16%20Inference-76B900?style=for-the-badge" alt="TensorRT" />
</p>

| 기술 | 프로젝트 내 역할 |
|---|---|
| CUDA | 학습/추론 연산의 GPU 병렬 처리 기반 |
| cuDNN / cuBLAS | 합성곱, 행렬곱, 정규화, 활성화 연산 가속 |
| AMP FP16/FP32 | 학습 속도와 정확도 균형을 위한 mixed precision |
| NVIDIA DALI | YOLO 데이터 로딩/전처리 병목을 GPU 파이프라인으로 이동 |
| TensorRT FP16 | 추론 최적화, kernel fusion, memory reuse |
| NPP | 이미지 resize, denoise, contour 보정 등 GPU 전후처리 |
| DLSS | UE5 3D viewer의 고해상도 렌더링 최적화 |

### DATA / STORAGE

<p>
	<img src="https://skillicons.dev/icons?i=sqlite&theme=dark" height="48" alt="SQLite" />
	<img src="https://img.shields.io/badge/JSON-Review%20Payload-374151?style=for-the-badge" alt="JSON" />
	<img src="https://img.shields.io/badge/SVG%2FPNG-CAD%20Export-374151?style=for-the-badge" alt="SVG PNG" />
</p>

| 기술 | 프로젝트 내 역할 |
|---|---|
| JSON | reviewed generation payload, project state, annotation save file |
| SQLite | SaaS user/session/project/job metadata |
| Local User Storage | user_id 기반 upload, preprocess, structure, project artifact 저장 |
| SVG / PNG Export | 검수된 도면의 CAD 스타일 내보내기 |
| Manifest Files | preprocess, dataset, training 결과 추적 |

### ETC

<p>
	<img src="https://skillicons.dev/icons?i=git,github,vscode,visualstudio,windows&theme=dark" height="48" alt="Git GitHub VSCode Visual Studio Windows" />
</p>

| 기술 | 프로젝트 내 역할 |
|---|---|
| Git / GitHub | 포트폴리오 문서화 및 버전 관리 |
| Visual Studio | UE5 C++ 개발 |
| VS Code | Python/FastAPI/FurnitureAI 개발 |
| Windows x64 | UE5 클라이언트 타겟 환경 |
| pytest | 백엔드 및 파이프라인 테스트 |
