AGENTS.md
# Fork-specific notes

- This is a fork of openclaw/openclaw deployed on Railway.
- Gateway port changed from upstream default 18789 to 34683.

## Modified files (port change)

- `Dockerfile` line ~248: healthcheck fallback changed from `process.env.PORT||18789` to `process.env.PORT||34683`
- `Dockerfile` line ~252: CMD changed to `--port ${PORT:-34683}` (upstream has no explicit --port flag)
- `docker-compose.yml` line ~25: port mapping changed to `${OPENCLAW_GATEWAY_PORT:-34683}:34683`
- `docker-compose.yml` line ~37: gateway command `--port` changed to `34683`
- `docker-compose.yml` line ~45: healthcheck URL changed to `127.0.0.1:34683`

## How the port works

- Railway auto-assigns PORT env var — the gateway uses it when set.
- If PORT is not set, falls back to 34683 (our custom default).
- Healthcheck reads the same PORT env var with the same fallback, so they always match.

## Rebasing from upstream

1. `git fetch upstream && git merge upstream/main` (on your `main` branch)
2. `git rebase main` (on feature branch)
3. Conflicts will only be in Dockerfile and docker-compose.yml on port-related lines.
4. Always keep port **34683** — reject upstream's 18789 on those lines.
5. Accept all other upstream changes (apt cache mounts, build args, etc.).
