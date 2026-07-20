# CityLights accessibility triage — demonstration report

Prepared on: 20 July 2026  
Pages tested: 5  
Reference target: WCAG 2.2 Level A and AA

## What this report is

This is a sample deliverable using W3C's deliberately inaccessible CityLights
“Before” demo. It shows the format and evidence standard for the proposed service; it
is not client work.

The triage identifies likely barriers on five specified page regions and gives
prioritised remediation guidance. It is not certification, a full WCAG conformance
evaluation, legal advice, or a guarantee that every barrier has been found. W3C notes
that evaluation tools cannot determine accessibility and require human judgement.

## Executive summary

The highest-leverage fix is to replace the shared image-and-table template with
semantic HTML. Across the five sampled page regions, axe-core found **203 affected
elements across 19 page/rule combinations and five distinct rule types**. These are
occurrence counts, not an accessibility score and not a count of every WCAG failure.

Manual review found four additional cross-page patterns that automation did not count:
keyboard focus is intentionally discarded on links; visual headings and page regions
have no semantic structure; the 800px layout requires 480px of horizontal scrolling at
a 320px viewport; and the document language is undeclared. The Survey page adds a
task-blocking form-label pattern.

| Priority | Finding | Affected scope | Recommended owner |
|---|---|---|---|
| Critical | Missing image alternatives | 150 elements; all 5 pages | Content + engineering |
| Critical | Form controls have no accessible name | 17 elements; all pages, concentrated on Survey | Engineering |
| High | Image links have no discernible name | 23 links; all 5 pages | Engineering |
| High | Focus is removed from interactive elements | 32 explicit handlers; all 5 pages | Engineering |
| High | No semantic headings or landmarks in the demo region | All 5 pages | Engineering/content |
| High | Fixed-width content does not reflow | All 5 pages | Front-end/design |
| High | Text contrast is below the minimum | 13 elements; Home, Tickets, Template | Design/front-end |
| Medium | Page language is not declared | All 5 documents | Engineering |

## Scope and environment

The tested region was `#page`, excluding W3C's surrounding demonstration controls:

1. `https://www.w3.org/WAI/demos/bad/before/home.html`
2. `https://www.w3.org/WAI/demos/bad/before/news.html`
3. `https://www.w3.org/WAI/demos/bad/before/tickets.html`
4. `https://www.w3.org/WAI/demos/bad/before/survey.html`
5. `https://www.w3.org/WAI/demos/bad/before/template.html`

Tested 20 July 2026 in headless Chromium 149.0.7827.55 at 1440×1000 and 320×900 CSS
pixels. Automated evidence used axe-core 4.12.1 with its WCAG 2 A/AA, 2.1 A/AA and
2.2 AA tags. Manual review used keyboard/focus probes, DOM and accessibility structure
inspection, source review, and visual inspection of desktop and narrow screenshots.

Not tested: assistive-technology/browser combinations with a human screen-reader user,
mobile screen readers, captions/audio description (no relevant media in scope),
authenticated states, external destinations, or a complete user-research evaluation.
The `lang` check applies to the document root outside the scoped `#page` scan and is
therefore recorded as a manual finding.

## Findings

### F-01 — Images are unavailable or noisy to non-visual users

Priority: Critical  
Status: Confirmed  
WCAG mapping: 1.1.1 Non-text Content (A)  
Affected: 150 images across all five pages (Home 33, News 39, Tickets 26, Survey 24,
Template 28)

User impact: Screen-reader users may miss meaningful content and linked destinations.
Decorative layout slices without empty alternatives can also create a long stream of
irrelevant file information.

Evidence: axe rule `image-alt`. Example:
`img[src$="border_left_top.gif"]` has no `alt` attribute. Raw selectors and HTML are
stored in each page JSON file. The Home screenshot also shows content images whose
purpose must be judged in context rather than assigned generic text.

Recommended fix: Classify every image by purpose. Give informative images concise,
context-specific alternatives; give decorative images `alt=""`; give image links an
accessible name that describes the destination. Prefer CSS for visual decoration and
real text for text rendered as images. Do not mechanically add filenames or the word
“image”.

Re-test: Inspect the accessibility tree and read the page in sequence. Every meaningful
image should convey an equivalent purpose once; decorative images should be silent;
the automated `image-alt` rule should report no confirmed violations.

### F-02 — Form controls have no programmatic labels

Priority: Critical  
Status: Confirmed  
WCAG mapping: 1.3.1 Info and Relationships (A), 3.3.2 Labels or Instructions (A),
4.1.2 Name, Role, Value (A)  
Affected: 17 automated occurrences: 11 inputs on Survey and six unnamed select
elements across the sample

User impact: Screen-reader and voice-input users cannot reliably discover what the
quick menu, radio buttons, text inputs, or Survey select request. The visual proximity
of text does not create a programmatic relationship.

Evidence: axe rules `label` and `select-name`. Example:
`input[type="radio"][name="res"][value="1"]` has no associated `label`, `aria-label`,
or `aria-labelledby`. The manual inventory found 13 Survey controls without a
programmatic label when all input/select elements were considered.

Recommended fix: Use visible `<label for>` text with unique control IDs. Wrap related
radio groups in `<fieldset>` with a descriptive `<legend>`. Give each select a persistent
visible label. Keep instructions and error references programmatically connected with
`aria-describedby` only where native HTML is insufficient.

Re-test: Navigate controls with a keyboard and inspect their computed accessible names.
Each name should be unique enough to answer “what value belongs here?” without nearby
visual context.

### F-03 — Image links have no discernible destination

Priority: High  
Status: Confirmed  
WCAG mapping: 2.4.4 Link Purpose (In Context) (A), 4.1.2 Name, Role, Value (A)  
Affected: 23 links across all five pages

User impact: A screen-reader links list contains unnamed items, so navigation becomes
guesswork. Voice users also lack a reliable name by which to activate the link.

Evidence: axe rule `link-name`. Example navigation link:
`a[href="javascript:location.href='home.html';"]` contains an image without an
accessible alternative and has no other accessible text.

Recommended fix: Use ordinary destination URLs in `href` and meaningful text. If an
image must be the only link content, its alternative should name the destination.
Avoid redundant `aria-label` values that conflict with visible text.

Re-test: Review the links list without surrounding visual layout. Every item should
identify its destination or action, and activation should work without JavaScript.

### F-04 — Focus is deliberately removed from links

Priority: High  
Status: Confirmed by focus probe and source inspection  
WCAG mapping: 2.1.1 Keyboard (A), 2.4.7 Focus Visible (AA), 3.2.1 On Focus (A)  
Affected: 32 elements with explicit focus-blur handlers across all five pages

User impact: Keyboard users lose their position and may be unable to perceive or
operate primary navigation. The behaviour also breaks predictable focus order.

Evidence: Programmatically focusing the sampled navigation links did not retain focus.
Example: `onfocus="blur();"` on the Home link produced `retainedFocus: false` and no
visible outline. Counts by page: Home 14, News 4, Tickets 4, Survey 4, Template 6.

Recommended fix: Remove all focus-blur handlers. Use native links/buttons and provide a
high-contrast `:focus-visible` indicator that is at least as clear as the hover state.
Do not change context merely because a control receives focus.

Re-test: Starting at the browser chrome, traverse the complete page with `Tab` and
`Shift+Tab`. Focus should remain visible, move logically, and permit activation with
`Enter` or `Space` as appropriate.

### F-05 — Visual structure has no semantic equivalent

Priority: High  
Status: Confirmed by DOM inspection  
WCAG mapping: 1.3.1 Info and Relationships (A), 2.4.1 Bypass Blocks (A), 2.4.6
Headings and Labels (AA)  
Affected: all five pages

User impact: Screen-reader users cannot navigate by heading or region, and the table-
based layout produces a less understandable reading structure when presentation is
removed.

Evidence: Within each `#page` region, the inventory found zero `h1`–`h6` elements,
zero native landmarks, zero ARIA landmarks, and no local skip link. Text such as
“Welcome to CityLights” and “Citylights Survey” is styled visually instead of marked
as a heading.

Recommended fix: Build the shared page shell with `header`, `nav`, `main`, and `footer`;
use a descriptive page `h1` and nested headings based on content structure. Add a skip
link that moves focus to `main`. Use data tables only for data and include correct
headers; use CSS layout for presentation.

Re-test: Inspect the heading outline, landmark list, and CSS-free reading order. They
should provide a concise map that matches the visual page.

### F-06 — Fixed-width content does not reflow

Priority: High  
Status: Confirmed by viewport measurement and screenshot  
WCAG mapping: 1.4.10 Reflow (AA)  
Affected: all five pages

User impact: People who zoom substantially or use a narrow screen must pan horizontally
to follow lines and operate controls, making content easy to lose.

Evidence: At a 320 CSS-pixel viewport, each scoped region retained an 800px scroll
width—**480px wider than the viewport**. The narrow screenshots show the desktop layout
cropped rather than reorganised.

Recommended fix: Replace fixed table widths and pixel-positioned layout with responsive
CSS. Allow text and content columns to wrap; stack secondary columns; keep horizontal
scrolling only for genuine two-dimensional content such as a wide data table.

Re-test: At 320 CSS pixels and at 400% browser zoom from a 1280px desktop viewport,
users should read and operate ordinary content without horizontal scrolling.

### F-07 — Text contrast is below the minimum

Priority: High  
Status: Confirmed automatically and visually spot-checked  
WCAG mapping: 1.4.3 Contrast (Minimum) (AA)  
Affected: 13 elements on Home (2), Tickets (9), and Template (2)

User impact: Low-vision users and people using low-quality or glare-affected displays
may not be able to read key labels and table content.

Evidence: axe rule `color-contrast`. Example: foreground `#41545d` on background
`#a9b8bf` measured **3.88:1** for text that requires 4.5:1.

Recommended fix: Update design tokens rather than patching instances. Use a foreground/
background pair of at least 4.5:1 for normal text and 3:1 only where the text genuinely
meets WCAG's large-text definition. Recheck hover, focus, selected, and disabled states.

Re-test: Measure rendered colours at the actual font size/weight and confirm the
minimum ratio in every state.

### F-08 — The document language is undeclared

Priority: Medium  
Status: Confirmed by document-root inspection  
WCAG mapping: 3.1.1 Language of Page (A)  
Affected: all five documents

User impact: Screen readers may choose the wrong pronunciation rules and voice.

Evidence: `document.documentElement.getAttribute('lang')` returned `null` on every URL.

Recommended fix: Add the correct BCP 47 language to the root, for example
`<html lang="en">`, and mark meaningful passages in another language separately.

Re-test: Inspect the DOM and accessibility tree and confirm the default and any
language changes match the content.

## Recommended remediation sequence

1. Rebuild the shared shell once: semantic regions/headings, responsive layout, native
   links, preserved focus, named quick menu, document language, and image policy.
2. Fix the Survey form with labels, fieldset/legend grouping, and accessible validation;
   then complete an empty/invalid/valid submission keyboard test.
3. Correct shared colour tokens and review the remaining meaningful images and link
   purposes page by page.
4. Re-run the same five-page configuration and manual checks, recording fixed, partly
   fixed, not fixed, or unable to retest for each finding.

## Limitations and next step

This triage intentionally samples five public page regions. A full evaluation would
also cover complete user processes, responsive states, supported assistive-technology
combinations, documents and media, dynamic behaviours, and representative disabled-
user testing. Fixing the items here should materially reduce barriers, but a clean
re-test would not by itself establish WCAG conformance.

## Appendix A — Page-level automated summary

| Page | Distinct rule violations | Affected elements | Needs-review items | Passed automated rules |
|---|---:|---:|---:|---:|
| Home | 4 | 43 | 0 | 9 |
| News | 3 | 44 | 0 | 9 |
| Tickets | 4 | 40 | 0 | 9 |
| Survey | 4 | 41 | 0 | 10 |
| Template | 4 | 35 | 0 | 8 |
| **Total** | **19 page/rule combinations** | **203** | **0** | Not summed as a score |

## Appendix B — Manual results

| Check | Result | Evidence |
|---|---|---|
| Titles distinguish sampled pages | Pass in sampled state | Unique document titles in `manual-probes.json` |
| Document language | Barrier found | `declaredLanguage: null` on all five pages |
| Keyboard focus retention/visibility | Barrier found | 32 scoped elements failed to retain focus |
| Headings and landmarks | Barrier found | 0 headings and 0 landmarks in each `#page` region |
| Narrow reflow | Barrier found | 800px content at 320px; 480px overflow on every page |
| Image purpose | Barrier found/needs content judgement | 150 missing alternatives; context review required |
| Control names | Barrier found | Automated label/select rules plus Survey inventory |
| Contrast | Barrier found | 13 confirmed elements; example ratio 3.88:1 |
| Screen-reader/browser user test | Not performed | Outside this triage's demonstrated scope |
| Media, authentication, complex dynamic states | N/A/not sampled | No representative content or state in scope |

## Appendix C — Evidence index

- `evidence/2026-07-20/summary.json` — configuration and page counts
- `evidence/2026-07-20/{page}.json` — raw axe results, selectors, HTML, help URLs
- `evidence/2026-07-20/manual-probes.json` — structure, focus, and reflow probes
- `evidence/2026-07-20/screenshots/` — full desktop and 320px screenshots
- `pages.json` and `scripts/` — exact page scope and repeatable collection code

Primary references: [WCAG 2.2](https://www.w3.org/TR/WCAG22/),
[W3C evaluation-tool limitations](https://www.w3.org/WAI/test-evaluate/tools/selecting/),
and [W3C Easy Checks](https://www.w3.org/WAI/test-evaluate/preliminary/).
