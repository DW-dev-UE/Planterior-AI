<p align="right">
  <a href="../en/README.md">US English</a> &nbsp;|&nbsp;
  <a href="../ko/README.md">KR 한국어</a> &nbsp;|&nbsp;
  <a href="../ja/README.md">JP 日本語</a>
</p>


# FloorplanSP

**FloorplanSP** は、2Dマンション間取り画像を入力として、AI解析、ユーザー確認、Unreal Engine 5 による3D室内空間の自動生成、AI家具配置、CAD出力までをつなぐ統合パイプラインです。

> このリポジトリはポートフォリオ公開用のドキュメントリポジトリです。実際のソースコードは含めず、スクリーンショットと技術説明で構成します。


---

## Tech Stack

<table>
<tr>
<td valign="top" width="160"><b>ENGINE</b></td>
<td>
	<img src="https://skillicons.dev/icons?i=unreal&theme=dark" />
</td>
</tr>
<tr>
<td valign="top"><b>LANGUAGE</b></td>
<td>
	<img src="https://skillicons.dev/icons?i=c,cpp,python,react&theme=dark" />
</td>
</tr>
<tr>
<td valign="top"><b>AI</b></td>
<td>
	<img src="https://skillicons.dev/icons?i=pytorch,tensorflow&theme=dark" />
</td>
</tr>
<tr>
<td valign="top"><b>CLOUD</b></td>
<td>
	<img src="https://skillicons.dev/icons?i=aws,gcp,azure&theme=dark" />
</td>
</tr>
<tr>
<td valign="top"><b>ETC</b></td>
<td>
	<img src="https://skillicons.dev/icons?i=git,docker,github,html,css,vscode&theme=dark" />
</td>
</tr>
</table>

> アイコンはGitHub Markdownで表示される外部画像リンクを使用します。完全なオフライン構成にしたい場合は、`assets/icons/` にPNG/SVGを配置して画像パスを置き換えてください。

---

## Project Overview

FloorplanSPでは、ユーザーが間取り画像をアップロードすると、サーバーが構造要素を解析し、UE5クライアント上で壁、ドア、窓、部屋領域を確認した後、確認済みデータをもとに3D空間をプロシージャルに生成します。

<p align="center">
  <img src="../assets/screenshots/overview/01-service-flow.png" width="860" alt="スクリーンショット領域: サービス全体フロー">
</p>
<p align="center"><sub>スクリーンショット領域: サービス全体フロー</sub></p>

> ログイン、プロジェクト選択、画像アップロード、AI確認、3D生成の流れを配置してください。

### 主な機能

| 機能 | 説明 | 詳細 |
|---|---|---|
| AI間取り認識 | 2D図面から壁、ドア、窓、寸法線などを抽出 | [AI間取り解析](./technical/ai-floorplan.md) |
| SaaSバックエンド | 認証、セッション、プロジェクト、job、scale、確認状態を管理 | [SaaSバックエンド](./technical/backend-saas.md) |
| UE5ランタイム生成 | 確認済み構造データから壁、床、天井、窓、ドア、照明、家具を生成 | [UE5ランタイムビルダー](./technical/ue5-runtime.md) |
| AI家具配置 | 家具配置データセット生成、アノテーション、Transformer学習 | [AI家具配置モデル](./technical/furniture-ai.md) |
| CAD出力 | 確認済み図面をSVG/PNGとして出力 | [UE5ランタイムビルダー](./technical/ue5-runtime.md#cad-export) |

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
  <img src="../assets/screenshots/overview/02-ai-review.png" width="860" alt="スクリーンショット領域: AI確認UI">
</p>
<p align="center"><sub>スクリーンショット領域: AI確認UI</sub></p>

> スケール設定、壁確認、ドア/窓確認画面を配置してください。

<p align="center">
  <img src="../assets/screenshots/overview/03-ue5-result.png" width="860" alt="スクリーンショット領域: UE5 3D結果">
</p>
<p align="center"><sub>スクリーンショット領域: UE5 3D結果</sub></p>

> 生成された壁、床、天井、ドア、窓、照明、材質、家具を表示してください。

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

この公開リポジトリにはソースコードを含めません。

含める内容:

- プロジェクト概要
- システムアーキテクチャ
- 機能別技術説明
- スクリーンショット領域
- ポートフォリオ向け技術スタック
- 今後の拡張計画

---

## Technical Documents

- [AI間取り解析](./technical/ai-floorplan.md)
- [SaaSバックエンド](./technical/backend-saas.md)
- [UE5ランタイムビルダー](./technical/ue5-runtime.md)
- [AI家具配置モデル](./technical/furniture-ai.md)
