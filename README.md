# POC App Repo

Dummy repository for triggering workflows. On **push** or **pull_request** to `main`/`master`, this repo sends a `repository_dispatch` event to **cli-test-automation-poc**.

## Setup

1. **Target repo**: `cli-test-automation-poc` must exist (same GitHub owner as this repo, or set `TARGET_OWNER` in the workflow).

2. **Secret in this repo**: Add **`REPO_DISPATCH_TOKEN`** (exact name):
   - **Settings → Secrets and variables → Actions → New repository secret**
   - Name: `REPO_DISPATCH_TOKEN`
   - Value: a PAT with `repo` scope (or fine-grained access to the target repo). The token’s account must have access to `cli-test-automation-poc`.

3. **Target repo workflow**: In **cli-test-automation-poc**, a workflow must listen for the event:
   ```yaml
   on:
     repository_dispatch:
       types: [run-tests]
   ```

## Config checklist

| Item | Where | Notes |
|------|--------|--------|
| Secret name | This repo → Settings → Actions → Secrets | Must be exactly `REPO_DISPATCH_TOKEN` |
| Token scope | PAT used for the secret | Classic: `repo`. Fine-grained: access to `cli-test-automation-poc`. |
| Target repo | `cli-test-automation-poc` | Same owner as this repo, or edit `TARGET_OWNER` in `.github/workflows/trigger-tests.yml` |
| Listener workflow | In `cli-test-automation-poc` | Must have `on.repository_dispatch.types: [run-tests]` |

## Usage

- **Push** or **pull request** to `main`/`master` → workflow runs and sends `repository_dispatch` with type `run-tests` to `cli-test-automation-poc`, including `client_payload` (source repo, event, ref, sha, PR number, actor).
- You can also trigger this workflow by sending a `repository_dispatch` with type `run-tests` to this repo (no cross-repo dispatch is sent in that case).
