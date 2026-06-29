# CI run log

Append-only log written by the capture workflows. Each scheduled run records a
timestamped entry with the tail of its output, then commits + pushes here.

- `ci-30m.md` — 30-minute capture runs (`main.py`)
- `ci-6h.md` — 6-hour capture runs (`main1.py`)
