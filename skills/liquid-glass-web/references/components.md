# Liquid Glass · component reference

Copy `assets/glass.css` and `assets/refraction-snippet.html` once into the page.
Place page-specific CSS after the stylesheet and application scripts after the snippet.

## Materials and sampling

| Role | Markup | Sampling |
|---|---|---|
| Floating navigation, compact controls | `.lg`, optionally `.lg--thin` | 1× |
| Large dialog covering text or graphics | `.lg.lg--thick` with `data-lg-map-scale="2"` | 2× |
| Bottom-layer reading container | `.lg.lg--lite` | No optical maps |
| Shared selection | `.lg.lg--overlay` | 1×, fixed 90% transparency |

Choose sampling by the surface's role. 2× doubles both map dimensions and uses
four times the map pixels, preserving optical distances in CSS pixels. Three
maps describe displacement, dispersion, and edge fading. Reuse maps while moving;
geometry changes rebuild them. Closed dialogs initialize when visible.

All materials retain tint, border, highlights, shadow, and pointer lighting.
The optical band is `min(39.2px, 22% of the shorter dimension)`; corner normals
follow circular radii. Local blur and dispersion scale with the band, while
the center stays clear. `--lg-blur` remains a compatibility token at 0px.

```html
<article class="lg lg--lite">Reading content</article>
<dialog class="lg lg--thick" data-lg-source="#scene" data-lg-map-scale="2">
  <h2>Details</h2>
  <form method="dialog"><button class="lg-chip">Close</button></form>
</dialog>
```

## Browser paths and source scope

Version 0.1.2 uses native SVG backdrop filters on verified desktop
Chromium environments. Safari uses a bounded visual source copy and ordinary
SVG filters. Both paths share optical formulas and map density. Other environments
retain the clear tinted material. Engine identification and syntax checks live
in one entry point; iOS product names do not imply desktop Chromium capabilities.

`data-lg-source="#scene"` names the HTML region behind a pane. Use a source
outside the pane with self-contained styles and the text or graphics that should
appear through the glass. Transparent wrappers retain their page appearance.
Inside the clipped optical input, Safari paints the static `.lg-backdrop` CSS
background at its page coordinates, then the source copy. Without that element,
the input uses the body's background color. Chromium creates no copies.
Safari panes without a declared source retain the base material.

Copies are inert and `aria-hidden`; original content retains interaction and
semantics. Content, source size, images, and theme changes refresh the copy.
Scrolling and `lg:move` synchronize position. After application-driven movement,
dispatch `document.dispatchEvent(new Event('lg:move'))`. Dispatch `lg:refresh`
on the source after style changes outside its subtree, and `lg:theme` on document
after custom theme changes. The bundled appearance panel sends the theme event.

### Scope and verification

The bridge supports declared static HTML regions. Live video, canvas, embedded
documents, animated sources, and recursive glass compositions need separate
rendering work. Keep source regions small and outside other full-material panes.
A copy represents its declared source rather than the whole composited page.
The paths target similar perceptible optics; font rasterization and sampling
can differ. 2× maps offer limited improvement rather than eliminating all aliasing.

Technical-study checks: Safari 27.0.1 retained refraction and alignment at 100%
and 200% zoom after relative filter-bounds correction. Desktop Chrome passed
the user's visual check. These observations precede final four-example acceptance.

## Shared navigation

Use one selection above the link icons and labels. Its source wrapper and selection
are siblings inside the nav. Keep the source wrapper transparent.

```html
<nav class="lg lg--thin" data-lg-nav data-lg-source="#page-content"
     aria-label="Sections" style="--lg-radius:999px;position:sticky;top:12px">
  <div class="lg-nav-items" id="section-links">
    <a href="#intro">Intro</a>
    <a href="#work">Work</a>
  </div>
  <div class="lg lg--overlay lg-selection"
       data-lg-source="#section-links" aria-hidden="true"></div>
</nav>
```

The snippet initializes `[data-lg-nav]` at load. Links point to existing sections.
Pointer capture maintains direct dragging; release selects the nearest item and
scrolls to its section. A nonzero `scroll-margin-top` on the target sets its
landing offset. Otherwise horizontal navigation reserves its height plus 24px,
and vertical navigation reserves 24px. Clicks, drag releases, and natural
scrolling use the same section positions.
Wheel, touch, navigation keys, and Escape interrupt programmatic scrolling.
Escape cancels a held drag; keyboard users operate the original links.
Reduced motion positions immediately. Orientation follows measured link positions;
the gallery provides a horizontal/vertical switch.

## Appearance and tokens

An optional `#themeToggle` button creates a native popover with Light/Dark buttons
and a labeled 5–95% transparency slider, initially 40%. Visual content is limited
to labels and controls. The slider changes tint alpha; foreground opacity and
optics stay unchanged. Overlay transparency stays at 90%. Values persist through
theme changes in the page session. Escape returns focus to the trigger; outside
clicks dismiss the panel. System accessibility preferences take priority.

| Token | Default / purpose |
|---|---|
| `--lg-border-width` | 0.5px; also drives inner corner radius |
| `--lg-border` | Light alpha 0.65; dark alpha 0.10 |
| `--lg-spec` | Top highlight: light alpha 0.95; dark alpha 0.20 |
| `--lg-tint`, `--lg-tint-a` | Theme channels and opacity; initial alpha 0.60 |
| `--lg-sat`, `--lg-bright` | Both 1 on the native path |
| `--lg-radius` | 26px; 999px for capsules |
| `--lg-shadow`, `--lg-shadow-sm` | Full and thin shadows |
| `--sheen-a`, `--spot-max` | Sheen and pointer-light intensity |
| `--bg`, `--ink`, `--ink-dim`, `--hairline`, `--accent`, `--accent-ink` | Theme colors |

Bottom and inner-ring highlights retain their existing values. Filter IDs,
`--lg-filter`, `--mx`, `--my`, and `--spot-a` belong to runtime state.

## Controls and examples

`.lg-chip` is a regular control; `.lg-cta` adds an accent fill.
`.lg-materialize` supplies an entrance; `.lg-backdrop` is optional artwork.
The examples use static background sources for consistent copy rendering.

- `example-quick-start.html`: lightweight reading card and full navigation.
- `example-components.html`: materials, status chip, shared selection, 2× dialog.
- `example-music-player.html`: separate player logic and appearance controls.
- `example-resume.html`: lightweight content, full navigation, print styles.

## Checks and troubleshooting

Check source alignment during scrolling, dragging, resizing, and dialog opening.
Check themes, slider endpoints, overlay alpha, focus, Escape, 200% zoom, system
accessibility preferences, and print. Verify inert copies, unique IDs, offline
loading, and exact canonical embedding.

For missing optics, inspect the runtime path, source, dimensions, filter ID, and
backdrop-root ancestors. Use concentric circular radii for corner alignment.
For heavy scrolling, simplify sources and use lightweight bottom-layer containers.
Custom theme controls should dispatch the documented refresh event.
