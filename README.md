# The Bot Leagues — Docs

This repository contains a small static site for "The Bot Leagues" (HTML + CSS). The `teams.html` page lists teams and player profiles.

Docs:
- `docs/teams.md` — How to add a team or player, manual and recommended JSON-driven approaches, and tips for previewing locally.

Quick preview:

```bash
# from the project root
python3 -m http.server 8000
# open http://localhost:8000/
```

If you want, I can implement the JSON-driven loader so teams are stored in `data/teams.json` and rendered with a tiny `scripts/teams.js`.
