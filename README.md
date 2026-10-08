# Liquid Glass Web

[中文说明](README.zh-CN.md) · [Live examples](https://voyhuang.github.io/liquid-glass-web/) · [Install](#install)

A standalone, Codex-first [Agent Skill](https://agentskills.io/specification)
for Liquid Glass web interfaces. **v0.1.2** brings dual-path edge refraction,
soft folded reflections, local color dispersion, and clear centers without
whole-pane Gaussian blur. No npm package, Plugin, CDN, or build step.

## Preview

[Open the four examples](https://voyhuang.github.io/liquid-glass-web/).
Pages serves the same single-file HTML examples that ship inside the skill.

## What's new in v0.1.2

- Native backdrop optics on Chromium; declared-source copies on Safari.
- Lightweight bottom-layer containers and one draggable glass navigation selection.
- 2× optical maps for large floating surfaces that cover text or graphics;
  compact surfaces retain 1× maps.
- Clear centers, shared edge optics, neutral saturation/brightness, and 0.5px borders.
- Dark border/top-highlight alpha: 0.10 / 0.20; overlay transparency stays at 90%.
- The appearance button opens a menu with Light/Dark controls and a 5–95% (40% initial)
  tint-transparency slider. No numeric readout or helper text.

## Install

The installation below is pinned to the v0.1.2 preview release.

Ask Codex:

```text
Use $skill-installer to install https://github.com/voyhuang/liquid-glass-web/tree/v0.1.2/skills/liquid-glass-web
```

Use `main` instead of `v0.1.2` to follow development. Restart Codex if the skill
does not appear immediately. Invoke it with `$liquid-glass-web`.

For manual installation:

```sh
git clone --depth 1 --branch v0.1.2 https://github.com/voyhuang/liquid-glass-web.git
mkdir -p ~/.codex/skills
cp -R liquid-glass-web/skills/liquid-glass-web ~/.codex/skills/liquid-glass-web
```

The copy command assumes the destination does not exist. Back up an existing
installation before replacing it. Install the skill subdirectory, not this
repository root.

## Quick use

1. Copy [glass.css](skills/liquid-glass-web/assets/glass.css) into a style block.
2. Use `.lg` for floating surfaces, `.lg--lite` for bottom-layer content,
   and `data-lg-source` for Safari background regions.
3. Copy [refraction-snippet.html](skills/liquid-glass-web/assets/refraction-snippet.html)
   once near the end of the body, before application scripts.
4. Put page-specific CSS after the canonical stylesheet.

```html
<article class="lg lg--lite">
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
| `.lg--lite`, `.lg--overlay` | Lightweight content / fixed-90%-transparent selection |
| `data-lg-source` | Declared Safari background source selector |
| `data-lg-map-scale="2"` | 2× maps for large content-overlapping surfaces |
| `data-lg-nav` | Optional shared draggable navigation; see component reference |
| `--lg-border-width` | 0.5px frame and concentric inner-radius inset |
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

Candidate policy (2026-10-08): verified desktop Chromium uses native SVG backdrop
filters; Safari uses ordinary SVG filters on bounded, inert copies of declared
HTML sources. Other environments and Safari surfaces without a source retain
the clear tinted material. The menu uses native popovers. Source limitations,
events, and verification scope are centralized in the
[component reference](skills/liquid-glass-web/references/components.md#browser-paths-and-source-scope).

The ladder is **edge refraction → clear tinted surface → solid accessibility
surface**. Reduced transparency, increased contrast, forced colors, and print
override the material. Media-query support varies by browser.

## Accessibility and performance

Use lightweight material for large reading containers and one shared overlay
for navigation. Keep the number of full optical panes small. Preserve visible
focus, accessible names, and contrast over busy backgrounds. The menu supports
Escape and outside-click dismissal; the slider retains native keyboard access.

Maps update on pane resize, not scroll or cursor movement. Each pane has a
distinct filter; closed dialogs initialize when visible. The optical band is
capped at 22% of the shorter dimension so opposite edges stay separated.
Reduced motion stops decorative motion. Print flattens surfaces.
2× maps use four times the map pixels; they leave rendered pane dimensions
unchanged. Safari sources refresh for content/size/theme changes and synchronize
position during scroll and drag.

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
