# CondoSense Landing Page

Static marketing landing page for CondoSense.

## Production Surface

These are the only files required to deploy/serve the live site:

- `index.html` — the deployable landing page.
- `disclosure_shield_icon.svg`
- `sirs_column_icon.svg`
- `overhead_folder_icon.svg`
- `fiduciary_scale_icon.svg`

No build step is required; `index.html` loads Tailwind and fonts from
external CDNs at runtime.

## Archival / Non-Production Assets

The following are raw design-tool exports kept for historical/design
reference only. They are **not** used by, linked from, or required by
`index.html`, and must not be deployed as part of the live site:

- `Stitch - Projects.html`
- `Stitch - Projects_files/` (supporting CSS/JS/font bundles for the
 file above)
