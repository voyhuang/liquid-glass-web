# Contributing

Keep this repository static and self-contained: no runtime dependency, npm
publication, CDN, web font, Plugin manifest, or build requirement.

- Prefer design-token overrides over changes to the optical profile.
- Keep all four examples offline-capable and at five panes or fewer per view.
- Keep English and Chinese README sections aligned.
- When changing canonical CSS or the snippet, update every embedded example.
- Preserve clear centers, edge-local processing, and the shared size scaling.
- Verify theme/menu controls, dialog opening, unique filter IDs, responsive
  sizes, and relevant accessibility/print behavior.

Validate the installable skill with Codex's local validator when available:

```sh
python3 ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/liquid-glass-web
```

Contributions are licensed under this repository's MIT License.
