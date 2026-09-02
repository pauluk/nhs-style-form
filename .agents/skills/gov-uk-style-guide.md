# Skill: NHS/GOV.UK-inspired Style Guide

Use this skill whenever you are building or editing any UI in this repository
— a page, a form, a component, a mockup, or a variant — in HTML, React, or a
Python UI framework.

**Read the full guide first: [`STYLE_GUIDE.md`](../../STYLE_GUIDE.md)** (repo
root). It defines the complete design system: tokens, typography, layout,
the boxed date pattern, the large-radio pattern, buttons, notices, SVG usage,
accessibility requirements, and a definition-of-done checklist. Do not
improvise a different visual style for this repo — follow that document.

Quick reference (see `STYLE_GUIDE.md` for the full detail behind each):

- **Tokens** (section 2): use the `--colour-*`, `--space-*`, and shadow
  custom properties — don't hardcode colours or spacing.
- **Typography** (section 3): Arial-based system stack, sentence case,
  the documented type scale.
- **Layout** (section 4): constrained form + flexible result panel,
  responsive breakpoints at 950px and 560px.
- **Forms** (sections 6–8): labels before fields, hints between label and
  field, the boxed Day/Month/Year date pattern (never a bare
  `<input type="date">`), the large-radio pattern for important
  mutually-exclusive choices.
- **Buttons** (section 9): green primary, white/blue secondary, verb-led
  labels.
- **Accessibility** (section 15): WCAG 2.2 AA, visible focus always,
  44×44px minimum targets, semantic HTML before ARIA.
- **Before calling anything done**: check it against the definition-of-done
  checklist in section 20 of `STYLE_GUIDE.md`.

This is an independent design system inspired by NHS/GOV.UK patterns — it is
not an official NHS or GOV.UK product. Do not imply official endorsement and
do not use protected logos or branding.
