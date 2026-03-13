# POC App Repo

Dummy repository for triggering workflows. On **push** or **pull_request** to `main`/`master`, this repo **calls a reusable workflow** in **cli-test-automation-poc**. The full run (including tests) appears in **this repo’s Actions** tab so you don’t switch repos.

## Setup

1. **Target repo**: **cli-test-automation-poc** must exist and contain the reusable workflow `.github/workflows/run-tests-reusable.yml` on its default branch (e.g. `main`).

2. **Secrets in this repo**:
   - **`REPO_DISPATCH_TOKEN`**: PAT with read access to **cli-test-automation-poc** (for checkout in the called workflow).
   - **`RP_API_KEY`** (optional): Report Portal API key. When set, the called workflow runs pytest with `--reportportal` so results are sent to Report Portal.

3. **Allow reuse (if cli-test-automation-poc is private)**  
   In **cli-test-automation-poc**: **Settings → Actions → General → “Access”** → allow access from this repository (or from repositories in your org).

## Config checklist

| Item | Where | Notes |
|------|--------|--------|
| REPO_DISPATCH_TOKEN | This repo → Settings → Actions → Secrets | Read access to **cli-test-automation-poc** (for checkout). |
| RP_API_KEY (optional) | This repo → Settings → Actions → Secrets | Report Portal API key; when set, tests report to RP. |
| Reusable workflow | **cli-test-automation-poc** | `run-tests-reusable.yml` on default branch |
| Access (private test repo) | **cli-test-automation-poc** → Settings → Actions | Allow this repo to use its workflows |

## Usage

- **Push** or **pull request** to `main`/`master` → this workflow runs and **calls** the reusable workflow in **cli-test-automation-poc**. The whole run (trigger + tests) appears under **poc-app-repo → Actions**.
- **repository_dispatch** with type `run-tests` to this repo still runs the “Trigger tests” workflow but does **not** call the reusable workflow (no duplicate test run).
