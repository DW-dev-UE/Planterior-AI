# FloorplanSP

<p align="right">
  <a href="./en/README.md">US English</a> &nbsp;|&nbsp;
  <a href="./ko/README.md">KR 한국어</a> &nbsp;|&nbsp;
  <a href="./ja/README.md">JP 日本語</a>
</p>

<p align="center">
  <b>AI-powered floorplan recognition, UE5 runtime 3D generation, SaaS project management, and AI furniture placement.</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Unreal%20Engine-5.5-0E1128?style=for-the-badge&logo=unrealengine&logoColor=white" alt="Unreal Engine 5.5">
  <img src="https://img.shields.io/badge/FastAPI-SaaS%20Backend-009688?style=for-the-badge&logo=fastapi&logoColor=white" alt="FastAPI">
  <img src="https://img.shields.io/badge/Python-AI%20Pipeline-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/C++-UE5%20Runtime-00599C?style=for-the-badge&logo=cplusplus&logoColor=white" alt="C++">
  <img src="https://img.shields.io/badge/AWS-Cloud%20Infrastructure-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white" alt="AWS">
</p>

> **Portfolio Repository Notice**  
> This repository is for portfolio presentation only. The production source code is private and is not included in this public repository.  
> Instead, this repository provides screenshots, architecture notes, technical documentation, and project-level implementation details.

---

## Overview

**FloorplanSP** is a service pipeline that converts a 2D apartment floorplan image into a verified 3D interior space generated at runtime in **Unreal Engine 5**.

The system combines:

- AI-based floorplan structure recognition
- User review for walls, doors, windows, rooms, and scale
- FastAPI-based SaaS backend for authentication, projects, jobs, and review state
- UE5 runtime procedural generation for walls, floors, ceilings, openings, lights, materials, and furniture
- AI furniture placement dataset generation and Transformer-based training pipeline
- CAD-style SVG/PNG export

<p align="center">
  <img src="./assets/screenshots/overview/01-service-flow.png" width="860" alt="FloorplanSP service overview screenshot">
</p>
<p align="center"><sub>Screenshot slot: service flow, upload, AI review, and UE5 runtime result</sub></p>

---

## Key Features

| Area | Feature |
|---|---|
| AI Floorplan Recognition | Detects wall, door, window, room, dimension, and scale candidates from floorplan images |
| Review-first Workflow | Separates wall review and opening review to improve reliability before 3D generation |
| UE5 Runtime Builder | Procedurally generates 3D spaces using reviewed floorplan data |
| SaaS Backend | Handles users, sessions, projects, jobs, scale confirmation, review state, quota, and audit logs |
| AWS Cloud Usage | Uses AWS as the cloud infrastructure layer for service deployment and portfolio-scale SaaS operation |
| Furniture AI | Generates furniture placement datasets and trains a Transformer-based placement model |
| CAD Export | Exports reviewed floorplan data as SVG/PNG drawings |
| Multilingual Docs | Provides documentation in English, Korean, and Japanese |

---

## System Architecture

```text
2D Floorplan Image
	-> FastAPI SaaS Backend
		-> Upload / Job Management
		-> Preprocess
		-> Scale Confirmation
		-> YOLO / Recognition Backend
		-> Geometry Solver
		-> Confidence Aggregator
		-> Validation Gate
		-> Wall / Opening Review State
	-> UE5 Client
		-> Annotation Widget
		-> Runtime CSG Wall Generator
		-> Door / Window / Furniture Spawners
		-> Runtime Lighting
		-> CAD Export
	-> Furniture AI
		-> PyQt6 Annotator
		-> Synthetic Dataset Generator
		-> Variation Generator
		-> Transformer Training
```

<p align="center">
  <img src="./assets/screenshots/overview/02-ai-review.png" width="860" alt="AI review workflow screenshot">
</p>
<p align="center"><sub>Screenshot slot: wall review, opening review, and scale confirmation UI</sub></p>

<p align="center">
  <img src="./assets/screenshots/overview/03-ue5-result.png" width="860" alt="UE5 generated interior screenshot">
</p>
<p align="center"><sub>Screenshot slot: UE5 generated interior space</sub></p>

---

## Tech Stack

<table>
<tr>
<td valign="top" width="190"><b>ENGINE</b></td>
<td>
	<img src="https://skillicons.dev/icons?i=unreal&theme=dark" height="48" />
	<br>
	Unreal Engine 5.5, Runtime CSG, Procedural Mesh, UMG, Slate, Dynamic Lighting, DLSS
</td>
</tr>
<tr>
<td valign="top"><b>CLIENT / LANGUAGE</b></td>
<td>
	<img src="https://skillicons.dev/icons?i=cpp,python,html,css&theme=dark" height="48" />
	<br>
	C++, Blueprint, Python, PyQt6, HTML/CSS
</td>
</tr>
<tr>
<td valign="top"><b>BACKEND / SAAS</b></td>
<td>
	<img src="https://skillicons.dev/icons?i=fastapi,sqlite,docker&theme=dark" height="48" />
	<br>
	FastAPI, JWT, Refresh Token Rotation, SQLite, Rate Limit, Audit Log, OpenAPI
</td>
</tr>
<tr>
<td valign="top"><b>CLOUD</b></td>
<td>
	<img src="https://skillicons.dev/icons?i=aws&theme=dark" height="48" />
	<br>
	AWS-based cloud infrastructure for backend deployment, runtime operation, and service expansion
</td>
</tr>
<tr>
<td valign="top"><b>AI / CV</b></td>
<td>
	<img src="https://skillicons.dev/icons?i=pytorch,tensorflow,opencv&theme=dark" height="48" />
	<br>
	YOLO, Instance Segmentation, OpenCV, EasyOCR, Geometry Solver, Confidence Aggregator, Validation Gate
</td>
</tr>
<tr>
<td valign="top"><b>FURNITURE AI</b></td>
<td>
	<img src="https://skillicons.dev/icons?i=pytorch,python&theme=dark" height="48" />
	<br>
	PyQt6 Annotator, Synthetic Dataset Generator, Variation Generator, Transformer Encoder-Decoder
</td>
</tr>
<tr>
<td valign="top"><b>GPU</b></td>
<td>
	<img src="https://skillicons.dev/icons?i=cuda&theme=dark" height="48" />
	<br>
	CUDA, cuDNN, cuBLAS, AMP, NVIDIA DALI, TensorRT, NPP
</td>
</tr>
<tr>
<td valign="top"><b>DATA / EXPORT</b></td>
<td>
	<img src="https://skillicons.dev/icons?i=sqlite&theme=dark" height="48" />
	<br>
	JSON, SQLite, User Storage, Manifest Files, SVG/PNG CAD Export
</td>
</tr>
<tr>
<td valign="top"><b>ETC</b></td>
<td>
	<img src="https://skillicons.dev/icons?i=git,github,vscode,visualstudio,windows,docker&theme=dark" height="48" />
	<br>
	Git, GitHub, Visual Studio, VS Code, Windows x64, Docker, pytest
</td>
</tr>
</table>

---

## AWS Usage

AWS is used as the cloud infrastructure layer for the SaaS-oriented backend environment.

In this portfolio, AWS is described at the infrastructure level rather than exposing private deployment details. The project is designed so that the FastAPI backend, project/job storage, authentication workflow, AI processing pipeline, and UE5 client communication can be operated as a cloud-backed service.

Typical responsibilities covered by the AWS layer include:

- backend service deployment
- external access to SaaS APIs
- storage expansion for uploaded floorplan images and generated artifacts
- scalable separation of API server and AI inference workers
- future migration path for managed database, object storage, queue, and monitoring services

> Private credentials, deployment scripts, account IDs, endpoints, and infrastructure secrets are intentionally not included.

---

## Runtime Workflow

```text
Login
	-> Project Create / Load
	-> Floorplan Upload
	-> Scale Confirmation
	-> Preprocess
	-> Structure Detection
	-> Wall Review
	-> Opening Review
	-> Final Annotation
	-> UE5 Runtime Generation
	-> AI Furniture Placement
	-> CAD Export
```

---

## Documentation

| Document | Description |
|---|---|
| [English Documentation](./en/README.md) | English project overview and technical links |
| [한국어 문서](./ko/README.md) | Korean project overview and technical links |
| [日本語ドキュメント](./ja/README.md) | Japanese project overview and technical links |
| [AI Floorplan Recognition](./en/technical/ai-floorplan.md) | AI/CV pipeline, YOLO, OCR, geometry solving |
| [SaaS Backend](./en/technical/backend-saas.md) | FastAPI backend, auth, jobs, projects, review state |
| [UE5 Runtime Builder](./en/technical/ue5-runtime.md) | UE5 procedural 3D generation pipeline |
| [Furniture AI](./en/technical/furniture-ai.md) | Furniture dataset generation and Transformer training |

---

## Repository Scope

This repository includes:

- README documentation
- multilingual project descriptions
- architecture notes
- technical explanation pages
- screenshot placeholders
- portfolio-oriented project structure

This repository does **not** include:

- production source code
- private model weights
- user data
- floorplan datasets containing private information
- AWS credentials or deployment secrets
- production API endpoints

---

## Screenshot Policy

The placeholder images under `assets/screenshots/` are intended to be replaced with real project screenshots.

Recommended screenshot groups:

```text
assets/screenshots/overview/
	01-service-flow.png
	02-ai-review.png
	03-ue5-result.png

assets/screenshots/technical/
	01-yolo-pipeline.png
	02-saas-backend.png
	03-furniture-ai.png
	04-ue5-runtime.png
	05-cad-export.png
```

---

## Project Status

This portfolio describes the complete system architecture and the implemented project direction across UE5 runtime generation, backend SaaS flow, AI/CV processing, and furniture placement AI.

Some production-level assets are intentionally omitted because this is a public portfolio repository.

---

## License / Usage

This repository is provided for portfolio review only.  
The source code, datasets, trained weights, and production infrastructure are private.
