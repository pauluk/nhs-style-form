# Agent instructions for this repo

This repository's UI must follow the NHS/GOV.UK-inspired design system
documented in [`STYLE_GUIDE.md`](STYLE_GUIDE.md) — see also the loaded skill
at `.agents/skills/gov-uk-style-guide.md` for a quick-reference summary.

Before building or editing any page, form, or component here:
1. Read `STYLE_GUIDE.md` (or the skill summary) first.
2. Use the documented design tokens, not ad-hoc colours/spacing.
3. Follow the boxed-date and large-radio patterns exactly as specified —
   these are the two most commonly missed pieces.
4. Check new work against the definition-of-done checklist (section 20 of
   `STYLE_GUIDE.md`) before considering it finished.

A known failure mode worth checking every time: it's easy to add HTML
markup with the right class names but forget the matching CSS rules for
those classes, especially when building incrementally across several
prompts. Verify every class used in the HTML has a corresponding CSS rule
before calling anything done.
