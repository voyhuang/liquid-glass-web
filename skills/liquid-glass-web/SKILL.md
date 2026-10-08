---
name: liquid-glass-web
description: Build Liquid Glass web interfaces with edge refraction, clear centers, lightweight content surfaces, and draggable glass navigation. Use for translucent navigation, cards, dialogs, media controls, and glass rendering issues.
---

# Liquid Glass Web · v0.1.2

## Workflow

1. Preserve the page's content, layout, semantics, and application behavior.
2. Choose a bundled example and copy `assets/glass.css` and `assets/refraction-snippet.html` verbatim. Place page-specific styles and behavior after the assets.
3. Choose materials by role. Declare Safari background sources with `data-lg-source`; use `references/components.md` for rendering paths, source scope, appearance controls, and shared navigation.
4. Customize the documented tokens and verify the completed page in its target browsers.

## Materials

- Use `.lg` for floating surfaces with edge refraction, folded reflections, local dispersion, and clear centers.
- Use `data-lg-map-scale="2"` on large full-material surfaces that may cover text or graphics, such as large dialogs. Compact surfaces use the default 1× maps.
- Use `.lg--lite` for large bottom-layer content containers, retaining tint, rim lighting, shadow, and pointer lighting.
- Use one `.lg--overlay` for a shared navigation selection above button icons and labels. Keep its transparency at 90%.
- Match the frame and optical map with concentric circular corners. Reuse maps while scrolling and dragging; rebuild for geometry changes.

## Core parameters

| Parameter | Default |
|---|---|
| Appearance transparency | 5–95%; initially 40% |
| Overlay transparency | 90% |
| `--lg-border-width` | 0.5px |
| Dark border / top highlight alpha | 0.10 / 0.20 |
| `--lg-sat` / `--lg-bright` | 1 / 1 |
| `--lg-tint` / `--lg-tint-a` | Tint channels / opacity |

The compact appearance panel contains Light/Dark buttons and a labeled slider. Transparency changes the tint layer while preserving foreground content and optics. System accessibility preferences take priority.

## Examples

- `references/example-quick-start.html`: minimal complete page.
- `references/example-components.html`: materials, dialog, and draggable selection.
- `references/example-music-player.html`: media controls and appearance settings.
- `references/example-resume.html`: lightweight content and full-material navigation.

Each example runs offline and embeds the canonical assets. Component interfaces and browser behavior are documented in `references/components.md`.

## Acceptance

Verify target-browser optics, clear centers, corner alignment, appropriate map density, and stable scrolling. Check appearance defaults, fixed overlay transparency, pointer lighting, selection dragging, keyboard focus, Escape, 200% zoom, accessibility preferences, and print. Confirm canonical embedding and offline operation.
