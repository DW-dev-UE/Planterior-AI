<p align="right">
  <a href="../../en/technical/ue5-runtime.md">US English</a> &nbsp;|&nbsp;
  <a href="../../ko/technical/ue5-runtime.md">KR 한국어</a> &nbsp;|&nbsp;
  <a href="../../ja/technical/ue5-runtime.md">JP 日本語</a>
</p>


# UE5ランタイムビルダー

UE5ランタイムビルダーは、確認済みの間取りデータをもとに、壁、床、天井、ドア、窓、家具、照明をランタイムでプロシージャル生成します。

<p align="center">
  <img src="../../assets/screenshots/technical/04-ue5-runtime.png" width="860" alt="スクリーンショット領域: UE5ランタイム3D生成">
</p>
<p align="center"><sub>スクリーンショット領域: UE5ランタイム3D生成</sub></p>

> 生成された室内空間、壁/天井/床、窓/ドア、家具、照明を表示してください。

---

## コアコンポーネント

| モジュール | 役割 |
|---|---|
| Annotation Widget | 確認済みデータをgeneration payloadへ変換 |
| Wall Generator | CSG壁、床、天井、窓、部屋ジオメトリを生成 |
| Runtime CSG Actor | ランタイムでprocedural mesh boxを生成 |
| Door Spawner | ドアBlueprintを位置、回転、スケール付きでspawn |
| AutoPlace Manager | AI家具配置出力をUE actorへ変換 |
| Interior Formula | 部屋別の仕上げと構造を適用 |
| Ceiling Light Library | 部屋タイプとサイズに応じて照明Blueprintをspawn |

---

## Generation Flow

```text
Reviewed JSON
	-> Source Pixel Coordinates
	-> mm_per_px
	-> World Centimeter Conversion
	-> Wall / Floor / Ceiling CSG Spawn
	-> Door / Window Actor Spawn
	-> Room Matching
	-> Interior Formula Apply
	-> Furniture Placement
	-> Runtime Lighting
```

---

## 座標系

| 座標系 | 単位 | 用途 |
|---|---|---|
| Source Image Pixel | px | backend detectionとreview payload |
| Widget Local Space | px | UE5 annotation UI |
| World Centimeter | cm | UE5 runtime actor generation |

```text
mm_per_px = known_length_mm / measured_length_px
cm_per_px = mm_per_px * 0.1
```

---

## 安定性設計

| 問題 | 対応 |
|---|---|
| Z座標ずれ | floor/wall/ceiling Z resolverをsingle source of truthとして維持 |
| Z-fighting | placeholder hide、null material防止、collinear filler skip |
| Blueprint placeholder表示 | BasicShapeMaterial placeholder meshを自動的に非表示 |
| Window fallback表示 | fallback CSG cubeをinvisible処理 |
| Null material | material fallbackまたはspawn skip |
| Wall/opening mismatch | ユーザー確認とwall hashによるopening再検証 |

---

## CAD Export

<p align="center">
  <img src="../../assets/screenshots/technical/05-cad-export.png" width="860" alt="スクリーンショット領域: CAD SVG/PNG export">
</p>
<p align="center"><sub>スクリーンショット領域: CAD SVG/PNG export</sub></p>

> 2D図面の出力結果、SVG/PNG、寸法表示画面を配置してください。

確認済みannotationはCAD形式のSVG/PNGとして出力できます。

対応データ:

- 壁線分
- ドア位置と回転
- 窓線分
- 部屋polygon
- 家具マーカー
- 面積情報
- SVG / PNG export

---

## DLSS / Runtime Rendering

高解像度の室内空間をリアルタイムで確認するため、対応環境では以下を利用できます。

- DLSS upscaling
- Frame Generation対応環境
- dynamic lighting
- SSGI / SSRベースの室内レンダリング
- runtime spawned actor向けのmovable lighting
