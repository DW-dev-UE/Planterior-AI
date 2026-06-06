<p align="right">
  <a href="../../en/technical/furniture-ai.md">US English</a> &nbsp;|&nbsp;
  <a href="../../ko/technical/furniture-ai.md">KR 한국어</a> &nbsp;|&nbsp;
  <a href="../../ja/technical/furniture-ai.md">JP 日本語</a>
</p>


# AI 가구 배치 모델

AI 가구 배치 프로젝트는 방 구조 데이터를 기반으로 어떤 가구를 어디에 배치할지 학습하기 위한 Python 프로젝트입니다.

<p align="center">
  <img src="../../assets/screenshots/technical/03-furniture-ai.png" width="860" alt="AI 가구 배치 데이터셋 및 학습 스크린샷 영역">
</p>
<p align="center"><sub>AI 가구 배치 데이터셋 및 학습 스크린샷 영역</sub></p>

> PyQt6 어노테이터, 생성된 scene JSON, 학습 로그, 예측 시각화 이미지를 배치하세요.

---

## 목적

이 모듈은 FloorplanSP의 자동 가구 배치 기능을 위해 다음을 수행합니다.

- 가구 배치용 scene dataset 생성
- PyQt6 기반 수동 어노테이션
- synthetic room layout 생성
- reference scene 기반 variation 생성
- Transformer 기반 autoregressive 가구 배치 모델 학습
- 예측 결과 시각화

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

## 입력 데이터

모델 입력은 구조 토큰과 raster 입력을 함께 사용합니다.

| 입력 | 설명 |
|---|---|
| Scene Tokens | 벽, 문, 창문을 구조화된 token으로 표현 |
| Raster Tensor | wall, door, window, interior 채널을 가진 2D raster |
| Room Metadata | room type, size, pyeong group 등 |

예시 구조 토큰:

```text
[type, x1, y1, x2, y2, thickness]
```

---

## 출력 데이터

모델은 가구를 autoregressive token sequence로 생성합니다.

```text
[category, cx, cy, width, depth, rotation]
```

가구 하나는 6개 토큰으로 표현되며, 최대 가구 개수를 제한해 안정적인 생성을 수행합니다.

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

기본 모델 특성:

| 항목 | 값 |
|---|---|
| Model | Transformer Encoder-Decoder |
| Raster Size | 256 x 256 |
| Input Channel | wall / door / window / interior |
| Coordinate Bins | 256 |
| Rotation Bins | 24 |
| Max Furniture | 32 |
| Training | AdamW, cosine warmup, teacher forcing |

---

## Dataset Generation

### Synthetic Generator

- rectangular room 생성
- L-shape room 생성
- living room / bedroom / kitchen / office / dining room recipe 적용
- sofa, bed, wardrobe, desk, TV 등 category 기반 배치 규칙 적용

### Variation Generator

- 사람이 만든 reference scene을 기준으로 평수별 변형 생성
- room scaling
- wall attachment 관계 보존
- position / rotation jitter
- review_state 기반 approve / reject 관리

이 구조를 통해 실제 수작업 데이터가 부족한 초기 단계에서도 학습 데이터를 확장할 수 있습니다.

---

## FloorplanSP와의 연결

```text
Confirmed Room Structure
	-> Furniture AI Input
	-> Category / Position / Size / Rotation Prediction
	-> UE5 AutoPlace Manager
	-> Furniture Actor Spawn
```

현재 공개 문서 기준으로는 데이터 생성, 어노테이션, 학습, 샘플 예측까지를 포트폴리오 범위로 설명하고, production inference API 연동은 향후 확장 항목으로 분리합니다.

---

## 향후 확장

- production inference CLI 추가
- FastAPI inference endpoint 연결
- UE5 AutoPlaceManager와 직접 연동
- collision / accessibility constraint 추가
- 사용자 선호 기반 가구 스타일 추천
- 실제 아파트 평면도 기반 데이터셋 확장
