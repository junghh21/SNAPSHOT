# SNAPSHOT

Execution + hosting repo for the [`__GET`](https://github.com/junghh21/__GET)
market scraper.

## Architecture

```
__GET (code + image)                         SNAPSHOT (execution + hosting)
─────────────────────                        ──────────────────────────────
docker-publish.yml  ──build & push──►  ghcr.io/junghh21/venv_get:latest
                                                  │ (pull, private → needs PAT3)
                                                  ▼
                                       capture-30m.yml   every 30 min → main.py  → Telegram
                                       capture-6h.yml    every  6 h   → main1.py → Telegram
                                       pages.yml         → __LANGGRAPH dashboard → GitHub Pages
                                                  │
                                                  └─ each run appends to logs/ci-*.md and pushes
```

- **Image build stays in `__GET`** (`docker-publish.yml`), pushed to GHCR on every push.
- **This repo runs the captures**: each workflow clones the latest `__GET` source,
  runs it inside the `venv_get` image, and posts to Telegram.
- **Static hosting** is here too — `pages.yml` builds the dashboard and deploys to
  <https://junghh21.github.io/SNAPSHOT/>.
- **CI logs** are recorded to `logs/ci-30m.md` / `logs/ci-6h.md` and pushed each run.

## Required secrets

| Secret | Purpose |
|---|---|
| `PAT3` | pull the private `venv_get` GHCR image (packages:read) |
| `TELEGRAM_BOT_TOKEN` / `TELEGRAM_CHAT_ID` | main channel |
| `TELEGRAM_COIN_BOT_TOKEN` / `TELEGRAM_COIN_CHAT_ID` | coin channel |
