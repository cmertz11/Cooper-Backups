# BinScout Outstanding Tasks (Sorted by Owner)

Priority key:
- **1 = NOW**
- **2 = NEXT**
- **3 = LATER**
- **4 = NEVERMIND**

Status marker:
- **🔁 = expected to iterate**

## Locked Decisions (Session)

### Core flow
- Training Capture rename: **Compartment Item Training**.
- Retake behavior: **hard delete previous staged image**.
- Capture flow priority: **item-first**.
- Training log clear: **reset stream cursor** so old lines do not repopulate.
- Log visibility retention: **current session only (ephemeral)**.
- Training stream transport: **SSE + polling fallback**.

### Execution strategy shift (locked)
- Primary goal before speed/features: **full/solid data capture with minimal data-loss risk**.
- Execution order now:
  1. **Data-safety guarantees** for item + compartment/item capture flows.
  2. Inference routing/deploy/health/alerts hardening (Task 7).
  3. UX polish and secondary improvements.
- Release gate for capture reliability:
  - No silent drops.
  - Explicit capture states (staged/committed/failed).
  - Recoverable failure paths.
  - Auditability for captured and linked records.

### Task 7 (Inference migration + health + logs)
- Keep inference service **private/internal only** behind API.
- Rollout routing: **service-first with API-local fallback** during transition.
- If API fallback inference occurs in prod: system considered **CRIPPLED**.
- Alerting: **Telegram immediate alert**, then **15-minute digest** while degraded.
- Recovery alerts: **enabled for all degraded causes**.
- Monitoring policy: **periodic + request-time checks** with state machine (`HEALTHY`, `DEGRADED`, `RECOVERING`).
- Recovery stability window: **5 minutes** before returning to healthy.
- Health truth model: **compare DB intent + inference actual**.
- Model handoff: **manifest push triggers pull/load** on inference service.
- Active-button semantics: model is active only after deploy ACK + checks pass.
- Active gate strictness: **strict, no manual override**.
- Golden sample gate: **single known-sample pass required**.
- Golden pass validity: **valid until artifact hash/version changes**.
- Older models: **Deprecated/Inactive (kept)**, manual delete only.
- Dashboard load state colors:
  - **Green**: all required models loaded
  - **Yellow**: partial loaded (show count + missing type + last error snippet)
  - **Red**: none loaded
- Include **last successful inference timestamp per model type**.
- Logs in GUI: **extend existing Logs page**, **SSE live stream**, tail/filter, 7-day query window.
- Log display policy: **redacted by default + temporary guarded raw toggle**.
- Active deploy failure UX: **30s cooldown + visible countdown + inline detailed reason + expandable technical detail**.

### Runtime control
- Routing mode control: `INFERENCE_ROUTING_MODE=hybrid|service_only|api_only`.
- Routing mode control is **admin-only**, exposed in **Web UI**, **applies immediately**.
- Changing routing mode requires typed confirmation: **`confirm`**.

### Progress tracking (hourly updates)
- Include: **Data Capture Readiness %** in every hourly update.
- Definition: confidence-weighted estimate of how close we are to **full/solid data capture** (safe item + compartment/item dataset collection).
- Report format: `Data Capture Readiness: XX%` plus what moved it up/down.

## Chris-Owned (decisions / product calls)

| # | Priority (1/2/3/4) | Task | Status | Blocker / Decision Needed | Chris Thinks | Notes |
|---:|:---:|---|---|---|---|---|
| 1 | 1 | Rename **Training Capture** to better reflect purpose (Compartment Item image training) | Open 🔁 | Pick final label text |  |  |
| 2 | 1 | Ensure capture workflows are optimized for **item + compartment-to-item data collection** | Open 🔁 | Confirm exact “happy path” capture sequence |  |  |
| 3 | 1 | Require known-good image **test gate before model activation** | Open 🔁 | Define pass/fail threshold and required test set |  |  |
| 4 | 1 | Training/Datasets: inspect data in-app without download (stronger UX) | Partial 🔁 | Define minimum inspect feature set (thumbnails, labels, splits, filters) |  |  |
| 5 | 2 | Backups recent runs: make **Detail** a button opening modal/page; improve formatting | Open 🔁 | Decide modal vs page and data fields to show |  |  |
| 6 | 2 | Restore UX safety: add **Restore** button with confirmation modal requiring typed `restore` | Open 🔁 | Confirm final warning copy + role restrictions |  |  |
| 7 | 2 | Backups source of truth: confirm recent runs reflect actual NAS state; include backup size | Open 🔁 | Decide DB cache vs live NAS query strategy |  |  |
| 8 | 2 | Items page: replace/augment polling with events/toasts for changed parts | Open 🔁 | Choose event source + toast rules |  |  |
| 9 | 2 | Validate **Metrics** functionality end-to-end after recent changes | Unverified 🔁 | Need acceptance test cases |  |  |
| 10 | 3 | Settings cleanup: remove Create Account/Subscription/extra links/compartment prefix/organizer model; fix duplicate login/logout buttons | Open 🔁 | Confirm exact list to remove/hide |  |  |
| 11 | 3 | Keep API Key Helper, add global floating **Tools** button (API Key Helper as first modal tool) | Open 🔁 | Define tool list + placement |  |  |
| 12 | 3 | Move Auth control to small login/logout control outside main nav | Open 🔁 | Choose final location |  |  |
| 13 | 3 | Add top menu bar with tools, user, login/logout, date/time | Open 🔁 | Approve header layout |  |  |
| 14 | 3 | Show ENV marker (e.g., DEV) in top header | Open 🔁 | Confirm style/visibility rules |  |  |
| 15 | 4 | Any de-scoped or no-longer-needed work | Pending 🔁 | You mark items as 4 |  |  |

## Cooper-Owned (implementation tasks)

| # | Priority (1/2/3/4) | Task | Status | Blocker / Decision Needed | Chris Thinks | Notes |
|---:|:---:|---|---|---|---|---|
| 1 | 1 | Add/verify robust **Retake** flow during capture (discard previous staged image, keep new one) | Partial 🔁 | Confirm desired discard semantics + audit trail requirement |  |  |
| 2 | 1 | In Training/Training logs, **Clear** should not repopulate immediately with old stream content | Open 🔁 | Decide: clear view only vs clear cursor vs clear backend log source |  |  |
| 3 | 1 | Training run stream: evaluate/implement **push** (SSE/WebSocket) vs polling | Open 🔁 | Choose transport + fallback behavior |  |  |
| 4 | 1 | **Task 7: Inference migration + model-load health + logs to GUI** | Open 🔁 | Finalize handoff semantics, health thresholds, and log plumbing |  | See "Task 7 Subtasks" below |
| 5 | 2 | Restore flow: show progress/wait indicator near button during operation | Partial 🔁 | Define progress granularity (spinner vs staged progress) |  |  |
| 6 | 3 | rembg: make settings editable in UI and persisted in DB (not env-only) | Open 🔁 | Approve config schema + migration approach |  |  |

### Task 7 Subtasks (Inference migration + health + logs)

| Subtask | Priority | Description | Status | Chris Thinks | Notes |
|---:|:---:|---|---|---|---|
| 7.1 | 1 | **Without removing API inference yet**, begin wiring inference endpoints into the Inference Service (dual-path transition). | Open 🔁 |  |  |
| 7.2 | 1 | **Active button pushes model** to Inference API; if push fails, report failure and do **not** mark model Active. | Open 🔁 |  |  |
| 7.3 | 1 | Inference health includes model-load state for dashboard colors: **Green = all models loaded; Red = no models loaded; Yellow = partial (x loaded).** | Open 🔁 |  |  |
| 7.4 | 1 | Add best-practice path to get inference/API logs into Web GUI (tail/stream, filterable, auth-protected). | Open 🔁 |  |  |
| 7.5 | 1 | A model is **not Active** unless at least one local inference test passes against known-image metadata (golden sample check). | Open 🔁 |  |  |

## Parking Lot / Not Yet Itemized (captured from recent context)

| # | Priority (1/2/3/4) | Item | Status | Blocker / Decision Needed | Chris Thinks | Notes |
|---:|:---:|---|---|---|---|---|
| P1 | 2 | Set up **Cooper context backup repo** + periodic auto-commit/push for continuity files. | Open 🔁 | Need repo + auth configured on host. |  | Memory/plan continuity safeguard. |
| P2 | 2 | Prepare **Monday multi-agent launch kit** (operator steps + 3 agent prompts + lead orchestration prompt). | Open 🔁 | Scheduled for Monday evening. |  | Pinned until access window. |
| P3 | 1 | Enforce hourly update format to always include **Data Capture Readiness: XX%**. | Open 🔁 | None. |  | Prevent reporting drift. |
| P4 | 1 | Treat **API inference fallback in prod = CRIPPLED** as explicit release blocker + alert trigger. | Open 🔁 | None (policy locked). |  | Must be visible in runtime contract/checklist. |
| P5 | 1 | Preserve RECOVERING behavior: inference traffic served while under high-cadence checks until stable. | Open 🔁 | None (policy locked). |  | Operational nuance for implementation. |
| P6 | 2 | Add **“Why not Active?”** panel in UI (human reason + technical detail). | Open 🔁 | Confirm final placement. |  | Avoid log-diving for triage. |
| P7 | 2 | Add **one-click rollback to last known good model** (with audit reason). | Open 🔁 | Define guardrails + scope. |  | Complements deprecate/keep policy. |
| P8 | 2 | Add **Tailscale/network guardrail checklist** (inference internal-only, exposure checks). | Open 🔁 | None. |  | Environment safety consistency. |

## Shared (execution + feedback loop)

| # | Priority (1/2/3/4) | Task | Status | Blocker / Decision Needed | Chris Thinks | Notes |
|---:|:---:|---|---|---|---|---|
| 1 | 1 | Rapid loop on capture/data quality: run short capture sessions and tune UX/validation quickly | Open 🔁 | Need quick feedback cadence (e.g., every 30–60 min) |  |  |
