# BinScout Outstanding Tasks (Prioritized)

Priority key:
- **1 = NOW**
- **2 = NEXT**
- **3 = LATER**
- **4 = NEVERMIND**

| # | Priority (1/2/3/4) | Task | Status | Blocker / Decision Needed | Chris Thinks | Notes |
|---:|:---:|---|---|---|---|---|
| 1 | 1 | Rename **Training Capture** to better reflect purpose (Compartment Item image training) | Open | Pick final label text |  |  |
| 2 | 1 | Add/verify robust **Retake** flow during capture (discard previous staged image, keep new one) | Partial | Confirm desired discard semantics + audit trail requirement |  |  |
| 3 | 1 | Ensure capture workflows are optimized for **item + compartment-to-item data collection** | Open | Confirm exact “happy path” capture sequence |  |  |
| 4 | 1 | In Training/Training logs, **Clear** should not repopulate immediately with old stream content | Open | Decide: clear view only vs clear cursor vs clear backend log source |  |  |
| 5 | 1 | Training run stream: evaluate/implement **push** (SSE/WebSocket) vs polling | Open | Choose transport + fallback behavior |  |  |
| 6 | 1 | **CountGD red** on dashboard: complete migration/health integration with inference service | Open | Need expected target architecture + health criteria |  |  |
| 7 | 1 | Require known-good image **test gate before model activation** | Open | Define pass/fail threshold and required test set |  |  |
| 8 | 1 | Training/Datasets: inspect data in-app without download (stronger UX) | Partial | Define minimum inspect feature set (thumbnails, labels, splits, filters) |  |  |
| 9 | 2 | Backups recent runs: make **Detail** a button opening modal/page; improve formatting | Open | Decide modal vs page and data fields to show |  |  |
| 10 | 2 | Restore UX safety: add **Restore** button with confirmation modal requiring typed `restore` | Open | Confirm final warning copy + role restrictions |  |  |
| 11 | 2 | Restore flow: show progress/wait indicator near button during operation | Partial | Define progress granularity (spinner vs staged progress) |  |  |
| 12 | 2 | Backups source of truth: confirm recent runs reflect actual NAS state; include backup size | Open | Decide DB cache vs live NAS query strategy |  |  |
| 13 | 2 | Items page: replace/augment polling with events/toasts for changed parts | Open | Choose event source + toast rules |  |  |
| 14 | 2 | Validate **Metrics** functionality end-to-end after recent changes | Unverified | Need acceptance test cases |  |  |
| 15 | 3 | Settings cleanup: remove Create Account/Subscription/extra links/compartment prefix/organizer model; fix duplicate login/logout buttons | Open | Confirm exact list to remove/hide |  |  |
| 16 | 3 | rembg: make settings editable in UI and persisted in DB (not env-only) | Open | Approve config schema + migration approach |  |  |
| 17 | 3 | Keep API Key Helper, add global floating **Tools** button (API Key Helper as first modal tool) | Open | Define tool list + placement |  |  |
| 18 | 3 | Move Auth control to small login/logout control outside main nav | Open | Choose final location |  |  |
| 19 | 3 | Add top menu bar with tools, user, login/logout, date/time | Open | Approve header layout |  |  |
| 20 | 3 | Show ENV marker (e.g., DEV) in top header | Open | Confirm style/visibility rules |  |  |
| 21 | 4 | Any de-scoped or no-longer-needed work | Pending | You mark items as 4 |  |  |
