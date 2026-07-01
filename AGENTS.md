# simple-webpage

## Skill
Load `SKILL.md` for all webpage generation. It defines philosophy, patterns,
framework allowlist, and references to the cookbook.

## Local server
```bash
python3 -m http.server 8000
```

## Key rules
- No build step. No Node/npm/Bun.
- Check SKILL.md §Decision Framework before any design choice.
- CSS framework allowlist: SKILL.md §Framework Allowlist.
- Cookbook patterns in `cookbook/` — browse before writing new CSS.
- Design tokens in `tokens/` — import via `<link>`.
- Prefer single-file HTML for simple pages. Split at ~400 lines.

## Documentation access
- Use Context7 MCP (`resolve-library-id` + `query-docs`) for framework docs when available
- Fallback: fetch documentation directly via `webfetch`
- Preferred frameworks: Basecoat UI (shadcn without React), Pico CSS (classless)
