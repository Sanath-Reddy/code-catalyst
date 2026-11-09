# BrandGuard · CODE CATALYST

**Check the copy. Follow the evidence. Approve the exact revision.**

BrandGuard is a pre-publication review workspace for marketing campaigns. It checks draft copy and ad graphics against a brand's approved guidelines, links every finding to its source evidence, and puts a separate human reviewer in charge of the final approval. Local checks cover terminology, structured facts, numbers, and image text. Nebius adds semantic review for tone, messaging, and claims rules.

**[Open the hosted demo →](https://code-catalyst-tau.vercel.app/)**

Judges don't need to install anything. On the sign-in page, click **Try a private demo**. You get your own fictional LumaDesk workspace with sample campaigns already in it, and you can switch roles from the top bar. The floating **Judge guide** has the walkthrough and a **Round 3 missions** tab, before and after you sign in.

[Hosted demo](#hosted-demo) · [Judge walkthrough](#judge-walkthrough) · [Features](#what-the-project-supports) · [Run locally](#run-locally) · [Architecture](#architecture) · [Evaluation](#evaluation) · [Testing](#testing)

## Hosted demo

### Private demo (recommended)

1. Open **https://code-catalyst-tau.vercel.app/**.
2. Click **Try a private demo** and pick a role to start with.
3. Use the role switcher in the top bar to move between **Creator**, **Reviewer**, **Manager**, **Brand lead**, and **Admin**. Each role is a separate identity in the same sandbox, so approvals stay independent of the creator.

Sandboxes come from a ready pool, so they open straight away. Each one has its own analysis budget, so nothing you do touches other judges' work.

### Shared evaluation accounts

You can also use the shared fictional LumaDesk workspace:

| Role | Email |
|---|---|
| Creator | `creator@lumadesk.test` |
| Manager | `manager@lumadesk.test` |
| Reviewer | `reviewer@lumadesk.test` |
| Admin | `admin@lumadesk.test` |

All four use the password `BrandGuardAdmin2026!`. The workspace ID (`539ad43a-e582-46f0-8d0d-7e483782d5c6`) is optional, because sign-in finds the workspace from the email when an account belongs to only one. Give your campaign a unique title so you can find it again, and save before you **Sign out** to switch roles.

The local launcher generates its own accounts. For localhost, run `python3 scripts/dev_local.py accounts` instead.

## Judge walkthrough

Do these steps in order and keep the same campaign throughout.

### 1. Creator: from rough draft to corrected revision

1. Click **Review a campaign**. Choose the **LumaDesk** brand, enter a title, and assign the reviewer. Set **Channel** to `email`, **Audience** to `Prospective customers`, and **Purpose** to `Promote a trial`.
2. Paste this copy, keep **external semantic review** selected, and submit:

   ```text
   Lumadesk guarantees 10x productivity. Get your 30-day free trial. Sign up NOW!!!
   ```

3. Look at the findings: the brand spelling and the trial length break approved rules, and Nebius flags the unsupported performance claim and the pushy tone. The model's wording can differ between runs. Click **Evidence ↗** to see the approved passage behind a finding.
4. Click **Apply available fixes** (this applies only suggestions that are exact drop-in replacements), or click **Edit draft** and enter `Try LumaDesk free for 14 days. Start your trial.`
5. Click **Save all & recheck**, then **Submit for Review →** and **Confirm Submission →**.

You can also upload a **PDF or Word (.docx)** draft (the server extracts it and records page and paragraph provenance), or a **PNG/JPEG ad**. For an ad, local OCR reads the text, you correct it, and findings link back to image regions alongside contrast and brand-palette checks. A scanned PDF can be checked as an image campaign.

### 2. Manager: where the rules come from

1. Open **LumaDesk** under **Your brand library**, choose **Active**, then click **View Rules**.
2. Compare the rules with the creator's findings:

| Sample phrase | Approved rule | Check |
|---|---|---|
| `Lumadesk` | The product name is `LumaDesk`. | Local terminology |
| `30-day free trial` | The free trial lasts **14 days**. | Local fact/number |
| `10x productivity` | Performance claims require approved evidence. | Nebius semantic |
| `Sign up NOW!!!` | Use calm language. | Nebius semantic |

3. Open **Guideline health** to see which rules get disputed or reported as ambiguous most often. From there you can raise an **improvement request**: attach examples, propose new wording, run a **comparison** of the old rule against the new one on stored campaigns, and send it for peer review. A second manager, such as the **Brand lead**, has to approve the exact draft before it goes live.
4. When a guideline changes and is published, the **Rechecks** inbox shows the old and new rule and lists every campaign that relied on it. Each campaign is rechecked automatically. Campaigns with a confirmed or likely impact go back to a reviewer. Campaigns with only a possible impact keep their approval, unless the recheck finds something different.

To write a new source, click **＋ Add guideline source**. When `RULE_DRAFTING_ENABLED=1` is set, you can also start from an AI draft of an imported brand book, where every rule's evidence quote is checked against the source text. Activation always needs a rationale and an explicit verification checkbox.

<details>
<summary>If there is no active source: recreate the fictional guideline</summary>

Source key `brand_guide`, title `LumaDesk synthetic brand rules`, effective date before the next analysis, and source text:

```text
Write LumaDesk. The free trial lasts 14 days. Performance claims require approved evidence. Use calm language.
```

| ID | Category | Kind | Key | Approved value | Evidence quote |
|---|---|---|---|---|---|
| T01 | `terminology` | `terminology` | `product_name` | `LumaDesk` | `Write LumaDesk.` |
| F01 | `facts_claims` | `fact` | `trial_duration` | `14 days` | `The free trial lasts 14 days.` |
| C01 | `facts_claims` | `claim_policy` | `promotions` | `Substantiate performance claims.` | `Performance claims require approved evidence.` |
| G01 | `tone` | `guidance` | `tone` | `Use calm language.` | `Use calm language.` |

The exact fixture is in [verification/fixtures/lumadesk.json](verification/fixtures/lumadesk.json).

</details>

### 3. Reviewer: approve the exact revision

1. Open the campaign and confirm you are looking at the current, corrected revision.
2. If **Manual category review** appears, record each outstanding check: tick the source you checked, choose an outcome, and write a rationale of at least 20 characters.
3. Click **✓ Approve Exact Revision**, write a rationale, then **✓ Confirm Approval**.
4. Click **⬇ View Approved Export** and download the approved text and the **Audit Record (.json)**.
5. Open **Verify a record** (linked from the export, or at `/verify`) and upload the JSON. It checks the record against the stored decision, text, evidence and image, and tells you whether the approval is still current.

### 4. Admin: processing policy

Click **Processing policy** in the top bar. It contains:

- The Nebius switch, with a creator disclosure notice and an audit rationale.
- A provider status card showing the provider, the model, and whether the key is configured.
- The **retention period**. Inactive campaigns are purged hourly, and a deletion ledger keeps them deleted after a restore.

Changing the policy doesn't rerun old analyses or change an approved export. Please leave external processing **enabled** for the next judge.

## Troubleshooting

| What you see | What to do |
|---|---|
| Banner saying the server is waking up | The hosted backend is starting. The page retries on its own. |
| Sign-in fails | Copy the email and password exactly, or use **Try a private demo**. Local accounts are different: run `python3 scripts/dev_local.py accounts`. |
| **Local processing only** | As admin, enable the Processing policy. Then run a new check as creator with external review selected. |
| Provider not configured | The deployment owner must set `NEBIUS_API_KEY` on the API and worker. The browser never receives the key. |
| **INCOMPLETE** with no findings | Zero findings doesn't mean every category was covered. The reviewer can record the outstanding manual checks. |
| Image text is empty | The worker needs Tesseract. If `OCR_ENABLED=0`, type the text by hand. |
| Hourly limit reached | Each member has hourly caps on drafts, analysis runs, and uploads. Wait, or use another role. |
| No approval button | You must be the assigned reviewer, the current revision must be submitted, and the coverage and blocker requirements must be met. |
| Run or approval is stale | A guideline changed. Recheck the current revision, or follow it from the **Rechecks** inbox. |

## What the project supports

- **Guidelines.** Versioned guideline sources per brand, with manager-controlled activation and optional allowed and forbidden spellings. AI drafting from a brand book is optional, and every drafted rule is quote-checked.
- **Intake.** Pasted text, `.txt`/`.md` files, PDF and DOCX imports (up to 10 MB, 50 pages, and 100,000 characters) with file provenance, and PNG/JPEG ad graphics (up to 8 MB) with local OCR.
- **Checks.** Local terminology, fact, and number checks; image contrast and brand-palette checks per region; and Nebius semantic review. Semantic review retries transient errors and repairs rejected findings once. Proposals that fail validation are shown to the reviewer as unverified suggestions.
- **Review.** Located findings with evidence, category coverage, filters, dispositions, staged edits with undo, revision diffs, conflict recovery, reviewer assignment, and per-revision discussion.
- **Approval.** Separate creator and reviewer identities, manual checks, approval tied to an exact revision, text and JSON export, and a page that verifies a downloaded record.
- **Guideline change.** Each change is recorded with its impact. Only the campaigns that relied on it are reopened, through the Rechecks inbox. Guideline health and improvement requests include a comparison run and peer-reviewed activation.
- **Operations.** Private demo sandboxes with a role switcher, retention and purge, per-member hourly quotas, strict security headers (CSP, HSTS), health endpoints that report the migration head, the worker heartbeat, and the deployed commit, and a synthetic journey that runs every 30 minutes.

## Run locally

### Prerequisites

- Linux or macOS, Python **3.11+**, Node.js **22+**, and npm.
- PostgreSQL server tools: `initdb`, `pg_ctl`, `createdb`, and `psql`.
- Optional: **Tesseract** (`tesseract-ocr`, English data) for image campaigns.
- Optional: a Nebius API key for live semantic review. Local and manual review work without one.

### Start

```bash
git clone https://github.com/Sanath-Reddy/code-catalyst.git
cd code-catalyst
python3 -m venv .venv
.venv/bin/python -m pip install -r apps/api/requirements.lock
npm --prefix apps/web ci
python3 scripts/dev_local.py
```

Open **http://localhost:5183** and get the generated accounts with `python3 scripts/dev_local.py accounts`.

The launcher sets up everything on first run:

- It starts its own loopback PostgreSQL and applies the migrations.
- It creates a fictional workspace with an approved source.
- It builds the frontend and starts the API and worker.

Data and logs persist under the ignored `.local/dev/` folder. Press **Ctrl+C** to stop. Useful options:

```bash
python3 scripts/dev_local.py --local-only
```

```bash
python3 scripts/dev_local.py --db-port 55442 --api-port 8014 --web-port 5184
```

For live Nebius review, copy [.env.example](.env.example) to `.env` and set `NEBIUS_API_KEY`. Never put the key in a `VITE_*` variable. There is more detail in [docs/LOCAL_DEVELOPMENT.md](docs/LOCAL_DEVELOPMENT.md).

## Architecture

```mermaid
flowchart LR
    Web[React review workspace] --> API[FastAPI: identity, workflow, policy]
    API --> DB[(PostgreSQL: sources, revisions, jobs, evidence)]
    DB --> Worker[Supervised worker]
    Worker --> Local[Deterministic engine + Tesseract OCR]
    Worker -->|Policy enabled and selected| Nebius[Nebius semantic review]
    Local --> Validation[Server validates findings and quotes]
    Nebius --> Validation
    Validation --> DB
    API --> Human[Assigned human reviewer]
    Human --> Export[Approved revision, audit record, verify page]
```

| Layer | Technology and responsibility |
|---|---|
| Frontend | React, TypeScript, React Router, and Vite, hosted on Vercel with `/api` rewrites and strict headers |
| API | Python, FastAPI, Pydantic, SQLAlchemy, and Alembic (13 migrations). It holds the authoritative permissions and workflow checks. |
| Worker | A separate supervised service with a heartbeat. It runs analyses, OCR, rechecks, retention purges, and the demo pool. |
| Review engine | Extraction and deterministic rules in `packages/review_engine`. |
| Semantic review | Nebius Token Factory, `Qwen/Qwen3-235B-A22B-Instruct-2507` (the evaluated alternative is `Qwen/Qwen3-30B-A3B-Instruct-2507`) |
| Hosting | [render.yaml](render.yaml) defines the API and worker on Render with pre-deploy migrations, and [vercel.json](vercel.json) configures the frontend. Docker Compose and Caddy files are in `infra/`. |

Nebius only proposes findings. The server validates every quote and citation, and the final decision belongs to a human reviewer.

### Repository structure

```text
apps/web/                 React frontend and Playwright tests (e2e/)
apps/api/brandguard/      API: identity, sources, review, images, documents, impact, requests, retention, demo
apps/api/migrations/      Alembic migrations
apps/api/tests/           API and database integration tests
apps/worker/              Background worker
packages/review_engine/   Extraction and deterministic checks
evaluation/               Frozen benchmarks, fixtures, and committed results
contracts/openapi.json    API contract
verification/             Fixtures, requirement register, and recorded evidence
tests/                    Engine, worker, evaluation, and infrastructure tests
scripts/                  Local launcher, smoke and synthetic checks, load test, packaging
infra/                    Dockerfiles, Compose, Caddy, entrypoint, backup and restore
docs/                     Architecture, contracts, walkthrough, and project records
```

## Configuration

[.env.example](.env.example) lists every setting. The main ones are:

| Setting | Purpose |
|---|---|
| `DATABASE_URL` | PostgreSQL connection string. |
| `PUBLIC_ORIGIN` | Must match the browser's real origin, for session and CSRF checks. |
| `APP_ENV` | `development` or `production`. |
| `NEBIUS_API_KEY` | Server-side provider key. |
| `MODEL_*` | Provider, model, timeout, input and output limits, and the daily run limit. |
| `TRUST_PROXY_HEADERS=1` | Use only behind a proxy that overwrites `X-Forwarded-For` (Vercel → Render). |
| `OCR_ENABLED` | Set to `0` to turn off image OCR. |
| `DEMO_SANDBOXES_ENABLED` / `DEMO_POOL_SIZE` | Turn private demo sandboxes on or off, and set how many are kept ready. |
| `RULE_DRAFTING_ENABLED=1` | Turns on AI rule drafting from brand books. Off by default. |
| `SEED_PASSWORD` | When set, seeds the shared evaluation workspace on deploy. |
| `GIT_COMMIT_SHA` | Reported by `/api/v1/health/version`. Render sets its own equivalent. |

Health endpoints: `/api/v1/health/ready`, `/api/v1/health/worker`, and `/api/v1/health/version`.

## Evaluation

The benchmarks are frozen: each dataset's SHA is committed before it is run. The data is synthetic and fictional, and the results are in [evaluation/results/](evaluation/results/).

| Benchmark | Command | Result |
|---|---|---|
| Text review v2: 60 campaigns, held-out split | `python -m evaluation.benchmark_v2` | Precision 1.00 (CI 0.90–1.00), recall 0.97 (CI 0.86–1.00). No false "clean" result on the held-out split. |
| Guideline impact: 30 campaigns × 5 changes | `python -m evaluation.impact_benchmark` | 53 of 85 re-reviews avoided (62%) compared with invalidating the whole brand. 1 labelled approval missed. |
| OCR: 20 synthetic ad graphics | `python -m evaluation.ocr_benchmark` | Character error rate 0.136, word error rate 0.144. Lines read exactly: 79% overall, 44% when contrast is below 3:1. |
| Comparison runner: 24 seeded regressions | `python -m evaluation.comparison_benchmark` | 24 of 24 caught, 0 false alarms. |

## Testing

```bash
npm --prefix apps/web run build && npm --prefix apps/web run lint
```

The Python suite needs a **separate, disposable** UTF-8 PostgreSQL database, because it resets fixtures:

```bash
export TEST_DATABASE_URL='postgresql+psycopg://USER:PASSWORD@127.0.0.1:5432/brandguard_s1_test'
DATABASE_URL="$TEST_DATABASE_URL" .venv/bin/alembic -c apps/api/alembic.ini upgrade head
DATABASE_URL="$TEST_DATABASE_URL" .venv/bin/pytest apps/api/tests tests -q
```

For the browser tests, prepare the stack described in [tests/e2e/README.md](tests/e2e/README.md), then run:

```bash
npm --prefix apps/web exec -- playwright test --config apps/web/playwright.config.ts --reporter=list
```

The live Nebius test (`LIVE_NEBIUS=1 … nebius.spec.ts`) is opt-in and makes a real, paid provider request.

[GitHub Actions](https://github.com/Sanath-Reddy/code-catalyst/actions) runs four workflows:

- **Backend integration**: migrations, `alembic check`, and pytest on PostgreSQL 18.
- **Planning coverage**: checks the requirement-to-evidence mapping.
- **Dependency audit**: `pip-audit` and `npm audit`, weekly and on every push.
- **Synthetic journey**: every 30 minutes, once the `BRANDGUARD_URL` repository variable is set.

Dependabot keeps the dependencies up to date.

## Scope and limitations

- This is a hackathon prototype that uses fictional data. Model suggestions, and even valid citations, still need human judgement.
- OCR is less reliable on low-contrast text, and uncertain regions stay flagged for a person to check. Logo placement is not certified automatically.
- A zero-finding result doesn't prove full coverage or compliance. Missing, failed, or stale checks stay visible.
- The JSON export is an audit record that the verify page can check. It is not a digitally signed certificate, and it doesn't prove a marketing claim is true.
- Benchmark numbers come from small synthetic datasets with confidence intervals. They are not independent accuracy measurements.
- Historical PDFs, handoffs, and verification reports describe the revision they were written for.

## Further documentation

- [Judge walkthrough and deployment notes](docs/JUDGE_WALKTHROUGH.md) · [Local development](docs/LOCAL_DEVELOPMENT.md) · [Browser verification](tests/e2e/README.md)
- [Architecture](docs/ARCHITECTURE.md) · [Contracts](docs/CONTRACTS.md) · [Model plan](docs/MODEL_PLAN.md) · [Experiments](docs/EXPERIMENTS.md)
- [Round 3 improvements](docs/ROUND3_IMPROVEMENTS.md) · [Completed features](docs/COMPLETED_FEATURES.md) · [Changes from Round 1](docs/CHANGELOG_FROM_ROUND1.md)
- [Disclosures](docs/DISCLOSURES.md) · [Contributions](docs/CONTRIBUTIONS.md) · [Requirement register](verification/requirements.json)

## Team

Built by **CODE CATALYST**:

- **Sanath Reddy**: backend, workflow and security boundaries, integration, and verification.
- **Darshilkumar**: frontend, source and review interfaces, and the judge-facing experience.
- **Surya Prakash**: extraction, deterministic checks, OCR, the worker, and infrastructure.

## Feedback

Report problems in [GitHub Issues](https://github.com/Sanath-Reddy/code-catalyst/issues). Include the role, the steps you took, what you expected and what happened, and a request ID if you have one. Don't include provider keys or private data.

## License

[MIT](LICENSE). Third-party licences and disclosures are recorded in [docs/DISCLOSURES.md](docs/DISCLOSURES.md).
