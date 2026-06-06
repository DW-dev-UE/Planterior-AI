<p align="right">
  <a href="../../en/technical/ai-floorplan.md">US English</a> &nbsp;|&nbsp;
  <a href="../../ko/technical/ai-floorplan.md">KR 한국어</a> &nbsp;|&nbsp;
  <a href="../../ja/technical/ai-floorplan.md">JP 日本語</a>
</p>


# AI 도면 분석 기술

이 문서는 FloorplanSP에서 2D 도면 이미지를 구조화된 데이터로 변환하기 위한 AI/CV 파이프라인을 설명합니다.

<p align="center">
  <img src="../../assets/screenshots/technical/01-yolo-pipeline.png" width="860" alt="YOLO 기반 도면 분석 파이프라인 스크린샷 영역">
</p>
<p align="center"><sub>YOLO 기반 도면 분석 파이프라인 스크린샷 영역</sub></p>

> 모델 추론 결과, wall mask, opening mask, 검수 전/후 비교 이미지를 배치하세요.

---

## 목적

AI 도면 분석 모듈은 평면도 이미지에서 다음 구조 요소를 추출합니다.

| 대상 | 설명 |
|---|---|
| Wall | 벽체 영역, 중심선, 두께, junction |
| Door | 문 위치, 회전, gap, opening |
| Window | 창문 위치, 길이, sill, height, type |
| Room | 방 영역 polygon, 면적, room label |
| Scale | `mm_per_px` 기반 실제 치수 변환 |

---

## Recognition Pipeline

```text
Input Floorplan Image
	-> Preprocess
		-> Crop
		-> Deskew
		-> Contrast Enhancement
		-> OCR Text / Dimension Mask
		-> Inpaint
	-> Recognition Backend
		-> YOLO-based Detection / Segmentation
		-> Wall / Door / Window Candidates
	-> Geometry Solver
		-> Skeletonize
		-> Vectorize
		-> Junction Merge
		-> Opening Binding
		-> Room Closure
	-> Confidence Aggregator
	-> Validation Gate
	-> Review Payload
```

---

## YOLO 사용 목적

YOLO 계열 모델은 도면 이미지에서 벽, 문, 창문과 같은 구조 요소 후보를 빠르게 검출하기 위해 사용됩니다.

프로젝트 설계상 YOLO recognition backend는 다음 역할을 담당합니다.

- 벽 영역 또는 중심선 후보 추출
- 문/창문 후보 영역 검출
- opening class 분류
- confidence score 제공
- Geometry Solver가 사용할 후보 데이터 생성

AI 모델의 결과는 그대로 3D 생성에 사용하지 않고, Geometry Solver와 사용자 검수 단계를 거쳐 안정화합니다.

---

## CUDA / GPU 최적화 방향

학습 및 추론 환경에서는 CUDA 기반 GPU 파이프라인을 활용할 수 있도록 설계합니다.

| 기술 | 사용 목적 |
|---|---|
| cuDNN | 합성곱, 정규화, 활성화 등 딥러닝 연산 가속 |
| cuBLAS | 행렬곱 및 선형 연산 가속 |
| AMP Mixed Precision | FP16/FP32 혼합 정밀도로 학습 속도와 정확도 균형 확보 |
| NVIDIA DALI | 고해상도 도면 이미지 디코딩, 리사이징, 정규화 병목 완화 |
| TensorRT | FP16 추론 최적화, kernel fusion, memory reuse |
| NPP | 리사이징, 노이즈 제거, 윤곽 보정 등 이미지 전후처리 GPU 병렬화 |

> 참고: 실제 공개 저장소에는 소스코드를 포함하지 않으며, 이 문서는 포트폴리오 기술 설명을 위한 문서입니다.

---

## 검수 중심 설계

도면 인식 결과는 항상 사용자 검수 단계를 거칩니다.

```text
AI Detection
	-> Wall Review
	-> Opening Review
	-> Final Annotation
	-> UE5 Runtime Generation
```

이 구조를 사용한 이유는 다음과 같습니다.

- 도면마다 표기 방식이 다름
- 벽 두께, 창문 종류, 문 회전 방향은 자동 인식만으로 확정하기 어려움
- 3D 생성 단계에서는 작은 좌표 오류도 벽/천장/바닥 생성 오류로 이어질 수 있음
- 사용자 검수를 통해 최종 payload의 안정성을 높일 수 있음

---

## Output Contract

AI 분석 서버는 UE5 클라이언트가 사용할 수 있는 구조화 데이터를 반환합니다.

```json
{
	"job_id": "job_xxx",
	"scale_id": "scale_xxx",
	"mm_per_px": 8.741,
	"walls": [],
	"doors": [],
	"windows": [],
	"rooms": [],
	"next_review": "wall"
}
```

이 데이터는 UE5의 Annotation Widget과 Wall Generator에서 3D 생성 입력으로 사용됩니다.
