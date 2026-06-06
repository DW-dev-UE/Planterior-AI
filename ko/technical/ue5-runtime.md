<p align="right">
  <a href="../../en/technical/ue5-runtime.md">US English</a> &nbsp;|&nbsp;
  <a href="../../ko/technical/ue5-runtime.md">KR 한국어</a> &nbsp;|&nbsp;
  <a href="../../ja/technical/ue5-runtime.md">JP 日本語</a>
</p>


# UE5 런타임 빌더

UE5 런타임 빌더는 검수된 도면 데이터를 바탕으로 벽, 바닥, 천장, 창문, 문, 가구, 조명을 런타임에 절차적으로 생성합니다.

<p align="center">
  <img src="../../assets/screenshots/technical/04-ue5-runtime.png" width="860" alt="UE5 런타임 3D 생성 스크린샷 영역">
</p>
<p align="center"><sub>UE5 런타임 3D 생성 스크린샷 영역</sub></p>

> 생성된 실내 공간, 벽/천장/바닥, 창문/문, 가구, 조명을 보여주는 이미지를 배치하세요.

---

## 핵심 역할

| 모듈 | 역할 |
|---|---|
| Annotation Widget | 사용자 검수 결과를 generation payload로 변환 |
| Wall Generator | 벽, 바닥, 천장, 창문, 룸별 CSG 액터 생성 |
| Runtime CSG Actor | 런타임 박스 기반 procedural mesh 생성 |
| Door Spawner | 문 BP 액터 spawn 및 위치/회전/스케일 적용 |
| AutoPlace Manager | AI 가구 배치 결과를 UE 액터로 변환 |
| Interior Formula | 거실, 침실, 욕실 등 룸별 인테리어 마감 적용 |
| Ceiling Light Library | 룸 크기와 타입에 따라 조명 BP spawn |

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

## 좌표계

| 좌표계 | 단위 | 사용 위치 |
|---|---|---|
| Source Image Pixel | px | 백엔드 검출 및 review payload |
| Widget Local Space | px | UE5 Annotation UI |
| World Centimeter | cm | UE5 런타임 액터 생성 |

```text
mm_per_px = known_length_mm / measured_length_px
cm_per_px = mm_per_px * 0.1
```

---

## 안정성 설계

UE5 런타임 생성에서는 다음 문제가 발생할 수 있기 때문에 방어 로직을 둡니다.

| 문제 | 대응 |
|---|---|
| Z 좌표 오차 | floor / wall / ceiling Z resolver를 single source of truth로 유지 |
| Z-fighting | placeholder mesh hide, null material cube 방지, collinear filler skip |
| BP placeholder 노출 | BasicShapeMaterial 기반 placeholder 자동 hide |
| 창문 fallback 노출 | fallback CSG cube invisible 처리 |
| null material | material fallback 또는 spawn skip |
| wall/opening mismatch | 사용자 검수 및 wall hash 기반 opening 재검수 |

---

## CAD Export

<p align="center">
  <img src="../../assets/screenshots/technical/05-cad-export.png" width="860" alt="CAD SVG/PNG export 스크린샷 영역">
</p>
<p align="center"><sub>CAD SVG/PNG export 스크린샷 영역</sub></p>

> 2D 도면 내보내기 결과, SVG/PNG 출력, 치수선 표시 화면을 배치하세요.

검수된 annotation 데이터는 CAD 형태로 내보낼 수 있습니다.

지원 대상:

- 벽 선분
- 문 위치 및 회전
- 창문 선분
- 방 polygon
- 가구 마커
- 면적 정보
- SVG / PNG export

---

## DLSS / Runtime Rendering

고해상도 실내 공간을 실시간으로 확인하기 위해 지원 환경에서는 다음 렌더링 최적화를 적용할 수 있습니다.

- DLSS 기반 업스케일링
- Frame Generation 지원 환경 대응
- Dynamic Lighting
- SSGI / SSR 기반 실내 표현
- Runtime spawned actor에 맞춘 Movable lighting
