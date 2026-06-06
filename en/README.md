<p align="right">
  <a href="../en/README.md">US English</a> &nbsp;|&nbsp;
  <a href="../ko/README.md">KR 한국어</a> &nbsp;|&nbsp;
  <a href="../ja/README.md">JP 日本語</a>
</p>


# FloorplanSP

**FloorplanSP** is an integrated pipeline that converts a 2D apartment floorplan image into an AI-reviewed and user-verified 3D interior space generated at runtime in Unreal Engine 5.

> This repository is a public portfolio documentation repository. Source code is private. The project is presented through screenshots, architecture notes, and technical explanations.

![Unreal Engine](https://img.shields.io/badge/Unreal%20Engine-5.5-0E1128?style=for-the-badge&logo=unrealengine&logoColor=white)
![C++](https://img.shields.io/badge/C++-UE%20Runtime-00599C?style=for-the-badge&logo=cplusplus&logoColor=white)
![Python](https://img.shields.io/badge/Python-AI%20Backend-3776AB?style=for-the-badge&logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-SaaS%20API-009688?style=for-the-badge&logo=fastapi&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-YOLO%20%2F%20Furniture%20AI-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)
![CUDA](https://img.shields.io/badge/CUDA-GPU%20Acceleration-76B900?style=for-the-badge&logo=nvidia&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Deploy%20Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)

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
