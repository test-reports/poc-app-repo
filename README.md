# POC App Repo

Dummy repository for triggering workflows. On **push** or **pull_request** to `main`/`master`, this repo **calls a reusable workflow** in **cli-test-automation-poc**. The full run (including tests) appears in **this repo’s Actions** tab so you don’t switch repos.

## Setup

1. **Target repo**: **cli-test-automation-poc** must exist and contain the reusable workflow `.github/workflows/run-tests-reusable.yml` on its default branch (e.g. `main`).

2. **Secret in this repo**: Add **`REPO_DISPATCH_TOKEN`** (exact name):
   - **Settings → Secrets and variables → Actions → New repository secret**
   - Name: `REPO_DISPATCH_TOKEN`
   - Value: a PAT with `repo` scope (or fine-grained read access to **cli-test-automation-poc**). Used so the called workflow can checkout the test repo.

3. **Allow reuse (if cli-test-automation-poc is private)**  
   In **cli-test-automation-poc**: **Settings → Actions → General → “Access”** → allow access from this repository (or from repositories in your org).

## Config checklist

| Item | Where | Notes |
|------|--------|--------|
| Secret name | This repo → Settings → Actions → Secrets | Must be exactly `REPO_DISPATCH_TOKEN` |
| Token scope | PAT used for the secret | Read access to **cli-test-automation-poc** (for checkout in called workflow). |
| Reusable workflow | **cli-test-automation-poc** | `run-tests-reusable.yml` on default branch |
| Access (private test repo) | **cli-test-automation-poc** → Settings → Actions | Allow this repo to use its workflows |

## Usage

- **Push** or **pull request** to `main`/`master` → this workflow runs and **calls** the reusable workflow in **cli-test-automation-poc**. The whole run (trigger + tests) appears under **poc-app-repo → Actions**.
- **repository_dispatch** with type `run-tests` to this repo still runs the “Trigger tests” workflow but does **not** call the reusable workflow (no duplicate test run).
