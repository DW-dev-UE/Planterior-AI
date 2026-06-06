<p align="right">
  <a href="../en/README.md">US English</a> &nbsp;|&nbsp;
  <a href="../ko/README.md">KR 한국어</a> &nbsp;|&nbsp;
  <a href="../ja/README.md">JP 日本語</a>
</p>


# FloorplanSP

**FloorplanSP**는 2D 아파트 평면도 이미지를 입력받아 AI 분석, 사용자 검수, Unreal Engine 5 기반 3D 실내 공간 자동 생성, AI 가구 배치, CAD 도면 내보내기까지 연결하는 통합 파이프라인 프로젝트입니다.

> 이 저장소는 포트폴리오 공개용 문서 저장소입니다. 실제 소스코드는 포함하지 않으며, 주요 기능은 스크린샷과 기술 설명으로 정리합니다.

![Unreal Engine](https://img.shields.io/badge/Unreal%20Engine-5.5-0E1128?style=for-the-badge&logo=unrealengine&logoColor=white)
![C++](https://img.shields.io/badge/C++-UE%20Runtime-00599C?style=for-the-badge&logo=cplusplus&logoColor=white)
![Python](https://img.shields.io/badge/Python-AI%20Backend-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-SaaS%20API-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-YOLO%20%2F%20Furniture%20AI-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![CUDA](https://img.shields.io/badge/CUDA-GPU%20Acceleration-76B900?style=for-the-badge&logo=nvidia&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Deploy%20Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)

---

## Project Overview

FloorplanSP는 사용자가 평면도 이미지를 업로드하면 서버가 도면 구조를 분석하고, UE5 클라이언트에서 사용자가 벽, 문, 창문, 방 영역을 검수한 뒤, 검수된 데이터를 기반으로 3D 공간을 절차적으로 생성하는 시스템입니다.

<p align="center">
  <img src="../assets/screenshots/overview/01-service-flow.png" width="860" alt="서비스 전체 흐름 스크린샷 영역">
</p>
<p align="center"><sub>서비스 전체 흐름 스크린샷 영역</sub></p>

> 여기에 로그인, 프로젝트 선택, 이미지 업로드, AI 검수, 3D 생성 흐름을 한 화면 또는 콜라주 형태로 배치하세요.

### 핵심 기능

| 기능 | 설명 | 상세 문서 |
|---|---|---|
| AI 도면 인식 | 2D 도면에서 벽, 문, 창문, 치수선 등 구조 요소를 추출 | [AI 도면 분석 기술](./technical/ai-floorplan.md) |
| SaaS 백엔드 | 인증, 세션, 프로젝트, job, scale, 검수 상태 저장 | [SaaS 백엔드](./technical/backend-saas.md) |
| UE5 런타임 생성 | 검수된 구조 데이터를 기반으로 벽, 바닥, 천장, 창문, 문, 조명, 가구를 생성 | [UE5 런타임 빌더](./technical/ue5-runtime.md) |
| AI 가구 배치 | 가구 배치 데이터셋 생성, 어노테이션, Transformer 학습 | [AI 가구 배치 모델](./technical/furniture-ai.md) |
| CAD 내보내기 | 검수된 도면을 SVG/PNG 형태로 내보내기 | [UE5 런타임 빌더](./technical/ue5-runtime.md#cad-export) |

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
  <img src="../assets/screenshots/overview/02-ai-review.png" width="860" alt="AI 검수 UI 스크린샷 영역">
</p>
<p align="center"><sub>AI 검수 UI 스크린샷 영역</sub></p>

> 벽 검수, 문/창문 검수, 스케일 확정 화면을 배치하세요.

<p align="center">
  <img src="../assets/screenshots/overview/03-ue5-result.png" width="860" alt="UE5 3D 결과 스크린샷 영역">
</p>
<p align="center"><sub>UE5 3D 결과 스크린샷 영역</sub></p>

> 벽, 바닥, 천장, 창문, 문, 조명, 가구가 생성된 3D 결과를 배치하세요.

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

이 공개 저장소에는 소스코드를 포함하지 않습니다.

대신 다음 자료를 포함합니다.

- 프로젝트 개요
- 시스템 아키텍처
- 기능별 기술 설명
- 스크린샷 공간
- 포트폴리오용 기술 스택 정리
- 향후 확장 계획

---

## Technical Documents

- [AI 도면 분석 기술](./technical/ai-floorplan.md)
- [SaaS 백엔드](./technical/backend-saas.md)
- [UE5 런타임 빌더](./technical/ue5-runtime.md)
- [AI 가구 배치 모델](./technical/furniture-ai.md)
