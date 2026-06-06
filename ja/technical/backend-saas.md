<p align="right">
  <a href="../../en/technical/backend-saas.md">US English</a> &nbsp;|&nbsp;
  <a href="../../ko/technical/backend-saas.md">KR 한국어</a> &nbsp;|&nbsp;
  <a href="../../ja/technical/backend-saas.md">JP 日本語</a>
</p>


# SaaSバックエンド

FloorplanSPのバックエンドはFastAPIベースのSaaSサーバーで、AI間取り解析パイプラインとUE5プロジェクト保存/復元をつなぎます。

<p align="center">
  <img src="../../assets/screenshots/technical/02-saas-backend.png" width="860" alt="スクリーンショット領域: SaaSバックエンドとAPIフロー">
</p>
<p align="center"><sub>スクリーンショット領域: SaaSバックエンドとAPIフロー</sub></p>

> ログイン、プロジェクト一覧、job状態、APIドキュメント、サーバーログなどを配置してください。

---

## サーバーの役割

| 領域 | 役割 |
|---|---|
| Auth | 登録、ログイン、access JWT、refresh token rotation |
| Session | セッション管理、revoke、別端末ログイン検出 |
| Project | プロジェクト作成、保存、読み込み、quota検証 |
| Job | 画像アップロード、job ownership、状態管理 |
| Scale | ユーザー確定scale、`mm_per_px`管理 |
| Structure | wall review、opening review、final annotation保存 |
| Storage | ユーザー別durable storage |
| Security | rate limit、audit log、token masking |

---

## Runtime Pipeline

```text
UE5 Client
	-> Login
	-> Project Create / Load
	-> Floorplan Upload
	-> Scale Confirm
	-> Preprocess
	-> Structure Detect
	-> Wall Confirm
	-> Opening Confirm
	-> Final Annotation Save
```

---

## API Endpoint例

| Method | Path | 説明 |
|---|---|---|
| POST | `/api/v1/auth/login` | ログイン |
| POST | `/api/v1/auth/refresh` | refresh token rotation |
| GET | `/api/v1/projects` | プロジェクト一覧 |
| POST | `/api/v1/projects` | プロジェクト作成 |
| POST | `/api/v1/floorplan/upload` | 間取り画像アップロード |
| POST | `/api/v1/scale/user-confirm` | scale確定 |
| POST | `/api/v1/structure/detect` | 構造認識とwall review開始 |
| POST | `/api/v1/structure/user-confirm` | wallまたはopening確認を保存 |
| GET | `/api/v1/structure/{job_id}` | 保存済み構造確認データを取得 |

---

## Wall -> Opening -> Done Review Flow

```text
structure/detect
	-> next_review = wall

structure/user-confirm, review_stage = wall
	-> confirmed wallsを保存
	-> wall_hashを計算
	-> opening候補を統合
	-> next_review = opening

structure/user-confirm, review_stage = opening
	-> confirmed doors/windowsを保存
	-> final_annotationを生成
	-> next_review = done
```

`wall_hash` により、壁編集後に既存のドア/窓確認データが有効かどうかを判定します。

---

## Storage Policy

```text
data/users/<user_id>/
	account.json
	uploads/
	sessions/
	preprocess/
	structure/
	projects/
```

重要方針:

- `user_id` をcanonical storage keyとして使用
- `slug` は表示またはlegacy fallback用
- すべてのjob/project APIでownership validationを行う
- active scaleはactive jobに紐づく必要がある
- production modeではanonymous default userを使わない

---

## Security

| 方針 | 説明 |
|---|---|
| Access JWT | 短いTTLのaccess token |
| Refresh Rotation | refresh token再利用時にsession familyをrevoke |
| Ownership Check | job/projectアクセス権限を検証 |
| Rate Limit | API濫用を防止 |
| Audit Log | ユーザー操作を記録 |
| Token Masking | token/password/raw auth responseのログ出力を防止 |

---

## Production拡張計画

- SQLite -> PostgreSQL migration
- process-local queue -> Redis / RabbitMQ / SQS
- AI inference worker分離
- object storage連携
- monitoring / tracing / alerting
