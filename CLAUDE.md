# Fork-specific notes

- This is a fork of openclaw/openclaw deployed on Railway at agent.hypertransient.com.
- Do NOT modify upstream source files (src/**, docs/**, etc.) — only deployment config.
- Railway auto-assigns the PORT env var. Do not override it manually.
- Railway's builder does NOT support BuildKit `--mount=type=cache` directives. After syncing upstream, strip them from the Dockerfile.
- The "update available" message in gateway logs is expected — updates happen by syncing the fork and rebuilding, not via the control UI.

## Syncing from upstream

1. On GitHub: "Sync fork" or merge upstream/main into your main
2. Resolve conflicts by accepting upstream — unless it's a file you intentionally changed for Railway
3. **Check the Dockerfile** for `--mount=type=cache` lines and remove them (Railway rejects these)
4. Push main, Railway rebuilds automatically
