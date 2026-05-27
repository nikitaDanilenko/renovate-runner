# renovate-runner

Self-hosted Renovate runner. One GitHub Actions workflow runs Renovate on a schedule against the repos listed in `config.js`.

## Setup

### 1. Create a GitHub App

GitHub → Settings → Developer settings → GitHub Apps → **New GitHub App**.

- **Name**: e.g. `nikita-renovate`
- **Homepage URL**: any (e.g. this repo's URL)
- **Webhook**: disable (uncheck "Active")
- **Repository permissions**:
  - Contents: Read & write
  - Issues: Read & write
  - Pull requests: Read & write
  - Workflows: Read & write
  - Metadata: Read-only (auto)
- **Subscribe to events**: none
- **Where can this GitHub App be installed?**: Only on this account

Create. Then:

- Generate a **private key** → downloads a `.pem` file.
- Note the **App ID** (top of the app settings page).
- Install the app on the target repositories (App page → Install App).

### 2. Add secrets/vars to this repo

Settings → Secrets and variables → Actions:

- **Variable** `RENOVATE_APP_ID` = the App ID (numeric).
- **Secret** `RENOVATE_APP_KEY` = full contents of the `.pem` file, including `-----BEGIN`/`-----END` lines.

### 3. Configure target repos

Edit `config.js` and add each repo to the `repositories` array. Each target repo should contain its own `renovate.json` defining update rules.

### 4. Run

- Push to `main` and wait for the cron (`0 5 * * 1`, Mondays 05:00 UTC).
- Or trigger manually: Actions → Renovate → **Run workflow**.

## Notes

- The App's private key never leaves GitHub. The workflow mints a short-lived installation token per run.
- Renovate's own version (`renovatebot/github-action`) is pinned via tag; once running, Renovate will pin it to a SHA on its first PR here.
- Log level can be set to `debug` via manual dispatch when troubleshooting.