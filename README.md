# simple-webpage

A vanilla HTML/CSS/JS reference repository and OpenCode agent skill. No frameworks, no build steps, no bloat.

## Quick Start

1. Open any file in `skills/simple-webpage/cookbook/` in your browser
2. Copy patterns into your project
3. Customize with tokens from `skills/simple-webpage/tokens/`

## Structure

```
simple-webpage/
├── AGENTS.md                    # Internal repo instructions
├── README.md                    # This file
├── skills/
│   └── simple-webpage/
│       ├── SKILL.md             # Agent skill definition (reusable)
│       ├── cookbook/             # Standalone HTML pattern demos
│       │   ├── layouts/
│       │   ├── navigation/
│       │   ├── forms/
│       │   ├── cards/
│       │   ├── effects/
│       │   ├── interactivity/
│       │   └── typography/
│       ├── docs/                # Principles, guides, tutorials
│       │   ├── principles/
│       │   ├── guides/
│       │   └── tutorials/
│       ├── tokens/              # Design token CSS files
│       └── templates/           # Starting point files
└── extraction/                  # Gitignored — video extraction tools
```

## For AI Agents

Load `skills/simple-webpage/SKILL.md` before generating any webpage. It defines:
- Philosophy (ponytail YAGNI ladder)
- Decision framework (when to use what)
- HTML/CSS/JS patterns with code examples
- CSS framework allowlist (classless, shadcn-aesthetic, minimal-class)
- Anti-patterns, design reference, accessibility minimums
- Cookbook index with links to all patterns

## Serving Locally

```bash
python3 -m http.server 8000
# or: npx serve .
# or: php -S localhost:8000
```

## Tech Stack

- SKILL.md · HTML · CSS · JavaScript · Design Tokens
