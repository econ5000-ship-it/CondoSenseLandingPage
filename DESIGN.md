---
name: Institutional Trust
colors:
  surface: '#f8f9ff'
  surface-dim: '#cbdbf5'
  surface-bright: '#f8f9ff'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#eff4ff'
  surface-container: '#e5eeff'
  surface-container-high: '#dce9ff'
  surface-container-highest: '#d3e4fe'
  on-surface: '#0b1c30'
  on-surface-variant: '#45464d'
  inverse-surface: '#213145'
  inverse-on-surface: '#eaf1ff'
  outline: '#76777d'
  outline-variant: '#c6c6cd'
  surface-tint: '#565e74'
  primary: '#000000'
  on-primary: '#ffffff'
  primary-container: '#131b2e'
  on-primary-container: '#7c839b'
  inverse-primary: '#bec6e0'
  secondary: '#bb0112'
  on-secondary: '#ffffff'
  secondary-container: '#e02928'
  on-secondary-container: '#fffbff'
  tertiary: '#000000'
  on-tertiary: '#ffffff'
  tertiary-container: '#271901'
  on-tertiary-container: '#98805d'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#dae2fd'
  primary-fixed-dim: '#bec6e0'
  on-primary-fixed: '#131b2e'
  on-primary-fixed-variant: '#3f465c'
  secondary-fixed: '#ffdad6'
  secondary-fixed-dim: '#ffb4ab'
  on-secondary-fixed: '#410002'
  on-secondary-fixed-variant: '#93000b'
  tertiary-fixed: '#fcdeb5'
  tertiary-fixed-dim: '#dec29a'
  on-tertiary-fixed: '#271901'
  on-tertiary-fixed-variant: '#574425'
  background: '#f8f9ff'
  on-background: '#0b1c30'
  surface-variant: '#d3e4fe'
typography:
  display-lg:
    fontFamily: Source Serif 4
    fontSize: 48px
    fontWeight: '700'
    lineHeight: '1.2'
    letterSpacing: -0.02em
  headline-lg:
    fontFamily: Source Serif 4
    fontSize: 32px
    fontWeight: '600'
    lineHeight: '1.3'
  headline-lg-mobile:
    fontFamily: Source Serif 4
    fontSize: 24px
    fontWeight: '600'
    lineHeight: '1.3'
  headline-md:
    fontFamily: Source Serif 4
    fontSize: 24px
    fontWeight: '600'
    lineHeight: '1.4'
  body-lg:
    fontFamily: Public Sans
    fontSize: 18px
    fontWeight: '400'
    lineHeight: '1.6'
  body-md:
    fontFamily: Public Sans
    fontSize: 16px
    fontWeight: '400'
    lineHeight: '1.6'
  body-sm:
    fontFamily: Public Sans
    fontSize: 14px
    fontWeight: '400'
    lineHeight: '1.5'
  label-md:
    fontFamily: Public Sans
    fontSize: 12px
    fontWeight: '600'
    lineHeight: '1'
    letterSpacing: 0.05em
spacing:
  unit: 8px
  container-max: 1280px
  gutter: 24px
  margin-mobile: 16px
  margin-desktop: 40px
---

## Brand & Style
This design system is built upon the pillars of **Institutional Legal Trust**. It is designed for CondoSense to project an aura of unassailable authority, precision, and forensic clarity. The aesthetic follows a strict **Minimalist/Corporate** movement, stripping away decorative elements to focus on data integrity and professional conduct.

The target audience—condominium boards, legal counsel, and financial auditors—requires a UI that evokes stability and meticulousness. The emotional response should be one of security and confidence in the face of complex regulatory environments. High contrast and generous whitespace are used not just for aesthetics, but as functional tools to reduce cognitive load during intense document reviews and audits.

## Colors
The palette is intentionally restrained to maintain an institutional "paper-and-ink" feel. 

- **Primary (Slate):** Used for headlines, primary actions, and structural navigation. It provides the foundational weight of the interface.
- **Accent (Crimson Alert):** Reserved exclusively for risk metrics, critical failures, and legal non-compliance alerts. It must be used sparingly to maintain its psychological impact.
- **Neutrals:** A range of cool greys handle secondary information and structural boundaries. 
- **Surface:** A "Crisp White" background ensures maximum legibility and a clean, sterile environment for data entry.

## Typography
The typographic system utilizes a "Serif for Authority, Sans for Precision" pairing.

- **Headlines:** Source Serif 4 provides a literary, established feel reminiscent of legal filings and official reports.
- **Body & Data:** Public Sans is used for its exceptional legibility in dense tables and form fields. Its neutral, institutional character ensures that the data remains the focal point.
- **Hierarchy:** Use bold weights for primary data points and regular weights for narrative text. All caps should be reserved for small labels and table headers to signify category divisions.

## Layout & Spacing
The layout relies on a **Fixed Grid** system for desktop to ensure a predictable, document-like reading experience. 

- **Grid:** A 12-column grid with 24px gutters. Content should be centered with a maximum width of 1280px.
- **Rhythm:** Spacing follows a strict 8px base unit. 
- **Alignment:** Use rigid edge-to-edge alignment for data columns. Avoid staggered layouts; everything must feel structurally sound and "locked in."
- **Mobile:** Transition to a single-column layout with 16px side margins. Elements like data tables should allow for horizontal overflow with clear visual cues rather than shrinking text size.

## Elevation & Depth
In keeping with the auditing aesthetic, the system avoids physical metaphors like heavy shadows or neomorphism. 

- **Tonal Layers:** Depth is communicated through subtle background shifts (e.g., a light grey background for a side panel against a white main content area).
- **Outlines:** Use "Low-contrast outlines" (1px solid #E2E8F0) to define containers and cards. 
- **Interaction Depth:** Only apply a very slight, diffused shadow (4px blur, 2% opacity) on hover states to indicate interactivity. For the default state, elements should appear perfectly flat on the page.

## Shapes
To reinforce the legal and corporate tone, this design system uses **Sharp (0px)** corners for all primary UI components including buttons, cards, and input fields. Sharp corners suggest a no-nonsense, precise environment where boundaries are clearly defined and absolute.

## Components
Consistent component styling ensures the audit process feels systematic.

- **Buttons:** Solid #0F172A backgrounds with white Public Sans text for primary actions. Use a ghost style (Slate border, no fill) for secondary actions. Always 0px border radius.
- **Cards:** White background, 1px #E2E8F0 border, no shadow. Padding should be generous (minimum 24px) to keep content readable.
- **Input Fields:** Minimalist design with a bottom-border only or a very light 4-sided border. Use Slate for the label and a subtle grey for placeholder text. The focus state should utilize a 2px Slate bottom-border.
- **Chips/Status Indicators:** Use square-edged tags. For "Risk" or "Failed" statuses, use a Crimson background with white text. For "Compliant," use a subtle grey or Slate. Avoid bright greens; keep the palette sober.
- **Data Tables:** The core of the system. Use thin horizontal dividers only. Remove vertical lines to reduce visual noise. Row headers should be bold.
- **Risk Metrics:** Visualized using high-contrast Crimson line graphs or simple numerical blocks. Avoid playful icons; use standard legal symbols or clear text-based warnings.