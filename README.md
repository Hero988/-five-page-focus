# Five Page Focus — free accessibility triage demonstration

This repository shows how a five-page accessibility sample can become a smaller,
prioritised remediation backlog instead of an unreviewed scanner dump.

- [Read the completed report in Markdown](sample-report.md).
- Open `sample-report.html` for the standalone browser version.
- If GitHub Pages is enabled, `index.html` provides the short method overview and links
  to the browser report.
- The static privacy, service-terms and safe-intake pages provide the policy boundary
  for the future £199 review without collecting data through this site.

The example uses W3C's deliberately inaccessible CityLights “Before” demonstration.
It consolidates 203 automated affected elements and manual observations into eight
root-cause findings. No client site or customer information is included.

## The method

1. Choose one to five public pages that represent an important journey or set of
   shared templates.
2. Run an automated evidence pass, but treat every result as evidence to review—not a
   finished finding.
3. Check keyboard access, visible focus, structure, forms, images, links, contrast,
   language, and narrow-screen reflow manually.
4. Remove false positives and record reproducible evidence for the barriers that
   remain.
5. Consolidate repeated component defects into root causes, then prioritise by user
   impact, reach, and dependency.
6. Give every recommendation a specific re-test condition.

The result is sampled triage. It is not certification, legal advice, disabled-user
research, a complete WCAG conformance evaluation, or proof that every barrier was
found.

## Commercial disclosure

Five Page Focus sells a £29 local toolkit that packages this workflow with pinned
Playwright and axe-core dependencies, machine-assisted probes, a 12-part manual
checklist, report templates, a renderer, and the completed demonstration. This free
repository is useful without buying it and does not contain the reusable paid kit.
It also contains no form, tracker, cookie, remote font, customer evidence, payment
credential or live £199 checkout.

[View the £29 Five Page Focus Accessibility Triage Kit on Gumroad](https://gissuliman.gumroad.com/l/five-page-accessibility-triage-kit?utm_source=github&utm_medium=repository&utm_campaign=five_page_workflow)

Five Page Focus is a trading name of Suliman Tadros, sole trader, 2 Rosecroft Gardens,
London NW2 6HX. Product support: `gissuliman@gmail.com`. Please do not send client URLs,
credentials, evidence, or personal data in an initial support message.
