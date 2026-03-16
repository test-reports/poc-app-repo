# POC App Repo

Dummy repository for triggering workflows. On **push** or **pull_request** to `main`/`master`, this repo **calls a reusable workflow** in **cli-test-automation-poc**. The full run (including tests) appears in **this repo’s Actions** tab so you don’t switch repos. test

## Setup

1. **Target repo**: **cli-test-automation-poc** must exist and contain the reusable workflow `.github/workflows/run-tests-from-app.yml` on its default branch (e.g. `main`).

2. **Secrets in this repo**:
   - **`REPO_DISPATCH_TOKEN`**: PAT with read access to **cli-test-automation-poc** (for checking out and running tests via the reusable workflow).

3. **Allow reuse (if cli-test-automation-poc is private)**  
   In **cli-test-automation-poc**: **Settings → Actions → General → “Access”** → allow access from this repository (or from repositories in your org).

4. **Self-hosted runner for this repo (Option A)**  
   The called workflow runs in **this repo’s** context and uses `runs-on: self-hosted`. Register a self-hosted runner for **poc-app-repo** so the test job is picked up (otherwise it stays on “Waiting for a runner to pick up this job…”).

   **Register a runner for this repository:**
   - In **poc-app-repo** on GitHub: **Settings → Actions → Runners → New self-hosted runner**.
   - Choose OS (e.g. Linux, macOS, Windows) and follow the shown commands on the machine that will run the job (can be the same machine already used for **cli-test-automation-poc**).
   - When asked, select **Repository** and this repo so the runner is assigned to **poc-app-repo**.
   - After the runner is online, workflow runs triggered from this repo will use it for the test job.

   To reuse the same machine as **cli-test-automation-poc**: add a **second** runner on that machine in a new folder, then run the config script for **poc-app-repo** (different token/path). Or use an **organization-level** runner that is allowed to run for both repos.

## Config checklist

| Item | Where | Notes |
|------|--------|--------|
| REPO_DISPATCH_TOKEN | This repo → Settings → Actions → Secrets | Read access to **cli-test-automation-poc** (for checkout). |
| Reusable workflow | **cli-test-automation-poc** | `run-tests-from-app.yml` on default branch |
| Access (private test repo) | **cli-test-automation-poc** → Settings → Actions | Allow this repo to use its workflows |
| Self-hosted runner | **poc-app-repo** → Settings → Actions → Runners | Required so the called workflow’s test job is picked up (runs in this repo’s context) |

## Usage

- **Push** or **pull request** to `main`/`master` → this workflow runs and **calls** the reusable workflow in **cli-test-automation-poc**. The whole run (trigger + tests) appears under **poc-app-repo → Actions**.
- **repository_dispatch** with type `run-tests` to this repo still runs the “Trigger tests” workflow but does **not** call the reusable workflow (no duplicate test run).
