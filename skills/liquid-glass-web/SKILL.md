---
name: liquid-glass-web
description: Build Liquid Glass web interfaces with size-aware edge refraction, soft folded reflections, local dispersion, clear centers, specular lighting, and accessible fallbacks. Use for liquid glass, glassmorphism, translucent navigation, cards, dialogs, media controls, and debugging glass edges or browser fallbacks.
---

# Liquid Glass Web · v0.1.1

Use the bundled CSS and SVG/JavaScript together. The approved material has a
clear center: no whole-pane Gaussian blur. Chromium receives edge refraction;
other browsers retain translucent tint, rim lighting, and color adjustment.
This is an independent visual approximation, not Apple's shader.

## Fidelity and workflow

1. Inspect the existing layout and preserve content, semantics, and application logic.
2. Copy `assets/glass.css` verbatim; place page-specific CSS after it.
3. Apply `.lg` only to floating controls or compact surfaces. Use `.lg-chip`
   inside panes; do not stack glass on glass. Keep at most five panes per view.
4. Copy `assets/refraction-snippet.html` verbatim near the end of `body`,
   before application-specific scripts. Use it once per document.
5. Customize documented tokens, not the optical curves. Each existing pane gets
   a unique filter and a size-dependent map. ResizeObserver refreshes maps when
   dimensions change, including when a closed dialog opens; scrolling does not
   rebuild maps. Panes added later need initialization by the host application;
   this snippet intentionally has no MutationObserver or public module API.
6. Test the relevant examples, theme/menu controls, responsive sizes, and
   accessibility settings after changes.

## References

| Need | Reference |
|---|---|
| Small complete page | `references/example-quick-start.html` |
| Nav, chips, card, standalone button, dialog | `references/example-components.html` |
| Interactive player | `references/example-music-player.html` |
| Portfolio, sticky navigation, print | `references/example-resume.html` |
| Component markup | `references/components.md` |

All four HTML examples work offline and embed both canonical assets exactly.
Use the closest example, keeping its embedded assets unchanged.

## Material behavior

- Shape: circular rounded rectangles and pills. Frame and pseudo-elements must
  share concentric radii. Do not add `corner-shape: squircle`: the optical map
  models circular corners, not superellipses.
- Refraction acts normal to every edge; normals turn continuously around corners.
  The main band is `min(39.2px, 22% of the shorter pane dimension)`.
- Five-degree falloff plus a near-edge Gaussian shoulder produces stretching,
  compression, and a folded image. Do not replace this with turbulence.
- The reflection axis uses symmetric weak ghost samples and local blur, not a
  broad low-contrast strip.
- Color dispersion and near-disappearance occupy the last 10px on large panes.
  All bands, displacement amplitudes, and blur radii scale together on small
  panes. Opposing bands do not meet in the center.
- There is no global backdrop blur. Keep the independent background decoration
  blur and temporary entrance animation distinct from material blur.

## Public classes and tokens

| Interface | Meaning |
|---|---|
| `.lg` | Base pane with tint, refraction, and specular layers |
| `.lg--thick` / `.lg--thin` | Surface hierarchy/shadow; neither adds global blur |
| `.lg-materialize` | Entrance with `--enter-delay`; no persistent final filter |
| `.lg-chip` / `.lg-cta` | Non-glass controls / accent variant |
| `.lg-backdrop` | Optional decorative animated color field |
| `--lg-radius` | 26px default; 999px for pills |
| `--lg-tint` / `--lg-tint-a` | Theme color channels / opacity |
| `--lg-sat` / `--lg-bright` | Color adjustment, defaults 1.8 / 1.08 |
| `--lg-shadow` / `--lg-shadow-sm` | Large / small surface shadows |
| `--sheen-a` / `--spot-max` | Sheen / cursor spotlight |
| `--bg`, `--ink`, `--ink-dim`, `--hairline` | Theme content colors |
| `--accent` / `--accent-ink` | Focus and CTA colors |
| `--lg-blur` | Legacy token retained at 0px; no longer controls material blur |

`--lg-filter`, `--mx`, `--my`, and `--spot-a` are runtime state.
Preserve all other existing color and decoration tokens in the stylesheet.

## Optional appearance menu

Add one button with `id="themeToggle"`, `type="button"`, and an accessible
appearance label. The snippet creates an opaque, top-layer popover: clicking
opens the menu without changing theme. It contains Light/Dark controls and a
40–80% glass-transparency slider, with no displayed number or helper sentence.

The slider changes only tint alpha, not foreground opacity or optical effects.
A custom value survives theme changes within the page, but is not persisted
across reloads. Reduced transparency, contrast, forced colors, and print rules
override tint adjustments. Escape closes the menu and restores trigger focus;
outside clicks dismiss it. Omit the trigger to omit the menu.

## Browser and accessibility boundaries

The v0.1.1 policy retains the conservative Chromium gate
(`CSS.supports` plus `navigator.userAgentData`). It is a rendering policy,
not a claim that other engines can never gain support. Safari/Firefox use the
clear tinted fallback; no JavaScript also retains the static material.
Recheck browser support before claiming current engine capabilities.

Native popovers are required for the optional appearance menu. Do not restore
the v0.1.0 frost fallback or a refraction-status badge. System preference support
varies; retain all existing media-query overrides and visible keyboard focus.

## Troubleshooting

- Missing refraction: inspect the browser gate, generated per-pane filter IDs,
  hidden/zero-size panes, and ancestor backdrop roots.
- Effect vanishes after entrance: ensure the animation's final filter is removed.
- Corner mismatch: preserve concentric circular radii; do not mix squircle frames
  with circular optical maps.
- Muddy center: remove added whole-pane blur; check tint and background content.
- Thin panels look overprocessed: ensure every local blur uses the same optical
  size ratio as its displacement map, rather than a fixed large-pixel radius.
- Slow scroll: reduce pane count; avoid rebuilding maps on pointer or scroll events.

## Validation

Check exact asset embedding, offline operation, distinct per-pane filters,
dialog opening, clear centers, edge behavior on small/large panes, menu keyboard
operation, both themes, reduced motion/transparency, contrast, forced colors,
and print. Browser rendering checks do not prove Apple-equivalent optics.
