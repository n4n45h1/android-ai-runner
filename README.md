# android-ai-runner

Ephemeral Android emulator on GitHub Actions, reachable by the OpenCode server
over Tailscale + SSH. Public repo contains only runner infrastructure code.

## What it does

`.github/workflows/android-ai.yml` (manual `workflow_dispatch` only):

1. Joins the tailnet as `android-runner` (via `TAILSCALE_AUTHKEY`).
2. Starts an SSH server bound to the Tailscale IP only.
3. Enables KVM.
4. Boots an Android emulator (API 35, `google_apis`, x86_64, Pixel 7 Pro).
5. Keeps the session alive up to `session_minutes` (max 330) or until
   `/tmp/stop-android-session` exists on the runner.

## Required secret

Repository secret `TAILSCALE_AUTHKEY` (ephemeral, reusable, pre-approved).
Never commit it. This workflow runs only via `workflow_dispatch`, so PRs and
forks can never access secrets.

## Start / stop

```sh
android-start 300     # dispatch + wait until adb is ready
android-stop          # end the session
android-remote status
```
