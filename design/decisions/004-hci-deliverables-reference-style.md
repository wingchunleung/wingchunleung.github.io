# Design Decision Record 004: HCI Deliverables — Reference-Style Layout

**Date**: 2026-05-08
**Status**: Implemented (pre-deploy; local demo only)
**Supersedes**: The boxed `.assignment-card` markup introduced after DDR-003.
**Related**: DDR-001 (HCI Design Requirements), DDR-003 (HCI page content rewrite), `.claude/rules/src-pages.md`.

---

## 1. Context

The deliverables area on `/hci` boxes each assignment in a `border-left: 3px solid var(--color-celadon)` card with `padding: var(--space-5)` (32px) and a `--color-surface` background. That box was right when the page only had two assignments and a single status pill per phase. Two facts have outgrown it:

1. Phase 1 of the **Group Project** now embeds a custom 19-slide carousel; the carousel is constrained by the card's interior width.
2. Phase 1 of the **Final Project (Individual)** embeds a 16:9 video iframe; same constraint.

The user has signalled that **brainstorming photos, sketches, and process drawings** will be added next. The current card cannot host a wide image strip without further visual compression.

The user reference-pointed to the page's own Principles section ("the reference") as the visual register they want: calm, scannable, generous whitespace, no marketing-card chrome — but **with** the existing top-of-page TOC retained so a TA can jump straight to a specific assignment or to the references.

## 2. Constraints (locked before research)

| # | Constraint | Source |
|---|---|---|
| C1 | Keep the existing TOC pattern (Final Project, Group Project, plus a new "References" entry). | User decision, this session. |
| C2 | More room for embeds, slide carousels, and future brainstorming galleries. | User decision, this session. |
| C3 | Reference-style visual language: hairlines over boxes, generous margin, calm typographic rhythm. | User decision, this session. |
| C4 | Page-scoped change only. No edits to `BaseLayout`, `Header`, `Footer`, `IntroOverlay`, design tokens, or `astro.config`. | CLAUDE.md §Deployment, `.claude/rules/intro-overlay.md`. |
| C5 | "Artistic HCI" tone — quality from restraint, not flourish. | CLAUDE.md §Design Philosophy. |
| C6 | Local demo only this round. Do not commit. Do not push. Do not deploy. | User decision, this session. |

## 3. Phase A — Research (single research agent)

A general-purpose research agent surveyed reference-style design and research portfolios. Verified examples returned: Tufte CSS, Stanford HCI Group / Research, MIT Tangible Media / Projects, Shopify Polaris / Foundations, Stripe Docs, Bret Victor / worrydream.com.

**Patterns that recurred across all six:**

- Year-or-section headers do the dividing, not borders around content.
- Hairline rules **between** sections, never **around** them.
- A repeating typographic rhythm: display-serif H1 + monospace eyebrow + sans body — already the Ink & Signal stack (Cormorant Light, JetBrains Mono uppercase, Inter).
- Numbered top-level sections are rare; the TOC does the addressing.
- Full-width artifacts escape the prose-width column.

The right-gutter marginalia pattern (Tufte) was researched and is documented as a viable later move, but **deferred** in this round — adopting it now would expand scope past the user's brief.

## 4. Decision (Phase C)

Replace the boxed `.assignment-card` with an open `.deliverable` section. The data model (`Assignment`, `Phase`, `Artifact`) is unchanged — only the rendering and CSS change.

### Markup

```
<article id="assignment-{slug}" class="deliverable">
  <header class="deliverable-header">
    <span class="deliverable-eyebrow">Deliverable {number}</span>
    <h3 class="deliverable-title">{title}</h3>
  </header>
  <p class="deliverable-intro">{description?}</p>
  <ol class="phase-list" role="list">
    {phases — rich / disclosure / static modes unchanged}
  </ol>
</article>
```

### Layout

- `.assignments-section` wrapper drops `.content` (was 720px). Keeps `.container` (1200px).
- `.deliverable` is full container width. Hairline top divider, 96px top margin (88px after the first one collapses).
- Prose paragraphs stay capped at 65ch via the global `<p>` rule.
- Phase artifacts (video iframe, slide carousel, future image strips) fill the deliverable width — no card padding eating the embed.
- A new `.artifact-strip` utility lets future brainstorming galleries break into a responsive figure grid (`auto-fit, minmax(280px, 1fr)`) with mono `<figcaption>` per item.

### TOC

- Two hairlines (top + bottom) replace the celadon-tinted box.
- Each entry: mono celadon number + Cormorant Light title.
- Adds a third entry: **References** → `#references`.

### References section

- Receives `id="references"` and class `.references-section` so the TOC can anchor to it.
- The section label changes from "Reference" to "References"; the h2 stays "HCI principles" (still accurate — these are the principles being referenced).
- The 3-column principle grid is unchanged.

## 5. HCI principles in tension (and how this resolves them)

| # | Heuristic | Severity before | Severity after | Reason |
|---|---|---|---|---|
| H3 | Aesthetic & minimalist | 2 | 1 | Removing card chrome reduces decorative noise. The page's whole register was meant to argue for restraint; now it does. |
| H4 | User control & freedom | 2 | 1 | Wider embeds reduce horizontal scroll on the slide-viewer at narrow viewports. Anchor TOC retained. |
| H5 | Flexibility & efficiency | 3 | 2 | TOC retained and restyled for legibility; third entry makes References reachable in one click. |
| H6 | Recognition over recall | 1 | 1 | Mono eyebrow ("DELIVERABLE 01") + Cormorant display title is a stronger landmark than a `<h3>` inside a card. |

Severity scale per HCI-08 (`f(Frequency, Impact, Persistence)`).

## 6. Trade-offs (what was sacrificed)

- **Card surface signal.** A boxed card with a coloured stripe is an obvious "here is a thing" marker. Removing it relies on hairline + spacing rhythm. Mitigation: 96px top margin + 1px hairline; the rhythm is the point.
- **Surface-color contrast for readability.** `--color-surface` background is gone; deliverables sit on `--color-paper`. The page is more uniform — calmer, less perceived hierarchy. This is the cost of the reference register, paid willingly.
- **Marginalia is deferred.** The right-gutter callout pattern (Tufte sidenotes) was researched but not implemented. Future revisions can adopt it — the deliverable structure does not preclude it.

## 7. Open risks

- **Description-fade gradient colour.** The "Show more" fade in the rich-phase description used `--color-surface` because it sat on a card. Now it sits on `--color-bg` (paper). The CSS gradient end-stop is updated to match; if a future change re-introduces a card, this needs to flip back.
- **First-deliverable hairline.** A hairline above the first deliverable would be visually redundant with the TOC's bottom hairline. The CSS suppresses the top border on `:first-child`; if the markup is ever reordered (e.g., a wrapping `<header>` precedes the loop), the rule needs to be re-targeted.

## 8. Verification (Phase D self-evaluation)

Performed locally before reporting the demo URL:

1. **Render check** — `npm run dev`; visit `/hci`; confirm hero, TOC, both deliverables, and References anchor render with no console errors.
2. **TOC anchors** — clicking each entry scrolls to the right section with `scroll-margin-top` clearing the fixed header.
3. **Slide carousel** — fills deliverable width; prev/next + keyboard + click-zones still work.
4. **Video iframe** — fills deliverable width at 16:9.
5. **Show more / less** — description clip toggles cleanly; fade gradient blends into paper.
6. **Reduced motion** — no animations kick in; content remains readable.
7. **Mobile width (390px)** — TOC pills wrap, deliverables stack cleanly, embeds remain readable.
8. **Tab-order audit** — keyboard pass top-to-bottom: skip-link → header nav → TOC entries → first deliverable header → phase content → second deliverable → references.

**No deploy this round.** The "deploy from master root" pipeline (CLAUDE.md §Deployment) is a gated user-driven action.

---

## 9. Iteration 1 — Embed-as-figure refinement (same session, post-demo)

After viewing the first demo, the user noted the full-width embeds (video iframe at 1200px, slide carousel at 1200px) felt too banner-like. The deliverables read better when the media is a **contained figure** inside a wider editorial column, with a centered colophon-style acknowledgments block at the tail.

A second research pass surveyed Tufte CSS, Distill.pub, MIT Tangible Media, ACM CHI video paper landing pages, Are.na, Clagnut, and Stripe Press for the canonical "embed narrower than prose" pattern. Tufte CSS was the load-bearing reference: figure ≈ 55–63% of the surrounding text column, both capped (never full-bleed).

### Refinements

- `.phase-rich .phase-embed`, `.phase-rich .slide-viewer` — `max-width: 720px; margin-inline: auto`. ≈ 60% of the 1200px container. 16:9 video preserves comfortable viewing scale.
- `.phase-rich .phase-external` — `align-self: center` so the "Open in new tab" link sits under the centered figure.
- `.description-clip` — adds `max-width: 80ch` so the prose body extends past the figure but stays inside the comprehension-safe measure (Distill, Tufte both cap < 85ch). The `::after` fade gradient now aligns with the text.
- `.phase-acknowledgments` — converts from bordered card to centered colophon: `text-align: center; max-width: 56ch; margin: var(--space-5) auto 0; padding-top: var(--space-3); border-top: 1px solid var(--color-border)`. Drops the box, gains a film-credits register at the tail of each phase.

### Visual rhythm per rich phase, after this iteration

```
DELIVERABLE 01 (mono celadon, left)
Final Project (Individual) (Cormorant Light, left)
optional intro paragraph (65ch, left)
─────────────────────────────────────────────────────────
  Phase 1 · Video paper · Done (left)
  The Good, the Bad, the Trade-off (Cormorant Light celadon, left)

        [══════ Video iframe 720 px ══════]
                Open in new tab →

  Description prose at 80ch — wider than the figure but still
  inside the readable measure. Multiple paragraphs flow.

                     ─────
              ACKNOWLEDGMENTS
        Copyright: Some images used in the video...
        AI use: Edited by Grammarly...
─────────────────────────────────────────────────────────
  Phase 2 · Upcoming
```

Still no deploy. Demo on the same `npm run dev` instance.

---

## 10. Iteration 2 — Full-width description prose (same session, post-iteration-1 demo)

After viewing the iteration-1 demo, the user judged the centered figure (video, slides) at 720px to be the right size, and the centered colophon acknowledgments to be the right register, but explicitly asked for the description prose to span the **full** deliverable width — not capped at 80ch.

### Change

- `.description-clip` — `max-width: 80ch` removed. The clip now flows to the full deliverable width (≈ 1200px on desktop), matching the `<article class="deliverable">` parent.

### Trade-off (knowingly accepted)

At 1200px container width with `--text-small` Inter (14px), line measure is roughly 130–150 characters per line — outside the comprehension-safe 45–85ch range cited in HCI-03 (Visual Information Processing) and reinforced by the iteration-1 research (Tufte CSS, Distill.pub).

The user accepted this trade-off after seeing both states side-by-side. Reasoning: the deliverable area now reads as a single coherent block where the figure is the focus and the prose extends across the available space rather than sitting in a narrow column under it. This is the chosen visual rhythm for the page.

If a future revision wants to recover comprehension without losing the wide rhythm, candidates include:
- Multi-column flow (`column-count: 2; column-gap: var(--space-6)`) — keeps the wide footprint, restores per-column measure to ~60ch.
- Larger base font for descriptions (`var(--text-body)` instead of `var(--text-small)`) — fewer characters per line at the same pixel width.

These are deferred. The current revision matches the user's explicit instruction.

Still no deploy.

---

## 11. Iteration 3 — Defeat the global `<p>` cap, mitigate wide-line readability (same session)

After viewing iteration 2, the user reported the description **still** rendered narrow. Diagnosis: removing `max-width: 65ch` from `.description-para` did not free the paragraph because **`src/styles/global.css:75-77` defines `p { max-width: 65ch }` globally**. Iteration 2 deleted the local override, leaving the global rule in force.

A second concern surfaced from the typography research: at 14px Inter, `--color-text-secondary` (`#86868b`) computes to **3.59:1** contrast on white — already failing WCAG AA before any line-length consideration. At 130–150ch, the eye must track twice the distance per line; low-contrast wide-measure prose is the worst possible combination.

### Changes (this iteration)

- `.description-para`
  - `max-width: none` — the actual fix; defeats the global cap.
  - `color: var(--color-ink)` (#1d1d1f, ~16:1 on white — AAA) — replaces the failing wash grey.
  - `line-height: 1.85` (was `var(--leading-relaxed)` = 1.7) — leading must rise with measure (Bringhurst, Butterick).
  - `word-spacing: 0.03em` — loosens rivers on Inter at long measure.
- `.description-para + .description-para` — paragraph spacing bumped from `var(--space-3)` (16px ≈ 1.14em at 14px) to `1.5em` per WCAG 1.4.12 floor.
- `.description-body` — `max-height` recomputed from `5.4em` to `5.55em` (3 lines × 1.85 line-height) so the "Show more" clip still cuts cleanly at three lines.

### Why I did not propose multi-column / larger font (deferred from §10)

Both remain valid mitigations. The user's instruction in this round was unambiguous — make the existing prose flow full-width and keep the show-more behavior. Bumping line-height + contrast + word-spacing buys back a meaningful share of the readability cost without changing layout primitives.

### Sources cited in the iteration

- Butterick, *Practical Typography* — line spacing, line length.
- W3C WCAG 2.1/2.2 — SC 1.4.3 (Contrast), SC 1.4.8 (Visual Presentation), SC 1.4.12 (Text Spacing).
- WebAIM — Contrast and Color Accessibility.
- Bringhurst, *The Elements of Typographic Style*.

Still no deploy.
