# Web Research Agent Memory Index

This file is the index of saved research projects. Add a one-line entry for each project file you create alongside it, with the date the research was done.

## Format

```
- [project_<slug>.md](project_<slug>.md) — <one-line summary>; <key caveat or stale-by date> (researched YYYY-MM-DD)
```

Keep entries dated. Stale research is worse than no research.

## Project

> Replace these example entries with your own. Delete the examples once you have real ones.

- [project_local_plumbers.md](project_local_plumbers.md) — Vetted plumbers within 30 min of the property; primary contact verified by phone (researched YYYY-MM-DD)
- [project_wifi_extenders.md](project_wifi_extenders.md) — Wi-Fi extender / mesh options for the property; pricing and local stockists (researched YYYY-MM-DD)
- [project_local_vendor_recs.md](project_local_vendor_recs.md) — Vetted local vendor contacts for guest concierge requests (researched YYYY-MM-DD)

## Conventions

- One file per research project. Filename pattern: `project_<short_slug>.md`.
- At the top of each file, record: date, the question asked, the answer / recommendation, and the sources.
- Don't ship project files that contain real personal contacts or pricing into a public repo. Keep this folder out of any public commits — `.gitignore` excludes the working `.env`, but project memory files are markdown and will be committed unless you add them to `.gitignore` yourself.
