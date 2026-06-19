# InteriorAgent-IDSDL — Documentation

This repository contains the documentation site for **IDSDL**, an Interior-Design-aware Scene
Description Language for composing structured 3D indoor scenes.

🌐 **Live site:** <https://interioragent.github.io/docs/>
💻 **Source code:** <https://github.com/KunalMGupta/interioragent>

The site is built with [Jekyll](https://jekyllrb.com) and the
[Just the Docs](https://just-the-docs.github.io/just-the-docs/) theme, and is published to
GitHub Pages.

## What's here

| Page | Contents |
|---|---|
| **Getting Started** | A complete end-to-end example and the coordinate system. |
| **Object Registration** | Retrieving, scaling, rotating, copying, and querying assets; lighting. |
| **Groups** | `RelativeGroup`, `AroundGroup`, `GridGroup`, `RoomGroup`, hierarchical layout, and `SentenceASCIIGenerator`. |
| **Constraints** | Gradient constraints (overlap, bounds, clearance, access, visibility) and VLM constraints (proportions, room size, wall overlap). |

Every feature is illustrated with top-down (floor-plan) and perspective renders under
`assets/scenes/`.

## Editing the docs

Each page is a Markdown file with Just-the-Docs front matter that controls its place in the
navigation:

```yaml
---
title: RelativeGroup
layout: home
parent: Groups        # nests this page under the "Groups" section
nav_order: 1          # ordering within its level
---
```

To add a page, create `your-page.md` with appropriate front matter; the navigation updates
automatically.

## Building locally

Requires Ruby ≥ 3 with [Bundler](https://bundler.io) and [Jekyll](https://jekyllrb.com):

```bash
bundle install
bundle exec jekyll serve   # preview at http://localhost:4000
```

The built site is written to `_site/` (git-ignored).

### Quick preview without Ruby

If you only need to eyeball the content, the main code repo ships `build_preview.py`, which
renders all pages into a single standalone `preview.html` using Python — no Ruby toolchain
required:

```bash
# from the interioragent code repo, with docs/ alongside
python build_preview.py
(cd docs && python -m http.server 8000)   # open http://localhost:8000/preview.html
```

## Figures

The rendered figures in `assets/scenes/` are generated from dedicated minimal example scenes
by `docs_figures.py` in the [code repository](https://github.com/KunalMGupta/interioragent),
which builds each scene with IDSDL and renders it top-down and in perspective. Only the
resulting PNGs are committed here; the source `.blend` files are not.

## License

Documentation content © Kunal Gupta. The Just-the-Docs theme and the GitHub Pages deployment
workflow are MIT-licensed (see `LICENSE`).
