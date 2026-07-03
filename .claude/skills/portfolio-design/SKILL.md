---
name: portfolio-design
description: >
  Design system and house style for Marcos Moreno's personal portfolio (the Hugo
  site in this repo). Use this skill for ANY visual or copy change to the site:
  editing the hero, sections, colours, typography, spacing, animations, or copy;
  adding or restyling a project, experience entry, achievement, or skill; or
  general "make it pop / give it more hook" requests. Apply it even when the user
  says something casual like "tweak the site", "change the accent colour", "add a
  project" or "make the hero stronger" without naming the design system. The point
  of the skill is that every change stays inside one coherent visual language
  instead of drifting back to a generic template.
---

# Marcos Moreno — portfolio design system

## What this site is

Personal portfolio for **Marcos Moreno Martínez**, an AI Engineering Leader
(Computer Vision / ML, based in Spain). The audience is **CTOs and technical
recruiters** skimming for ~7 seconds. The page has one job: prove, fast, that he
ships AI in production *and* leads people.

It's a **Hugo** site (originally the `hugo-profile` theme). Treat the theme as a
starting point, not a constraint — the design language below overrides theme
defaults.

## The thesis: direct the eye, don't add noise

The redesign is not "more colours and more animation". It's **attention
direction**, grounded in three real principles. Keep these in mind for every
change, because they explain *why* the rules exist:

1. **Visual saliency / pop-out.** On a desaturated field, one saturated colour is
   found pre-attentively. So the page is warm-paper neutral and the signal accent
   appears *only* on what a recruiter must see first. If the accent spreads, it
   stops working. This is the most important rule — guard it.
2. **Luminance contrast + numeric anchoring.** The brightest, largest elements
   should be the name and the hard metrics (6 yrs, 30+ models, 8 people, 92%
   recall). Recruiters scan in an F/Z pattern; the numbers are the argument.
3. **Isolation effect (von Restorff).** Most AI portfolios are dark-with-neon.
   This one is deliberately the opposite — a light "instrument / lab" aesthetic —
   so it's remembered. Do not drift it toward the dark-neon cliché.

## Design tokens — use these exactly

```css
:root{
  /* surfaces (warm paper, lab/notebook feel) */
  --paper:#FAF8F3; --paper-2:#F1EEE6; --card:#FFFFFF;
  /* ink */
  --ink:#15181C; --ink-2:#565C63; --ink-3:#8A9098;
  --line:#E5E0D5;
  /* THE signal accent — reserved, never decorative */
  --accent:#FF4A1C; --accent-ink:#B5310E; --accent-soft:rgba(255,74,28,.10);
  /* type */
  --disp:"Space Grotesk",sans-serif;   /* headlines, name, metric numbers */
  --body:"Inter",sans-serif;            /* paragraphs */
  --mono:"JetBrains Mono",monospace;    /* eyebrows, labels, metrics labels, data */
}
```

- Background carries a faint dotted grid (`radial-gradient` dots, ~26px, `--line`)
  — reads like an annotation/labeling canvas. Keep it barely-there.
- Type scale: display weights 500/700 only; body 400/500. Headlines tight
  (`letter-spacing:-.02em`, `line-height:.98` for the H1). Sentence case
  everywhere — never Title Case, never ALL CAPS.

## Accent discipline (the rule that protects the design)

`--accent` is allowed on, and basically *only* on:
the live "detection" graphics, the confidence bar, the **primary CTA**, the
**key metric numbers/units**, and the active nav state. Everything else is
ink/neutral. Skill/tech pills are ALWAYS neutral — the owner explicitly
rejected accent ("hot") pills; do not reintroduce them.

Before shipping any change, count the accent uses on screen. If it's creeping
onto borders, backgrounds, icons, every link — pull it back. Scarcity is the
mechanism.

## Signature motif: "detection" (vision-model vernacular)

The page treats the visitor as if Marcos's own vision model is inferring on the
page — this is authentically his world (YOLOv3, IoU, recall) and nobody can copy
it. Use it with restraint:

- **Hero portrait:** a bounding box that *draws/locks on* once on load, plus a
  single confidence read-out that ticks `0.00 → 0.98`. Corner-bracket framing is
  fine.
- **Eyebrows / section markers:** monospace, e.g. `[ experience ]`,
  `// ml & deep learning`, `inference: live`.
- **Metrics:** present as readouts — lowercase mono label (`models_shipped`) above
  a big display number with the unit in accent.

**Hard don'ts for the motif** (learned from user feedback):
- A detection label on the box is the motif; chips floating over the
  face/body are not. KEEP a single YOLO-style label anchored to the top edge
  of the bounding box (e.g. `ai_engineering_leader · 0.98`) — it's the
  signature and it's wanted. Do NOT stick additional category chips over the
  face or torso (no `computer_vision`, `team_lead`, etc. floating). One label
  on the box: yes. Chips over the person: no.
- Don't let the motif leak into every element. It's seasoning, not the meal.

## Motion — deliberate and counted

Allowed: bounding box lock-on (once, on load), confidence count-up, metric
numbers counting up when scrolled into view (IntersectionObserver), gentle
fade-up reveals on sections, small hover lifts on buttons/pills. That's the
budget — resist adding more. Over-animation is the #1 tell of AI-generated
design.

Always honour `prefers-reduced-motion: reduce`: disable transitions/animations
and render final states (box drawn, numbers at final value, sections visible).

## Quality floor (non-negotiable)

- Responsive down to mobile (hero collapses to one column ≤860px; metrics to 2
  columns ≤760px; nav links may hide behind a menu on mobile).
- Visible keyboard focus on every interactive element.
- Real contrast: body text is `--ink-2` on `--paper`; never put low-contrast grey
  on grey. Accent text on light uses `--accent-ink`, not `--accent`.
- Keep the real content and real numbers. Generic copy makes the design feel as
  templated as a generic layout would.

## How to integrate changes in this Hugo repo

Before editing, **inspect the repo first** (`hugo.toml`/`config`, `layouts/`,
`assets/`, `data/`, `themes/`). Then prefer, in order:

1. **Config / data files** for content (projects, experience, achievements,
   social links) — add a project by adding a data entry, not by hand-coding HTML,
   when the theme supports it.
2. **Override files** in the project root (`assets/` custom SCSS/CSS,
   `layouts/partials/` overrides) rather than editing vendored files under
   `themes/`. Overrides keep theme updates painless and diffs clean.
3. Load the three webfonts once (Space Grotesk, Inter, JetBrains Mono) in the
   head partial; don't import per-component.

Keep commits small and scoped (one concern per commit). Run `hugo server` /
build and sanity-check the hero, metrics, and mobile width after each change.

## Worked example — adding a project card

Stay in the system: card on `--card`, 1px `--line`, radius ~14px; a mono
`[ project ]` eyebrow; title in `--disp` 500; the tech stack as neutral mono
pills (never accent — owner preference); a short plain-language outcome
sentence led by a hard number if there is one (e.g. "92% recall, 75% IoU").
No accent on the card border or background.

## Self-check before finishing any task

- Is the accent still scarce and reserved? (count it)
- Are the hard metrics the brightest/biggest thing in their section?
- Did I keep it light/paper — not drift to dark-neon?
- No literal category labels stuck on the portrait?
- Reduced-motion + keyboard focus + mobile all still good?
- Is the copy specific and real, not filler?

A reference mockup of the target look can live at
`reference/mockup.html` in this skill folder — open it to recalibrate the vibe
before a big change.
