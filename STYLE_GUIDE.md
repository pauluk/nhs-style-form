# NHS/GOV.UK-inspired Style Guide

Version 1.0 — 20 August 2026

This guide documents the visual language and interaction patterns used by the Risk Register example. It is intended to make the style reproducible in plain HTML, React, server-rendered Python applications, and Python UI frameworks.

The interface is inspired by NHS and GOV.UK service patterns: direct language, strong hierarchy, high contrast, visible focus, large interaction targets, progressive disclosure, and restrained decoration. It is an independent example rather than an official NHS or GOV.UK product. Do not imply official endorsement, and do not copy protected logos or branding.

## 1. Design principles

1. **Make the task obvious.** Each page has one clear purpose and one primary action.
2. **Prefer labels over symbols.** SVG graphics support text; they do not replace it.
3. **Design for scanning.** Use short headings, grouped fields, hints, summaries, and status panels.
4. **Show consequences early.** Preview calculated wording, scores, or output before submission.
5. **Keep people in control.** Generated or pre-filled content remains editable and requires an explicit commit.
6. **Reveal complexity progressively.** Put templates, advanced tools, and supporting JSON in collapsible sections.
7. **Treat accessibility as structure.** Semantic HTML, keyboard operation, focus order, error summaries, and live status are part of the component design.
8. **Validate twice.** Client-side feedback improves usability; server-side validation and calculation remain authoritative.

## 2. Design tokens

Use custom properties in HTML/CSS, a theme object in React, or equivalent constants in a Python template/theme layer.

```css
:root {
  /* Brand and action */
  --colour-blue: #005eb8;
  --colour-blue-dark: #003d78;
  --colour-green: #00703c;
  --colour-green-dark: #00401e;
  --colour-purple: #4c1d95;

  /* Status */
  --colour-focus: #ffdd00;
  --colour-error: #d4351c;
  --colour-warning: #f47738;
  --colour-success: #00703c;

  /* Text and surfaces */
  --colour-ink: #212b32;
  --colour-secondary-text: #4c6272;
  --colour-border: #b1b4b6;
  --colour-page: #f3f2f1;
  --colour-panel: #ffffff;
  --colour-panel-soft: #fafafa;
  --colour-grey: #e8edee;
  --colour-blue-wash: #e9f2f8;
  --colour-green-wash: #e9f5ee;
  --colour-yellow-wash: #fff7bf;

  /* Layout */
  --content-max: 1520px;
  --form-column: 520px;
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-5: 20px;
  --space-6: 24px;
  --space-8: 32px;
  --space-10: 40px;

  /* Shape and depth */
  --border-width: 2px;
  --radius-small: 4px;
  --radius-dialog: 18px;
  --shadow-panel: 0 2px 8px rgb(0 0 0 / 7%);
  --shadow-dialog: 0 24px 70px rgb(0 27 68 / 67%);
}
```

Do not communicate status using colour alone. Pair colour with a label such as "Critical", an SVG shape, or explanatory text.

## 3. Typography

Use a highly legible sans-serif system stack. Arial closely matches the example and avoids a font download.

```css
body {
  margin: 0;
  color: var(--colour-ink);
  background: var(--colour-page);
  font-family: Arial, Helvetica, sans-serif;
  font-size: 16px;
  line-height: 1.5;
}
h1 { margin: 0 0 8px; color: var(--colour-blue-dark); font-size: 34px; line-height: 1.12; }
h2 { margin: 0; color: var(--colour-blue-dark); font-size: 26px; line-height: 1.2; }
h3 { margin: 0 0 14px; font-size: 19px; line-height: 1.3; }
p  { max-width: 70ch; }
```

Recommended scale:

| Role | Desktop | Small screen | Weight |
|---|---:|---:|---:|
| Page title | 34px | 30px | 700 |
| Section title | 26px | 24px | 700 |
| Subheading/legend | 19px | 19px | 700 |
| Body and controls | 16px | 16px | 400/700 |
| Hint and metadata | 14px | 14px | 400 |
| Badge label | 12px | 12px | 700 |

Use sentence case. Avoid all capitals except short metadata badges. Keep paragraphs around 70 characters wide. Use bold for labels and important outcomes, not for whole paragraphs.

The risk statement preview may use Georgia as a deliberate editorial accent:

```css
.statement-preview { font: 19px/1.55 Georgia, serif; }
```

## 4. Page shell and responsive layout

The default desktop view has a constrained form beside a flexible result panel.

```css
* { box-sizing: border-box; }
.service-header {
  color: #fff;
  background: var(--colour-blue);
  border-bottom: 6px solid var(--colour-blue-dark);
}
.service-header__inner {
  max-width: var(--content-max);
  margin: auto;
  padding: 20px 24px;
  font-size: 23px;
  font-weight: 700;
}
.page-grid {
  display: grid;
  grid-template-columns: minmax(360px, var(--form-column)) minmax(0, 1fr);
  gap: 22px;
  max-width: var(--content-max);
  margin: auto;
  padding: 24px;
}
.panel {
  min-width: 0;
  background: var(--colour-panel);
  border: 1px solid var(--colour-border);
  box-shadow: var(--shadow-panel);
}
@media (max-width: 950px) {
  .page-grid { grid-template-columns: 1fr; }
}
@media (max-width: 560px) {
  .page-grid { padding: 10px; }
}
```

### View states

- **Normal:** form and result are visible.
- **Form collapsed:** hide the form and let the result occupy the full content width. Keep a visible "Show risk form" button.
- **Focus form mode:** hide the result and centre the form at a comfortable maximum width.
- **Mobile:** stack form above result; do not create horizontal scrolling.

Represent these as classes on a page root (`.is-form-collapsed`, `.is-form-focused`) or as React/Python state. Update `aria-expanded` or `aria-pressed` on the controls and move focus to a useful destination after the transition.

## 5. Panels, headings and grouped sections

Panel headings have 22–24px padding and a bottom divider. Forms use semantic `fieldset` and `legend` elements.

```css
.panel__heading { padding: 22px; border-bottom: 1px solid var(--colour-border); }
.form-body { padding: 22px; }
.form-group {
  margin: 0 0 22px;
  padding: 18px;
  background: var(--colour-panel-soft);
  border: 1px solid var(--colour-border);
  border-left: 6px solid var(--colour-blue);
}
.form-group > legend {
  padding: 0 8px;
  color: var(--colour-blue-dark);
  font-size: 19px;
  font-weight: 700;
}
```

Number long task sections, for example "1. Risk statement", "2. Assessment and ownership". Do not put every field in a separate card.

## 6. Form controls

Labels always precede fields. Hints go between the label and the field. Required status and errors must be expressed in text.

```css
label, .field-label { display: block; margin: 0 0 5px; font-weight: 700; }
.hint { display: block; margin: -2px 0 7px; color: var(--colour-secondary-text); font-size: 14px; }
input, textarea, select {
  width: 100%;
  margin: 0 0 16px;
  padding: 10px;
  color: var(--colour-ink);
  background: #fff;
  border: 2px solid var(--colour-secondary-text);
  border-radius: 0;
  font: inherit;
}
textarea { min-height: 76px; resize: vertical; }
button:focus-visible,
input:focus,
textarea:focus,
select:focus,
summary:focus-visible {
  outline: 3px solid var(--colour-focus);
  outline-offset: 0;
  box-shadow: inset 0 0 0 2px #0b0c0c;
}
```

Never remove the browser focus indication without replacing it. A yellow outer focus and dark inner edge is a defining feature of this style.

### Widths

Use widths that suggest the expected answer. A postcode, day, or score should not look like a paragraph field. Use `max-width` rather than fixed width when localization or zoom could expand text.

## 7. Boxed date pattern: day, month and year

Do not rely only on `<input type="date">`. For a consistent, accessible service-style date, present three visible boxes in the order **Day, Month, Year**. This avoids inconsistent browser date pickers and makes the expected format explicit.

Visual pattern:

```text
Review date
For example, 31 8 2026

Day       Month     Year
[ 31 ]    [ 8  ]    [ 2026 ]
```

Semantic HTML:

```html
<fieldset class="date-input" aria-describedby="review-date-hint review-date-error">
  <legend class="field-label">Review date</legend>
  <div id="review-date-hint" class="hint">For example, 31 8 2026</div>
  <p id="review-date-error" class="error-message" hidden>
    Enter a real review date using day, month and year.
  </p>
  <div class="date-input__items">
    <div class="date-input__item">
      <label for="review-day">Day</label>
      <input id="review-day" name="review_day" inputmode="numeric"
             autocomplete="off" maxlength="2" pattern="[0-9]*" size="2">
    </div>
    <div class="date-input__item">
      <label for="review-month">Month</label>
      <input id="review-month" name="review_month" inputmode="numeric"
             autocomplete="off" maxlength="2" pattern="[0-9]*" size="2">
    </div>
    <div class="date-input__item date-input__item--year">
      <label for="review-year">Year</label>
      <input id="review-year" name="review_year" inputmode="numeric"
             autocomplete="off" maxlength="4" pattern="[0-9]*" size="4">
    </div>
  </div>
</fieldset>
```

```css
.date-input { margin: 0 0 24px; padding: 0; border: 0; }
.date-input__items { display: flex; align-items: flex-end; gap: 20px; flex-wrap: wrap; }
.date-input__item { width: 72px; }
.date-input__item--year { width: 104px; }
.date-input__item label { font-size: 16px; font-weight: 400; }
.date-input__item input {
  min-height: 48px;
  margin-bottom: 0;
  padding: 8px;
  font-size: 19px;
  text-align: left;
}
```

Date behaviour contract:

- Keep the DOM and visual order Day → Month → Year for a UK-facing service.
- Accept one- or two-digit days/months; do not require leading zeroes.
- Do not auto-advance focus while a person is typing.
- Do not silently "correct" an invalid date such as 31 February.
- On submit, validate that all three parts exist, contain integers, and form a real calendar date.
- Apply an error border to the group and the relevant boxes; link the error using `aria-describedby`.
- Store and send a normalized ISO value (`YYYY-MM-DD`) after successful validation.
- When editing an ISO date, split it into the three display fields.
- Use a four-digit year unless a specific historical use case requires otherwise.
- If a date is optional, allow all three boxes to be empty; treat a partially entered date as an error.
- Do date arithmetic on the server using an explicit timezone. Display dates without timezone conversion unless the value represents a timestamp.

Normalization example:

```js
function readUkDate(prefix) {
  const day = Number(document.querySelector(`[name="${prefix}_day"]`).value);
  const month = Number(document.querySelector(`[name="${prefix}_month"]`).value);
  const year = Number(document.querySelector(`[name="${prefix}_year"]`).value);
  const candidate = new Date(Date.UTC(year, month - 1, day));
  const valid = candidate.getUTCFullYear() === year &&
    candidate.getUTCMonth() === month - 1 && candidate.getUTCDate() === day;
  if (!valid) return { value: null, error: "Enter a real date" };
  return { value: `${year.toString().padStart(4, "0")}-${String(month).padStart(2, "0")}-${String(day).padStart(2, "0")}` };
}
```

Do not use JavaScript parsing such as `new Date("31/08/2026")`; it is locale-dependent.

## 8. Large radio-button pattern

Use large radios when choices are important, mutually exclusive, and limited to roughly two to seven options. Each entire label row is clickable and has a minimum 48px target.

```html
<fieldset class="choice-group" aria-describedby="appetite-hint">
  <legend class="field-label">Risk appetite</legend>
  <div id="appetite-hint" class="hint">Select one option.</div>
  <label class="large-radio">
    <input type="radio" name="appetite" value="Low">
    <span class="large-radio__control" aria-hidden="true"></span>
    <span><strong>Low</strong><small>Only limited exposure is acceptable.</small></span>
  </label>
  <label class="large-radio">
    <input type="radio" name="appetite" value="Moderate">
    <span class="large-radio__control" aria-hidden="true"></span>
    <span><strong>Moderate</strong><small>Managed exposure may be accepted.</small></span>
  </label>
</fieldset>
```

```css
.choice-group { margin: 0 0 24px; padding: 0; border: 0; }
.large-radio {
  position: relative;
  display: grid;
  grid-template-columns: 36px minmax(0, 1fr);
  gap: 12px;
  align-items: start;
  max-width: 42rem;
  min-height: 52px;
  margin: 0 0 10px;
  padding: 10px 12px;
  background: #fff;
  border: 2px solid var(--colour-border);
  cursor: pointer;
}
.large-radio:hover { background: var(--colour-blue-wash); }
.large-radio input { position: absolute; width: 1px; height: 1px; opacity: 0; }
.large-radio__control {
  display: block;
  width: 32px;
  height: 32px;
  background: #fff;
  border: 3px solid var(--colour-ink);
  border-radius: 50%;
}
.large-radio input:checked + .large-radio__control {
  box-shadow: inset 0 0 0 7px #fff;
  background: var(--colour-ink);
}
.large-radio:has(input:checked) {
  border-color: var(--colour-blue);
  background: var(--colour-blue-wash);
}
.large-radio:has(input:focus-visible) {
  outline: 3px solid var(--colour-focus);
  outline-offset: 0;
  box-shadow: inset 0 0 0 2px #0b0c0c;
}
.large-radio strong, .large-radio small { display: block; }
.large-radio small { margin-top: 2px; color: var(--colour-secondary-text); font-size: 14px; }
```

If older browser support rules out `:has()`, toggle `.is-checked` and `.has-focus` classes from framework state, but retain the native radio input. Do not recreate selection exclusively with clickable `<div>` elements.

Large-radio behaviour contract:

- Arrow keys move between native radio options; Space selects.
- Clicking text, description, circle, or row selects the option.
- The selected state is visible without relying on colour.
- Use a `fieldset` and `legend` for the group question.
- Put conditional content immediately after its controlling option and manage focus when it appears.
- Do not preselect an answer when doing so could conceal a meaningful decision.

## 9. Buttons

Primary actions are green with a darker bottom edge. Secondary actions are white with a blue border. Destructive actions are red and must be clearly named.

```css
.button {
  display: inline-flex;
  gap: 8px;
  align-items: center;
  justify-content: center;
  min-height: 44px;
  padding: 10px 16px;
  color: #fff;
  background: var(--colour-green);
  border: 0;
  border-bottom: 4px solid var(--colour-green-dark);
  font: inherit;
  font-weight: 700;
  cursor: pointer;
}
.button:hover { filter: brightness(.88); }
.button:disabled { opacity: .65; cursor: not-allowed; }
.button--secondary {
  color: var(--colour-blue);
  background: #fff;
  border: 2px solid var(--colour-blue);
  border-bottom-width: 3px;
}
.button--secondary:hover { color: #fff; background: var(--colour-blue); }
```

Use verb-led labels: "Raise risk", "Use this draft", "Download PDF". During asynchronous work, disable only controls that would duplicate the operation and show a nearby text status.

## 10. Notices, errors and status

Use a thick left border and pale wash for notices. Keep status text in the document flow so layout does not jump.

```css
.notice { padding: 13px; border-left: 5px solid var(--colour-focus); background: var(--colour-yellow-wash); }
.notice--success { border-color: var(--colour-success); background: var(--colour-green-wash); }
.error-message { margin: 0 0 12px; color: var(--colour-error); font-weight: 700; }
.field--error { padding-left: 14px; border-left: 5px solid var(--colour-error); }
.field--error input { border-color: var(--colour-error); }
.status-line { min-height: 24px; font-weight: 700; }
```

For a failed submission, show an error summary at the top, focus it, and link each error to the corresponding control. For background progress, use `role="status"` or `aria-live="polite"`; reserve `role="alert"` for failures needing immediate attention.

## 11. Calculated previews and risk ratings

Display the structured risk statement and rating before submission. The rating cards use a four-column grid on wide screens and one column on narrow screens.

```css
.statement-preview {
  margin: 0 0 22px;
  padding: 18px;
  background: var(--colour-green-wash);
  border-left: 8px solid var(--colour-green);
}
.rating-grid { display: grid; grid-template-columns: repeat(4, 1fr); gap: 8px; }
.rating-card { padding: 10px; background: var(--colour-grey); border-top: 5px solid var(--colour-blue); }
.rating-card span { display: block; font-size: 12px; }
.rating-card strong { display: block; font-size: 20px; }
@media (max-width: 560px) { .rating-grid { grid-template-columns: 1fr; } }
```

Example deterministic 5×5 thresholds:

| Score | Rating | Suggested colour |
|---:|---|---|
| 1–4 | Low | `#00703c` |
| 5–9 | Moderate | `#ffdd00` with dark text |
| 10–16 | High | `#f47738` |
| 17–25 | Critical | `#d4351c` |

Show the equation, for example "Likelihood 4 × Impact 5 = 20 (Critical)". Calculate again on the server; never trust a client-submitted label or score.

## 12. SVG graphics

Use inline SVG symbols so graphics inherit the current text colour and remain sharp at any scale.

```html
<svg class="svg-sprite" aria-hidden="true">
  <defs>
    <symbol id="icon-check" viewBox="0 0 24 24">
      <path d="m5 12 4 4L19 6" />
    </symbol>
  </defs>
</svg>
<button class="button" type="button">
  <svg class="icon" aria-hidden="true" focusable="false"><use href="#icon-check"></use></svg>
  <span>Use this draft</span>
</button>
```

```css
.svg-sprite { position: absolute; width: 0; height: 0; overflow: hidden; }
.icon {
  width: 19px;
  height: 19px;
  flex: none;
  fill: none;
  stroke: currentColor;
  stroke-width: 1.8;
  stroke-linecap: round;
  stroke-linejoin: round;
}
```

Decorative SVGs use `aria-hidden="true"`. A standalone informative graphic uses `role="img"` and an accessible name. Always retain visible text on actionable controls.

## 13. Collapsible content and templates

Use native `<details>`/`<summary>` for simple progressive disclosure. Template selection should load fields but never immediately submit them.

```html
<details class="disclosure">
  <summary>Prefill from a risk template</summary>
  <div class="template-grid">...</div>
</details>
```

After loading a template:

1. Close the disclosure if that reduces clutter.
2. Announce which template loaded in the status line.
3. Keep every pre-filled field editable.
4. Recalculate previews.
5. Do not send data or consume a persistent identifier.

For a custom collapse button, use `aria-controls` and `aria-expanded`. The button remains available when the controlled panel is hidden.

## 14. Modal and generated-content pattern

Use the native `<dialog>` element where supported. The modal has a clear title, a close button with an accessible name, a constrained height, internal scrolling, and a dark backdrop.

The generated-content flow is:

```text
Choose context → adjust controls → generate draft → preview → commit or retry → edit form → submit
```

Generation must not automatically submit, send email, assign an ID, or overwrite the form before the person chooses "Use this draft". On close, return focus to the launch button. Escape closes the dialog unless an irreversible operation is underway.

## 15. Accessibility requirements

- Target WCAG 2.2 AA.
- Maintain at least 4.5:1 contrast for normal text and 3:1 for large text and meaningful non-text controls.
- Support keyboard-only use at 200% zoom and reflow at 320 CSS pixels.
- Use one page-level `h1`; keep heading levels sequential.
- Give every input a programmatic label.
- Use native `button`, `input`, `select`, `details`, `dialog`, `fieldset`, and `legend` before ARIA substitutes.
- Make pointer targets at least 44×44px; the large-radio pattern exceeds this.
- Never use placeholder text as the only label or hint.
- Respect `prefers-reduced-motion`; smooth scrolling and decorative animation must be optional.
- Provide a meaningful document title and skip link on full applications.
- Test with keyboard, screen reader, high contrast mode, browser zoom, and touch.

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { scroll-behavior: auto !important; animation-duration: .01ms !important; }
}
```

## 16. React implementation mapping

Build controlled or well-structured uncontrolled components without losing native semantics.

Suggested component boundaries:

```text
ServiceShell
├── ServiceHeader
├── RiskForm
│   ├── FormSection
│   ├── BoxedDateInput
│   ├── LargeRadioGroup
│   ├── StatementPreview
│   └── RatingSummary
├── ResultPanel
└── RiskStudioDialog
```

Rules:

- `BoxedDateInput` owns `{day, month, year}` for display and emits ISO only when valid.
- `LargeRadioGroup` renders native inputs sharing one `name`; derive IDs with `useId()`.
- Pass error IDs through `aria-describedby` and `aria-invalid`.
- Keep rating calculation in a shared pure function used by preview code and tested independently.
- Render untrusted/generated wording as text, not with `dangerouslySetInnerHTML`.
- Do not hide focus outlines in CSS resets.
- Use state for view modes and modal previews; use the server response as the submitted result.

## 17. Python framework mapping

### Server-rendered frameworks: Flask, Django, FastAPI/Jinja

- Put tokens and component CSS in a shared base template/static file.
- Create macros/partials for boxed dates, radio groups, error summaries, buttons, and notices.
- Parse boxed date parts explicitly and normalize them server-side.
- Re-render submitted values and per-field errors without discarding user input.
- Use POST for mutation and CSRF protection for authenticated browser forms.
- Escape all user and AI content by default.

### Component frameworks: NiceGUI, Reflex, Streamlit, Panel, Dash, Taipy

- Map tokens through theme configuration or narrowly scoped CSS classes.
- Prefer the framework's native accessible input where it preserves labels and keyboard behaviour.
- If a framework date widget cannot reproduce the three-box pattern, render three numeric/text inputs with a shared group label rather than visually faking a single date field.
- If a radio component cannot provide a 44px target and visible focus, wrap/skin the native radio or use a framework component with an equivalent accessibility contract.
- Store display date parts separately from the normalized API value.
- Recalculate previews reactively, but validate and rate risks again in the Python callback/backend.
- Keep API keys and email-provider calls exclusively on the server.

Frameworks may generate different DOM structures; matching the semantic and behavioural contract is more important than identical class names.

## 18. Data and API conventions

- Use `snake_case` field names in form/API payloads where Python is the backend.
- Use ISO 8601 dates (`YYYY-MM-DD`) at API boundaries.
- Represent likelihood and impact as integers 1–5.
- Return structured JSON alongside rendered HTML/PDF where integrations need it.
- Keep persistent ID generation server-side and serialized.
- Separate draft generation from final submission.
- Do not expose execution tokens or provider keys in browser source.
- Anonymous routes must restrict high-impact features such as arbitrary email recipients.

## 19. Content style

- Use plain English and active voice.
- Ask one thing per field.
- Put examples in hints: "For example, 31 8 2026".
- Explain why information is needed when it is not obvious.
- State errors as a correction: "Enter a real review date", not "Invalid input".
- Match button labels to the outcome.
- Use the risk syntax consistently: **If [cause/driver] happens, then [risk event] may occur, resulting in [business impact].**
- Avoid celebratory or playful language for severe outcomes. "Zest" may shape drafting tone but must not trivialize risk.

## 20. Definition-of-done checklist

Before calling an implementation complete, verify:

- [ ] Tokens and typography are applied consistently.
- [ ] Desktop, stacked mobile, form-only, and result-only layouts work.
- [ ] All controls have visible labels and keyboard focus.
- [ ] Dates use boxed Day, Month, Year inputs and normalize to ISO.
- [ ] Radio choices use native radios with a large clickable target.
- [ ] Required, partial, impossible, and boundary dates are tested.
- [ ] Errors are summarized, linked, and announced.
- [ ] SVGs support rather than replace text.
- [ ] Generated/template content is previewed and editable before commit.
- [ ] Client and server rating results agree at scores 4/5, 9/10, and 16/17.
- [ ] Submission loading, success, and failure states are visible.
- [ ] Secrets are absent from HTML, JavaScript bundles, URLs, and logs.
- [ ] The page works with keyboard, touch, 200% zoom, and reduced motion.
- [ ] HTML is valid and automated accessibility checks have no serious issues.
