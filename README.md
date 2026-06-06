# Planterior AI

<p align="right">
  <a href="./en/README.md">US English</a> &nbsp;|&nbsp;
  <a href="./ko/README.md">KR 한국어</a> &nbsp;|&nbsp;
  <a href="./ja/README.md">JP 日本語</a>
</p>

<p align="center">
  <b>AI-powered floorplan recognition and Unreal Engine 5 runtime interior generation.</b>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Unreal%20Engine-5.5-0E1128?style=for-the-badge&logo=unrealengine&logoColor=white" alt="Unreal Engine 5.5">
  <img src="https://img.shields.io/badge/FastAPI-SaaS%20Backend-009688?style=for-the-badge&logo=fastapi&logoColor=white" alt="FastAPI">
  <img src="https://img.shields.io/badge/Python-AI%20Pipeline-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/C++-UE5%20Runtime-00599C?style=for-the-badge&logo=cplusplus&logoColor=white" alt="C++">
  <img src="https://img.shields.io/badge/AWS-Cloud%20Infrastructure-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white" alt="AWS">
</p>

---

## Overview

**Planterior AI** is an integrated pipeline for turning a 2D apartment floorplan image into a reviewed, editable, and runtime-generated 3D interior space.

The name **Planterior** combines **Floorplan** and **Interior**.  
The project focuses on the full workflow from floorplan understanding to interior generation: AI recognition, user review, UE5 procedural generation, AI furniture placement, and CAD-style export.

This public repository is a portfolio documentation repository. The production source code, private model weights, deployment scripts, credentials, and datasets are not included.

<p align="center">
  <img src="./assets/screenshots/overview/01-service-flow.png" width="860" alt="Planterior AI service overview">
</p>
<p align="center"><sub>Screenshot slot: service flow, upload, AI review, and UE5 runtime result</sub></p>

---

## What This Project Does

| Stage | Description |
|---|---|
| Floorplan Upload | Uploads a 2D apartment floorplan image to the SaaS backend |
| Scale Confirmation | Confirms real-world scale using `mm_per_px` |
| AI Structure Recognition | Detects wall, door, window, room, and dimension candidates |
| User Review | Separates wall review and opening review before 3D generation |
| UE5 Runtime Generation | Generates walls, floors, ceilings, openings, lighting, materials, and furniture |
| Furniture AI | Builds furniture-placement datasets and trains a Transformer-based placement model |
| CAD Export | Exports reviewed floorplan data as SVG/PNG drawings |

---

## Architecture

```text
2D Floorplan Image
	-> AWS-backed FastAPI SaaS Backend
		-> Auth / Session / Project / Job
		-> Upload / Scale Confirmation
		-> Preprocess
		-> YOLO Recognition Backend
		-> Geometry Solver
		-> Confidence Aggregator
		-> Validation Gate
		-> Wall / Opening Review State
	-> Unreal Engine 5 Client
		-> Annotation Widget
		-> Runtime CSG Wall Generator
		-> Door / Window / Furniture Spawners
		-> Runtime Lighting
		-> CAD Export
	-> Furniture AI Pipeline
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
<p align="center"><sub>Screenshot slot: generated UE5 interior space</sub></p>

---

## Tech Stack

Each row links to the detailed technical document for that area.

<table>
<tr>
<td valign="top" width="190"><b>ENGINE</b></td>
<td>
	<a href="./en/technical/ue5-runtime.md">
		<img src="https://skillicons.dev/icons?i=unreal&theme=dark" height="48" alt="Unreal Engine" />
	</a>
	<br>
	<a href="./en/technical/ue5-runtime.md"><b>Unreal Engine 5.5</b></a>, Runtime CSG, Procedural Mesh, UMG, Slate, Dynamic Lighting, DLSS
</td>
</tr>
<tr>
<td valign="top"><b>CLIENT / LANGUAGE</b></td>
<td>
	<a href="./en/technical/ue5-runtime.md">
		<img src="https://skillicons.dev/icons?i=cpp,python,html,css&theme=dark" height="48" alt="C++ Python HTML CSS" />
	</a>
	<br>
	C++, Blueprint, Python, PyQt6, HTML/CSS
</td>
</tr>
<tr>
<td valign="top"><b>BACKEND / SAAS</b></td>
<td>
	<a href="./en/technical/backend-saas.md">
		<img src="https://skillicons.dev/icons?i=fastapi,sqlite,docker&theme=dark" height="48" alt="FastAPI SQLite Docker" />
	</a>
	<br>
	<a href="./en/technical/backend-saas.md"><b>FastAPI SaaS Backend</b></a>, JWT, Refresh Token Rotation, SQLite, Rate Limit, Audit Log, OpenAPI
</td>
</tr>
<tr>
<td valign="top"><b>CLOUD</b></td>
<td>
	<a href="./en/technical/backend-saas.md">
		<img src="https://skillicons.dev/icons?i=aws&theme=dark" height="48" alt="AWS" />
	</a>
	<br>
	AWS-backed backend operation, API access, storage expansion, and future worker separation
</td>
</tr>
<tr>
<td valign="top"><b>AI / CV</b></td>
<td>
	<a href="./en/technical/ai-floorplan.md">
		<img src="https://skillicons.dev/icons?i=pytorch,tensorflow,opencv&theme=dark" height="48" alt="PyTorch TensorFlow OpenCV" />
	</a>
	<br>
	<a href="./en/technical/ai-floorplan.md"><b>YOLO / Instance Segmentation</b></a>, OpenCV, EasyOCR, Geometry Solver, Confidence Aggregator, Validation Gate
</td>
</tr>
<tr>
<td valign="top"><b>FURNITURE AI</b></td>
<td>
	<a href="./en/technical/furniture-ai.md">
		<img src="https://skillicons.dev/icons?i=pytorch,python&theme=dark" height="48" alt="PyTorch Python" />
	</a>
	<br>
	<a href="./en/technical/furniture-ai.md"><b>Furniture Transformer</b></a>, PyQt6 Annotator, Synthetic Dataset Generator, Variation Generator
</td>
</tr>
<tr>
<td valign="top"><b>GPU</b></td>
<td>
	<a href="./en/technical/ai-floorplan.md">
		<img src="https://skillicons.dev/icons?i=cuda&theme=dark" height="48" alt="CUDA" />
	</a>
	<br>
	CUDA, cuDNN, cuBLAS, AMP, NVIDIA DALI, TensorRT, NPP
</td>
</tr>
<tr>
<td valign="top"><b>DATA / EXPORT</b></td>
<td>
	<a href="./en/technical/ue5-runtime.md#cad-export">
		<img src="https://skillicons.dev/icons?i=sqlite&theme=dark" height="48" alt="SQLite" />
	</a>
	<br>
	JSON, SQLite, user storage, manifest files, SVG/PNG CAD export
</td>
</tr>
<tr>
<td valign="top"><b>ETC</b></td>
<td>
	<img src="https://skillicons.dev/icons?i=git,github,vscode,visualstudio,windows,docker&theme=dark" height="48" alt="Git GitHub VS Code Visual Studio Windows Docker" />
	<br>
	Git, GitHub, Visual Studio, VS Code, Windows x64, Docker, pytest
</td>
</tr>
</table>

---

## AWS Usage

Planterior AI uses AWS as the cloud infrastructure layer for the SaaS backend environment.

The public portfolio keeps AWS details at an architectural level. Private deployment scripts, account identifiers, endpoints, credentials, and infrastructure secrets are not included.

AWS is used or prepared for the following responsibilities:

- backend service deployment
- external SaaS API access
- uploaded floorplan and artifact storage expansion
- separation of API server and AI inference workers
- managed database, object storage, queue, and monitoring migration path

---

## Repository Scope

Included in this repository:

- multilingual README files
- technical documentation
- architecture notes
- screenshot placeholders
- portfolio-oriented project structure

Not included:

- production source code
- private model weights
- private datasets
- AWS credentials
- deployment secrets
- production API endpoints
- user data

---

## Documentation

| Document | Description |
|---|---|
| [English](./en/README.md) | English project overview |
| [한국어](./ko/README.md) | Korean project overview |
| [日本語](./ja/README.md) | Japanese project overview |
| [AI Floorplan Recognition](./en/technical/ai-floorplan.md) | YOLO, OCR, preprocessing, geometry solving |
| [SaaS Backend](./en/technical/backend-saas.md) | FastAPI, auth, projects, jobs, review state, AWS-backed operation |
| [UE5 Runtime Builder](./en/technical/ue5-runtime.md) | Runtime CSG, procedural generation, CAD export |
| [Furniture AI](./en/technical/furniture-ai.md) | Dataset generation, annotation, Transformer training |

---

## Screenshot Guide

Replace the placeholder images with actual screenshots while keeping the same filenames.

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

## Status

Planterior AI is presented as a portfolio project covering the full pipeline from floorplan image understanding to UE5 runtime interior generation and AI furniture placement.

The public repository is intentionally documentation-first.
