<p align="right">
  <a href="../../en/technical/backend-saas.md">US English</a> &nbsp;|&nbsp;
  <a href="../../ko/technical/backend-saas.md">KR 한국어</a> &nbsp;|&nbsp;
  <a href="../../ja/technical/backend-saas.md">JP 日本語</a>
</p>


# SaaS Backend

The FloorplanSP backend is a FastAPI-based SaaS server that connects the AI floorplan pipeline with UE5 project persistence and review workflows.

<p align="center">
  <img src="../../assets/screenshots/technical/02-saas-backend.png" width="860" alt="Screenshot slot: SaaS backend dashboard and API flow">
</p>
<p align="center"><sub>Screenshot slot: SaaS backend dashboard and API flow</sub></p>

> Place screenshots of login, project list, job state, API docs, or server logs here.

---

## Responsibilities

| Area | Role |
|---|---|
| Auth | Registration, login, access JWT, refresh token rotation |
| Session | Session management, revocation, signed-in-elsewhere detection |
| Project | Project create/save/load and quota validation |
| Job | Image upload, job ownership, status management |
| Scale | User-confirmed scale and `mm_per_px` management |
| Structure | Wall review, opening review, and final annotation storage |
| Storage | Per-user durable storage |
| Security | Rate limiting, audit logging, and token masking |

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

## Example API Endpoints

| Method | Path | Description |
|---|---|---|
| POST | `/api/v1/auth/login` | Login |
| POST | `/api/v1/auth/refresh` | Refresh token rotation |
| GET | `/api/v1/projects` | Project list |
| POST | `/api/v1/projects` | Create project |
| POST | `/api/v1/floorplan/upload` | Upload floorplan image |
| POST | `/api/v1/scale/user-confirm` | Confirm scale |
| POST | `/api/v1/structure/detect` | Start structure detection and wall review |
| POST | `/api/v1/structure/user-confirm` | Save wall or opening review |
| GET | `/api/v1/structure/{job_id}` | Load saved structure review |

---

## Wall -> Opening -> Done Review Flow

```text
structure/detect
	-> next_review = wall

structure/user-confirm, review_stage = wall
	-> save confirmed walls
	-> compute wall_hash
	-> merge opening candidates
	-> next_review = opening

structure/user-confirm, review_stage = opening
	-> save confirmed doors/windows
	-> generate final_annotation
	-> next_review = done
```

The server uses `wall_hash` to detect whether previously reviewed openings are still valid after wall edits.

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

Key policies:

- `user_id` is the canonical storage key
- `slug` is only for display or legacy fallback
- every job/project API requires ownership validation
- active scale must belong to the active job
- anonymous default users are disabled in production mode

---

## Security

| Policy | Description |
|---|---|
| Access JWT | Short-lived access token |
| Refresh Rotation | Revokes session family on refresh token reuse |
| Ownership Check | Validates every job/project access |
| Rate Limit | Protects APIs from abuse |
| Audit Log | Records user actions |
| Token Masking | Prevents token/password/raw auth response logging |

---

## Production Extension Plan

- SQLite -> PostgreSQL migration
- process-local queue -> Redis / RabbitMQ / SQS
- dedicated AI inference workers
- object storage integration
- monitoring / tracing / alerting
