# Liquid Glass Web

[中文说明](README.zh-CN.md) · [Live examples](https://voyhuang.github.io/liquid-glass-web/) · [Install](#install)

A standalone, Codex-first [Agent Skill](https://agentskills.io/specification)
for Liquid Glass web interfaces. **v0.1.1** brings size-aware edge refraction,
soft folded reflections, local color dispersion, and clear centers without
whole-pane Gaussian blur. No npm package, Plugin, CDN, or build step.

## Preview

[Open the four examples](https://voyhuang.github.io/liquid-glass-web/).
Pages serves the same single-file HTML examples that ship inside the skill.

## What's new in v0.1.1

- Edge-distance refraction replaces whole-surface noise distortion.
- All four edges and rounded corners share the same optical profile.
- Smaller panes scale displacement, reflection blur, dispersion, and edge fade
  together; large panes retain a 39.2px optical band and a 10px edge treatment.
- Reflections use gentle symmetric ghost samples and localized blur.
- No global glass blur, including the non-Chromium fallback.
- The appearance button opens a menu with Light/Dark controls and a 40–80%
  tint-transparency slider. No numeric readout or helper text.

## Install

Ask Codex:

```text
Use $skill-installer to install https://github.com/voyhuang/liquid-glass-web/tree/v0.1.1/skills/liquid-glass-web
```

Use `main` instead of `v0.1.1` to follow development. Restart Codex if the skill
does not appear immediately. Invoke it with `$liquid-glass-web`.

For manual installation:

```sh
git clone --depth 1 --branch v0.1.1 https://github.com/voyhuang/liquid-glass-web.git
mkdir -p ~/.codex/skills
cp -R liquid-glass-web/skills/liquid-glass-web ~/.codex/skills/liquid-glass-web
```

The copy command assumes the destination does not exist. Back up an existing
installation before replacing it. Install the skill subdirectory, not this
repository root.

## Quick use

1. Copy [glass.css](skills/liquid-glass-web/assets/glass.css) into a style block.
2. Add `.lg` to a few floating surfaces, using `.lg-chip` for controls inside.
3. Copy [refraction-snippet.html](skills/liquid-glass-web/assets/refraction-snippet.html)
   once near the end of the body, before application scripts.
4. Put page-specific CSS after the canonical stylesheet.

```html
<article class="lg lg--thick lg-materialize">
  <h1>A clear surface</h1>
  <button id="themeToggle" class="lg-chip" type="button"
          aria-label="Appearance">◐</button>
</article>
```

The optional `themeToggle` trigger creates the appearance popover. Clicking it
does not change theme immediately. The slider changes tint alpha only; custom
values persist through theme changes, not reloads. Omit the trigger to omit the menu.

## Classes and tokens

| Interface | Purpose |
|---|---|
| `.lg` | Tint, size-aware edge optics, rim and spotlight |
| `.lg--thick`, `.lg--thin` | Surface hierarchy and shadow, not blur strength |
| `.lg-materialize` | Entrance; stagger with `--enter-delay` |
| `.lg-chip`, `.lg-cta` | Non-glass controls and accent variant |
| `.lg-backdrop` | Optional animated background |
| `--lg-radius` | Circular radius; 26px default, 999px for pills |
| `--lg-tint`, `--lg-tint-a` | Tint color channels and opacity |
| `--lg-sat`, `--lg-bright` | Saturation and brightness |
| `--lg-shadow`, `--lg-shadow-sm` | Surface shadows |
| `--sheen-a`, `--spot-max` | Sheen and cursor lighting |
| `--bg`, `--ink`, `--ink-dim`, `--accent` | Theme colors |
| `--lg-blur` | Legacy token, retained at 0px; no material effect |

Override tokens after the asset, not its material rules. Runtime filter IDs and
`--lg-filter` are internal state. Circular corner geometry is intentional;
do not add squircle styling without also changing the optical map.

## Examples

| Example | Contents | Live |
|---|---|---|
| [Quick Start](skills/liquid-glass-web/references/example-quick-start.html) | Minimal nav and card | [Open](https://voyhuang.github.io/liquid-glass-web/skills/liquid-glass-web/references/example-quick-start.html) |
| [Components](skills/liquid-glass-web/references/example-components.html) | Nav, card, chips, standalone CTA, dialog | [Open](https://voyhuang.github.io/liquid-glass-web/skills/liquid-glass-web/references/example-components.html) |
| [Music Player](skills/liquid-glass-web/references/example-music-player.html) | Playback demonstration and progress controls | [Open](https://voyhuang.github.io/liquid-glass-web/skills/liquid-glass-web/references/example-music-player.html) |
| [Résumé / Portfolio](skills/liquid-glass-web/references/example-resume.html) | Sticky navigation, responsive layout, print | [Open](https://voyhuang.github.io/liquid-glass-web/skills/liquid-glass-web/references/example-resume.html) |

Every example embeds the exact canonical CSS and snippet and works offline.
The optical study used during development is not a fifth published example.

## Browser fallbacks

Policy for v0.1.1 (2026-10-06): Chromium receives SVG backdrop refraction through
a conservative runtime gate. Safari/Firefox retain translucent tint and rim
lighting **without whole-pane blur**. This policy is not a fresh cross-engine
certification; engine support can change. The optional menu uses native popovers.

The ladder is **edge refraction → clear tinted surface → solid accessibility
surface**. Reduced transparency, increased contrast, forced colors, and print
override the material. Media-query support varies by browser.

## Accessibility and performance

Keep five panes or fewer per view and avoid glass-on-glass. Preserve visible
focus, accessible names, and contrast over busy backgrounds. The menu supports
Escape and outside-click dismissal; the slider retains native keyboard access.

Maps update on pane resize, not scroll or cursor movement. Each pane has a
distinct filter; closed dialogs initialize when visible. The optical band is
capped at 22% of the shorter dimension so opposite edges stay separated.
Reduced motion stops decorative motion. Print flattens surfaces.

## Troubleshooting

- **No refraction:** check the browser gate, filter IDs, pane size, and ancestors.
- **Lost effect after entrance:** remove a persistent final `filter`; retain
  the canonical animation's `backwards` fill mode.
- **Corner gaps:** preserve concentric circular radii.
- **Muddy center:** remove extra global blur and check tint opacity.
- **Small controls overprocessed:** scale all optical radii together.
- **Slow scroll:** reduce panes and avoid unnecessary compositing layers.

## Validate and contribute

```sh
python3 ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/liquid-glass-web
```

Review the relevant browser, keyboard, responsive, accessibility, and print
behavior. See [CONTRIBUTING.md](CONTRIBUTING.md). Asset changes must update all
four embedded examples and both README languages. There is no build pipeline.

## License and disclaimer

[MIT](LICENSE). Independent and unofficial; not affiliated with or endorsed by
Apple. No Apple code or proprietary artwork is included. The optical profile
is a visual approximation, not Apple's shader or a physically exact simulation.
