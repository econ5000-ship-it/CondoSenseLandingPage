# CondoSense Landing Page — Enhancement Plan

This repo is a static marketing landing page (`index.html`) plus archival
design-export artifacts (`Stitch - Projects.html`, `Stitch - Projects_files/`).
There is no build step, framework, or backend — every task below is a direct,
self-contained edit to plain HTML/CSS/JS.

Each task is strictly independent and atomic: it can be run in a brand-new,
empty AI chat session with zero prior context, touches only its own file(s),
and stays under 100 lines of diff. The junior/local model executing a task
should **not** get hung up on whitespace, indentation, or attribute-ordering
style — `ruff` is available in this environment and will auto-fix those
mechanically at the end of every task. Do not hand-fix formatting; let the
linter do it, including unsafe fixes.

---

### Task 1: Add Security Meta Headers to `index.html`

```markdown
Objective:
Add baseline browser security policy tags (CSP, Referrer-Policy,
Permissions-Policy) to the `<head>` of `index.html` without altering any
visible layout, copy, or existing functionality.

Target File:
- `index.html`

Context & Current Behavior:
The `<head>` block (lines 3–124) currently contains only a charset meta tag
(line 3), a viewport meta tag (line 5), the page `<title>` (line 6), two
Google Fonts `<link>` tags (lines 8 and 10), the Tailwind CDN `<script>`
(line 11), an inline Tailwind config `<script id="tailwind-config">`
(lines 12–102), and an inline `<style>` block (lines 103–123). There is no
Content-Security-Policy, Referrer-Policy, or Permissions-Policy meta tag
anywhere in the file. The page loads three external origins today:
`fonts.googleapis.com`, `fonts.gstatic.com` (implied by the Google Fonts
`@import`/`css2` links), `cdn.tailwindcss.com`, and images from
`lh3.googleusercontent.com` (see the `<img>` at line 156). Any CSP added
must not break these existing external loads.

Requirements & Constraints:
1. Immediately after the existing viewport meta tag (line 5) and before the
   `<title>` tag (line 6), insert three new meta tags:

       <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self' 'unsafe-inline' https://cdn.tailwindcss.com; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src https://fonts.gstatic.com; img-src 'self' https://lh3.googleusercontent.com data:; connect-src 'self';">
       <meta name="referrer" content="strict-origin-when-cross-origin">
       <meta http-equiv="Permissions-Policy" content="camera=(), microphone=(), geolocation=()">

2. Do not modify, remove, or reorder any existing tag, script, or style block.
3. Do not add any new external domains beyond the four already in use
   (`fonts.googleapis.com`, `fonts.gstatic.com`, `cdn.tailwindcss.com`,
   `lh3.googleusercontent.com`).
4. Do not touch `<body>` (line 125 onward) or the inline `<script>` block at
   the bottom of the file (lines 270–316).
5. Keep the total diff under 100 lines (this task is ~4 lines of insertion).
6. Do not modify any other file in the repository.

After editing, run `ruff check --fix` (ignore unsafe fixes) to auto-fix any
whitespace/indent/formatting issues — let ruff handle formatting, don't fix
it by hand. (If ruff reports no Python files to lint, that is expected and
fine; this task's correctness is judged by the HTML diff, not ruff output.)
```

---

### Task 2: Defer Non-Critical Script Loading and Lazy-Load Below-the-Fold Images

```markdown
Objective:
Improve first-paint performance in `index.html` by deferring the Tailwind
CDN script and lazy-loading images that are not in the initial viewport,
without changing any visual output.

Target File:
- `index.html`

Context & Current Behavior:
Line 11 currently loads Tailwind synchronously and render-blocking:

    <script src="https://cdn.tailwindcss.com/?plugins=forms,container-queries"></script>

There are four `<img>` tags total in the file:
- Line 156: the hero report screenshot (`src="https://lh3.googleusercontent.com/..."`) —
  this is above the fold and must NOT be lazy-loaded (it should stay eager so
  it doesn't visually pop in on load).
- Line 171: `disclosure_shield_icon.svg` inside the "Systemic Compliance
  Intelligence" grid (`id="compliance"` section, starts line 161) — below the
  fold on initial load.
- Line 179: `sirs_column_icon.svg` — same grid, below the fold.
- Line 187: `overhead_folder_icon.svg` — same grid, below the fold.
- Line 195: `fiduciary_scale_icon.svg` — same grid, below the fold.

None of these five `<img>` tags currently have `loading` or `decoding`
attributes.

Requirements & Constraints:
1. On line 11, add `defer` to the Tailwind script tag so it becomes:

       <script defer src="https://cdn.tailwindcss.com/?plugins=forms,container-queries"></script>

   Note: because the inline `tailwind-config` script (line 12) and the
   `tailwind.config` object it sets rely on the `tailwind` global existing by
   the time Tailwind CDN parses config, do NOT add `defer` to the
   `tailwind-config` script block itself (lines 12–102) — leave it exactly as
   is, since Tailwind CDN's own script self-executes on load and reads
   `window.tailwind.config` synchronously after this point; only defer the
   CDN loader tag on line 11.
2. Add `loading="lazy" decoding="async"` to the four icon `<img>` tags at
   lines 171, 179, 187, and 195 only.
3. Do NOT add `loading="lazy"` to the hero image at line 156 — it must remain
   eager-loaded since it is above the fold.
4. Do not change any `src`, `alt`, `class`, or `id` attribute values on any
   image or script tag.
5. Do not modify anything outside the five tags listed above (script at
   line 11; images at lines 171, 179, 187, 195).
6. Keep the total diff under 100 lines (this task is ~5 lines changed).
7. Do not modify any other file in the repository.

After editing, run `ruff check --fix` (ignore unsafe fixes) to auto-fix any
whitespace/indent/formatting issues — let ruff handle formatting, don't fix
it by hand.
```

---

### Task 3: Add Accessible Landmark Roles and ARIA Labels to Icon-Only Controls

```markdown
Objective:
Improve screen-reader accessibility in `index.html` by adding explicit ARIA
labeling to icon-only interactive controls and confirming/adjusting landmark
roles, without changing any visible text, styling, or layout.

Target File:
- `index.html`

Context & Current Behavior:
1. The `<header>` element (line 127) wraps a `<nav>` (line 128) — this is
   already correctly landmarked, no change needed there.
2. The `<main class="pt-20">` wrapper (line 138) through its closing tag
   (line 240) already wraps all primary page sections — no change needed.
3. The close button for the About popup, at line 261, is icon-only (uses the
   literal `&times;` glyph as its only content) and has no accessible name:

       <button id="close-popup" class="text-primary text-4xl leading-none hover:opacity-70 transition-opacity">&times;</button>

   Screen readers will announce this button with no meaningful label.
4. The submit button at lines 228–231 already has visible text
   ("Submit Diagnostic Request") plus a decorative Material Symbols icon span
   (line 230, `arrow_forward`) — this decorative icon should be hidden from
   assistive tech since the button already has an accessible name from its
   text content.
5. The footer (line 242) is currently `class="hidden ..."` — it is
   intentionally hidden today; do not change its visibility or `hidden`
   class, only fix its internal semantics if touched (in this task, it is
   NOT touched — leave lines 242–255 untouched).

Requirements & Constraints:
1. At line 261, add `aria-label="Close"` and `type="button"` to the
   close-popup button so it becomes:

       <button id="close-popup" type="button" aria-label="Close" class="text-primary text-4xl leading-none hover:opacity-70 transition-opacity">&times;</button>

2. At line 230, add `aria-hidden="true"` to the decorative arrow icon span so
   it becomes:

       <span class="material-symbols-outlined" aria-hidden="true" style="font-size: 18px;">arrow_forward</span>

3. At line 257, the About popup container currently has no `role`; add
   `role="dialog" aria-modal="true" aria-labelledby="about-popup-title"` to
   the outer `<div id="about-popup" ...>` tag, and add
   `id="about-popup-title"` to the `<h2>` at line 260 (the "About Our
   Engineering Framework" heading) so the dialog is properly labeled:

       <div id="about-popup" class="fixed inset-0 bg-black bg-opacity-50 z-50 hidden items-center justify-center" role="dialog" aria-modal="true" aria-labelledby="about-popup-title">
       ...
       <h2 id="about-popup-title" class="font-headline-md text-headline-md text-primary">About Our Engineering Framework</h2>

4. Do not change any visible text, class list ordering (beyond appending new
   attributes), or JavaScript logic (lines 270–316 must remain untouched).
5. Do not modify the footer (lines 242–255) in this task.
6. Keep the total diff under 100 lines (this task is ~4 lines changed).
7. Do not modify any other file in the repository.

After editing, run `ruff check --fix` (ignore unsafe fixes) to auto-fix any
whitespace/indent/formatting issues — let ruff handle formatting, don't fix
it by hand.
```

---

### Task 4: Add Open Graph and Twitter Card Metadata

```markdown
Objective:
Add complete Open Graph and Twitter Card meta tags to the `<head>` of
`index.html` so links to this page render rich social previews, using only
facts already present on the page (no invented marketing claims).

Target File:
- `index.html`

Context & Current Behavior:
The `<head>` (lines 3–124) currently has a `<title>` at line 6
("CondoSense | Institutional Compliance Auditing") and no `<meta
name="description">` or any `og:*`/`twitter:*` tags at all. The only
page copy available to source metadata from includes:
- H1 (line 142–144): "Automated HB 1021 Compliance Discrepancy Auditing for
  Florida Condominiums."
- Hero paragraph (line 145–147): "We engineer programmatic data auditing
  tools that parse public corporate filings and county registries to
  identify statutory compliance risks before enforcement action lands."
- Hero image (line 156): `src="https://lh3.googleusercontent.com/aida/ADBb0uiGDhKn0_W9MYB62-TWN3YGOAS5SZ5DEfUS8f4c5jAiqH4l9XEq9Jr61OkQECTWFbRgzxTznPnhGcHb4h46figumFYmsbSpVPslVSz36xnnfmFtFoReK6ZLbgYT3Vj-dMV9ESGrgDOnsUStyfyB6CB5c4wLFvwELDAO8-YHnbmLuICWVWCAyIf5eyH0GjOcWolaEm27lAcLyPy0juGtMHjKmH17-pgw9e3gbORtjKVRz4md4Zw0eM2iTOo"`,
  `alt="CondoSense Compliance Report"`.

There is no known canonical production URL in the repo; use the placeholder
`https://econ5000-ship-it.github.io/CondoSenseLandingPage/` for `og:url` (do not fabricate any other
domain).

Requirements & Constraints:
1. Immediately after the `<title>` tag (line 6) and before the
   `<!-- Google Fonts -->` comment (line 7), insert:

       <meta name="description" content="Automated HB 1021 compliance discrepancy auditing for Florida condominiums. We parse public corporate filings and county registries to identify statutory compliance risks before enforcement action lands.">
       <meta property="og:title" content="CondoSense | Institutional Compliance Auditing">
       <meta property="og:description" content="Automated HB 1021 compliance discrepancy auditing for Florida condominiums. We parse public corporate filings and county registries to identify statutory compliance risks before enforcement action lands.">
       <meta property="og:type" content="website">
       <meta property="og:url" content="https://econ5000-ship-it.github.io/CondoSenseLandingPage/">
       <meta property="og:image" content="https://lh3.googleusercontent.com/aida/ADBb0uiGDhKn0_W9MYB62-TWN3YGOAS5SZ5DEfUS8f4c5jAiqH4l9XEq9Jr61OkQECTWFbRgzxTznPnhGcHb4h46figumFYmsbSpVPslVSz36xnnfmFtFoReK6ZLbgYT3Vj-dMV9ESGrgDOnsUStyfyB6CB5c4wLFvwELDAO8-YHnbmLuICWVWCAyIf5eyH0GjOcWolaEm27lAcLyPy0juGtMHjKmH17-pgw9e3gbORtjKVRz4md4Zw0eM2iTOo">
       <meta name="twitter:card" content="summary_large_image">
       <meta name="twitter:title" content="CondoSense | Institutional Compliance Auditing">
       <meta name="twitter:description" content="Automated HB 1021 compliance discrepancy auditing for Florida condominiums. We parse public corporate filings and county registries to identify statutory compliance risks before enforcement action lands.">
       <meta name="twitter:image" content="https://lh3.googleusercontent.com/aida/ADBb0uiGDhKn0_W9MYB62-TWN3YGOAS5SZ5DEfUS8f4c5jAiqH4l9XEq9Jr61OkQECTWFbRgzxTznPnhGcHb4h46figumFYmsbSpVPslVSz36xnnfmFtFoReK6ZLbgYT3Vj-dMV9ESGrgDOnsUStyfyB6CB5c4wLFvwELDAO8-YHnbmLuICWVWCAyIf5eyH0GjOcWolaEm27lAcLyPy0juGtMHjKmH17-pgw9e3gbORtjKVRz4md4Zw0eM2iTOo">

2. If Task 1 (security meta headers) has already been applied to this file
   when you run this task, insert the block above it in the same location
   relative to `<title>`/`<!-- Google Fonts -->`; do not remove or duplicate
   any CSP/Referrer-Policy/Permissions-Policy tags that may already exist.
3. Do not alter the `<title>` tag itself, any font `<link>` tags, or any
   script/style block.
4. Keep the total diff under 100 lines (this task is ~11 lines of insertion).
5. Do not modify any other file in the repository.

After editing, run `ruff check --fix` (ignore unsafe fixes) to auto-fix any
whitespace/indent/formatting issues — let ruff handle formatting, don't fix
it by hand.
```

---

### Task 5: Document Production Surface vs. Archival Design-Export Assets

```markdown
Objective:
Create a `README.md` at the repository root that clearly documents which
files are the deployable production surface versus which are archival
design-export artifacts, so future contributors don't accidentally treat
export files as runtime dependencies.

Target File:
- `README.md` (create new file; it does not currently exist)

Context & Current Behavior:
The repository root (`/`) currently contains:
- `index.html` — the actual production landing page.
- `disclosure_shield_icon.svg`, `sirs_column_icon.svg`,
  `overhead_folder_icon.svg`, `fiduciary_scale_icon.svg` — four icon assets
  referenced directly by `index.html` (see lines 171, 179, 187, 195 of
  `index.html`).
- `Stitch - Projects.html` — a raw HTML export from a design tool (Stitch),
  not linked from or used by `index.html`.
- `Stitch - Projects_files/` — a directory of ~27 supporting export assets
  (CSS bundles like `Create-V25hCgkH.css`, JS bundles like
  `index-D7vPDn1n.js`, cached font responses like `css2`, `css2(1)` etc.,
  and a screenshot `unnamed.png`) that only support
  `Stitch - Projects.html`, not `index.html`.

There is currently no README anywhere in the repo explaining this split, so a
contributor could mistakenly assume `Stitch - Projects.html` or its
supporting files are part of the deployed site.

Requirements & Constraints:
1. Create `README.md` at the repository root with the following exact
   structure and content (fill in the file/directory names verbatim as
   listed above):

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

2. Do not create, delete, move, or rename any other file in this task —
   this is a documentation-only change.
3. Do not modify `index.html` or any `.svg` file.
4. Keep the new file under 100 lines total.

After creating the file, run `ruff check --fix` (ignore unsafe fixes) to
auto-fix any whitespace/indent/formatting issues in any Python files that may
exist in the repo — let ruff handle formatting, don't fix it by hand. (This
task only touches Markdown, so ruff is expected to report nothing to fix;
that is fine.)
```

---

### Task 6: Add Privacy-Safe CTA Click Instrumentation

```markdown
Objective:
Add lightweight, dependency-free CTA click tracking to `index.html` that
pushes an event to `window.dataLayer` (if present) with zero network calls,
zero cookies, and zero third-party SDKs.

Target File:
- `index.html`

Context & Current Behavior:
There are three primary calls-to-action in the page today, none of which
currently have any tracking attribute or click instrumentation:
1. Line 135: the header "Request Audit" link —
   `<a class="border border-[#0F172A] text-[#0F172A] px-6 py-2.5 font-label-md text-label-md hover:bg-[#0F172A] hover:text-white transition-all flat-design" href="#intake">Request Audit</a>`
2. Lines 149–151: the hero "Request an Association Diagnostic" link.
3. Lines 228–231: the intake form's "Submit Diagnostic Request" button.

The closing `<script>` block (lines 270–316) already contains unrelated
micro-interaction logic (form focus handling at lines 272–282, form
prevent-default handling at lines 285–288, and About-popup show/hide logic
at lines 291–315). This task must add new code to that same script block
without disturbing the existing logic.

Requirements & Constraints:
1. Add a `data-cta` attribute to each of the three CTA elements listed above,
   using these exact values:
   - Line 135 link: add `data-cta="nav-request-audit"`.
   - Line 149 link: add `data-cta="hero-request-diagnostic"`.
   - Line 228 button: add `data-cta="form-submit-diagnostic"`.
2. At the end of the existing `<script>` block (immediately before the
   closing `</script>` tag on line 316, after the existing
   `aboutPopup.addEventListener(...)` block that ends at line 315), add:

       // CTA click instrumentation (privacy-safe, no network calls)
       document.querySelectorAll('[data-cta]').forEach((el) => {
           el.addEventListener('click', () => {
               if (Array.isArray(window.dataLayer)) {
                   window.dataLayer.push({ event: 'cta_click', cta_id: el.dataset.cta });
               }
           });
       });

3. Do not send any data over the network, do not set any cookies, and do not
   load any external script or library.
4. Do not modify the existing form-focus, form-submit, or About-popup logic
   already present in the script block (lines 272–315) — only append the new
   snippet after it.
5. Keep the total diff under 100 lines (this task is ~10 lines changed).
6. Do not modify any other file in the repository.

After editing, run `ruff check --fix` (ignore unsafe fixes) to auto-fix any
whitespace/indent/formatting issues — let ruff handle formatting, don't fix
it by hand.
```

---

### Task 7: Add Organization JSON-LD Structured Data

```markdown
Objective:
Add a single valid `Organization` JSON-LD structured-data block to the
`<head>` of `index.html` to improve search-engine understanding of the page,
using only facts already present in the existing markup.

Target File:
- `index.html`

Context & Current Behavior:
The `<head>` (lines 3–124) contains no `<script type="application/ld+json">`
block today. The only brand facts available in the file are:
- Organization name: "CondoSense" (see nav brand text at line 129:
  `<div class="font-headline-md text-headline-md font-bold text-[#0F172A]">CondoSense</div>`,
  and footer brand text at line 244).
- Page `<title>` (line 6): "CondoSense | Institutional Compliance Auditing".
- Tagline/description text (lines 142–147, hero H1 and paragraph): automated
  HB 1021 compliance discrepancy auditing for Florida condominiums.

There is no known canonical production URL in the repo; use the same
placeholder as Task 4, `https://econ5000-ship-it.github.io/CondoSenseLandingPage/`, for the `url` field.
Do not invent a `logo`, `telephone`, `address`, or `sameAs` social profile
URLs, since none exist anywhere in the current markup — omit those fields
entirely rather than fabricate them.

Requirements & Constraints:
1. Immediately before the closing `</head>` tag (line 124), insert:

       <script type="application/ld+json">
       {
         "@context": "https://schema.org",
         "@type": "Organization",
         "name": "CondoSense",
         "url": "https://econ5000-ship-it.github.io/CondoSenseLandingPage/",
         "description": "Automated HB 1021 compliance discrepancy auditing for Florida condominiums."
       }
       </script>

2. Do not include any field not listed above (no `logo`, `sameAs`,
   `telephone`, `address`, etc.) since no such data exists in the page today.
3. Ensure the JSON inside the script block is syntactically valid JSON (no
   trailing commas, double-quoted keys/strings).
4. Do not modify any other tag in `<head>` — if Task 1 and/or Task 4 have
   already been applied to this file, leave their tags untouched and only
   insert this new script block right before `</head>`.
5. Keep the total diff under 100 lines (this task is ~9 lines of insertion).
6. Do not modify any other file in the repository.

After editing and if python changes occurred, run `ruff check --fix` (ignore unsafe fixes) to auto-fix any
whitespace/indent/formatting issues — let ruff handle formatting, don't fix
it by hand.
```
