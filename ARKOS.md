# gpui, as Arkos IDE uses it

This repository holds the sources of the `gpui` crate as published on crates.io, with Arkos
IDE's patches as commits on top. It is not a fork of the Zed monorepo: the crate's published
sources are 8 MB, the monorepo a gigabyte that every clone and CI run would pay for.

- The first commit on each `upstream` line is the crates.io tarball of that version, untouched
  (`.cargo_vcs_info.json` records the Zed commit it was cut from). Tag `v<version>`.
- Arkos's patches follow, each a commit that adds a hook and no feature. Tag
  `v<version>-arkos.<n>` for each state Arkos depends on; Arkos pins the tag in its
  `[patch.crates-io]` section.
- To move to a new gpui: commit the next tarball as `v<version>`, rebase the patch commits onto
  it, tag `v<version>-arkos.1`, and point Arkos at the tag.

Patches so far: `Window::set_glyph_shader` (tag `v0.2.2-arkos.1`), a per-window hook that
replaces the glyph (monochrome sprite) stages with functions from a Metal source the app
supplies, with `GlyphShader`, `GLYPH_SHADER_PRELUDE` and `GLYPH_SHADER_BASE` (Arkos's effects
design, 2026-10-28); `Window::set_glyph_shader_time` (tag `v0.2.2-arkos.2`), a clock for the
glyph stages: seconds the app sets, bound as one `float` at Metal buffer index 5
(`SpriteInputIndex::GlyphTime`) to both glyph stages on every glyph draw, whether or not an app
shader is set; gpui's own stages ignore it (Arkos's space effect, 2026-10-28).
