<p align="right">
  <a href="../../en/technical/ai-floorplan.md">US English</a> &nbsp;|&nbsp;
  <a href="../../ko/technical/ai-floorplan.md">KR 한국어</a> &nbsp;|&nbsp;
  <a href="../../ja/technical/ai-floorplan.md">JP 日本語</a>
</p>


# AI間取り解析

この文書では、2D間取り画像をレビュー可能な構造データとUE5ランタイム生成用データへ変換するAI/CVパイプラインを説明します。

<p align="center">
  <img src="../../assets/screenshots/technical/01-yolo-pipeline.png" width="860" alt="スクリーンショット領域: YOLOベースの間取り解析">
</p>
<p align="center"><sub>スクリーンショット領域: YOLOベースの間取り解析</sub></p>

> モデル出力、wall mask、opening mask、確認前後の比較画像を配置してください。

---

## 目的

AI認識モジュールは、間取り画像から次の構造要素を抽出します。

| 対象 | 説明 |
|---|---|
| Wall | 壁領域、中心線、厚み、junction |
| Door | ドア位置、回転、gap、opening |
| Window | 窓位置、長さ、sill、height、type |
| Room | 部屋polygon、面積、ラベル |
| Scale | `mm_per_px` による実寸変換 |

---

## Recognition Pipeline

```text
Input Planterior Image
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

## YOLOの用途

YOLO系モデルは、高解像度の間取り画像から壁、ドア、窓などの構造候補を高速に検出するために使用します。

主な役割:

- 壁領域または中心線候補の抽出
- ドア/窓候補の検出
- opening classの分類
- confidence scoreの提供
- Geometry Solverが利用する候補データの生成

モデル出力はそのまま3D生成に使わず、Geometry Solverとユーザー確認を通して安定化します。

---

## CUDA / GPU最適化方針

学習および推論環境では、CUDAベースのGPUパイプラインを活用できるように設計します。

| 技術 | 目的 |
|---|---|
| cuDNN | 畳み込み、正規化、活性化などの深層学習演算を高速化 |
| cuBLAS | 行列積および線形代数演算を高速化 |
| AMP Mixed Precision | FP16/FP32混合精度で学習速度と精度のバランスを取る |
| NVIDIA DALI | 高解像度画像のデコード、リサイズ、正規化のボトルネックを削減 |
| TensorRT | FP16推論最適化、kernel fusion、memory reuse |
| NPP | リサイズ、ノイズ除去、輪郭補正などのGPU画像処理 |

> 注: この公開リポジトリはポートフォリオ用ドキュメントであり、実装ソースコードは含みません。

---

## Review-First Design

```text
AI Detection
	-> Wall Review
	-> Opening Review
	-> Final Annotation
	-> UE5 Runtime Generation
```

間取り図は表記方法が多様で、小さな座標誤差でも3D生成に影響するため、ユーザー確認を前提とした設計にしています。

---

## Output Contract

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

このデータはUE5のAnnotation WidgetとWall Generatorで利用されます。
