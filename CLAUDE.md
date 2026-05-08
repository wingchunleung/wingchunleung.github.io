# HCI-Driven Website Project

## Project Overview
This is an HCI-driven website project where **all design and development decisions must be grounded in Human-Computer Interaction principles** from the ARIN5301 course (Prof. Xiaojuan Ma, HKUST).

This is a **research-driven HCI design project**, not just a coding project.

## Design Philosophy
- Prioritize **usability over aesthetics** (but recognize "attractive things work better" - Norman)
- Every design decision must reference an HCI principle
- Follow the **Design Thinking Process**: Empathize → Interpret → Ideate → Verify
- Start with **user needs**, not solutions or problems
- Balance Aesthetic and Goals (neither dominates)

## Mandatory Workflow (Before Any Coding)

### Phase A: Deep Research
- Analyze HCI materials from `/home/wing/Documents/HCI/`
- Research best practices for the specific design problem
- Identify applicable HCI principles from `docs/hci-knowledge-base.md`

### Phase B: Multi-Agent Design Thinking
- Use 8+ agents to propose different UI/UX approaches
- Agents must critique each other's proposals
- Evaluate every proposal against Nielsen's 10 Heuristics
- Evaluate against Gestalt principles
- Compare trade-offs (usability vs aesthetics vs cognitive load)

### Phase C: Decision Phase
- Combine or select the best approach
- **Justify decisions using specific HCI principles**
- Document the reasoning in design decision records

### Phase D: Implementation
- Only after A-C are complete
- Code must reflect the design decisions
- Self-evaluate against heuristics after implementation

## Core HCI Principles (Quick Reference)

### Nielsen's 10 Usability Heuristics
1. **Consistency** - Internal, external, and metaphorical consistency; principle of least surprise
2. **Mapping** - Match between system and real world
3. **Minimalism** - Aesthetic and minimalist design; Occam's Razor
4. **Freedom** - User control and freedom; undo/redo
5. **Flexibility** - Flexibility and efficiency of use; shortcuts for experts
6. **Recognition** - Recognition rather than recall; reduce memory load
7. **Visibility** - Visibility of system status; feedback
8. **Error Prevention** - Prevent errors before they occur
9. **Error Recovery** - Help users recognize, diagnose, and recover from errors
10. **Help** - Help and documentation

### Gestalt Principles (Visual Perception)
- **Closure** - Users complete incomplete shapes mentally
- **Continuation** - Elements aligned on a line/curve are perceived as related
- **Common Fate** - Elements moving together are perceived as grouped
- **Symmetry** - Symmetrical elements are perceived as belonging together
- **Figure-Ground** - Users distinguish foreground from background
- **Similarity** - Similar elements are perceived as grouped
- **Proximity** - Close elements are perceived as related

### Three Levels of Design (Norman)
- **Visceral** - Immediate emotional/sensory reaction (look & feel)
- **Behavioral** - Usability and function (does it work well?)
- **Reflective** - Meaning and self-image (what does it say about me?)

### Human Capabilities to Design For
- **Vision**: 80% of learning is through seeing and doing; consider color blindness (8% male, 0.5% female)
- **Memory**: Short-term (working) memory is limited (~7 items); reduce cognitive load
- **Cognition**: Attention is a scarce resource; minimize extraneous cognitive load
- **Perception**: Design for visual information processing (Features → Patterns → GIST)

### User Experience Models
- **Implementation Model** ← Representation Model → **Mental Model**
- Better design = Representation closer to Mental Model
- Reduce gap between what system does and what user thinks it does

### Design Thinking Process
1. **Empathize**: Immerse, Observe, Contact (interviews)
2. **Interpret**: Organize → Construct Framework (HTA/GOMS) → Generate POV
3. **Ideate**: Brainstorm solutions based on POV
4. **Verify**: Prototype and test with users

### Evaluation Methods
- **Heuristic Analysis**: Expert inspection against usability heuristics (Rule of Five: 5 evaluators catch ~75% of problems)
- **Questionnaires**: Likert scales, SUS, structured surveys
- **Usability Testing**: Task-based testing with real users
- **Severity Rating**: 0 (not a problem) → 4 (usability catastrophe)

## HCI Course Materials
PDF lecture notes are stored at: `/home/wing/Documents/HCI/`
- HCI-01: Introduction to HCI
- HCI-02: User-Centered Design
- HCI-03: Understanding Humans (perception, cognition, Gestalt)
- HCI-04: Design Empathize
- HCI-05: Design Ideate (HTA, GOMS, Activity Theory, Personas)
- HCI-07: Evaluation & Questionnaire Design
- HCI-08: Evaluation Heuristics (Nielsen's 10)
- HCI-11: Multimodal Interaction
- HCI-13: Human-Robot Interaction
- HCI-14: Ubiquitous Computing
- HCI-15: XR (Extended Reality)

## Technical Stack
- To be determined after HCI research phase
- Technology choices must be justified by HCI requirements

## Constraints
- All design decisions must be traceable to HCI principles
- No arbitrary UI choices
- Accessibility is mandatory (WCAG compliance)
- Consider cultural context in design (usability and aesthetics are user-dependent)
- Mobile-first responsive design (consider Fitts's Law for touch targets)

## Project Structure
```
/home/wing/website/
├── CLAUDE.md              # This file - project context
├── docs/
│   └── hci-knowledge-base.md  # Extracted HCI principles
├── design/                # Design decisions and artifacts
│   └── decisions/         # Design decision records
├── src/                   # Source code (after design phase)
├── public/                # Static assets
└── tests/                 # Tests including usability tests
```

## Deployment

**Live URL:** https://wingchunleung.github.io/website/

**GitHub Pages source:** `master` branch, `/ (root)` folder. Configured
under *GitHub → Settings → Pages → Build and deployment → Source: Deploy
from a branch*.

This means GitHub Pages serves the static files committed at the **root**
of `master`. The built output of `npm run build` (the contents of `dist/`)
is placed at the repo root and committed alongside the `src/` source —
that is why directories like `_astro/`, `about/`, `hci/`, `projects/`,
and files like `index.html`, `404.html`, `sitemap-*.xml` exist at the
repo root next to source folders. `.nojekyll` at the repo root is
required so GitHub Pages does not filter out the `_astro/` directory.

`astro.config.mjs` therefore uses `base: '/'` (not `/website`), since the
deployment is served from the root of the repository, not a subpath.

**Do not** switch the Pages source back to a `gh-pages` branch or to
GitHub Actions without an explicit user request — this "deploy from
master root" setup is intentional and replaces the older gh-pages /
GitHub Actions configurations referenced in some older skill files
(`.claude/skills/project-context/tech-stack.md`,
`.claude/skills/project-context/SKILL.md`) and DDRs.

## Canonical Landing-Page Intro Animation (LOCKED)

The brand-moment animation that plays once per session after the lock-screen
PIN is the **only** approved intro animation for this site. It survived 100+
prior iterations and is the design the user has chosen to keep.

**Source of truth:** `src/components/IntroOverlay.astro`. All HTML, CSS, and
JS for the animation live there. The homepage (`src/pages/index.astro`) only
imports and renders `<IntroOverlay />` — it must not contain inline intro
markup, intro CSS, or intro JS.

**Public contract with `LockScreen.astro`:**
- LockScreen sets `sessionStorage 'site-unlocked' = '1'` after a correct PIN.
- LockScreen dispatches a `window` `site-unlocked` CustomEvent in the same
  setTimeout that removes its DOM. The IntroOverlay listens for that event.
- The IntroOverlay sets `sessionStorage 'intro-seen' = '1'` on first run so
  returning visits in the same session skip the animation.

**Sequence (3.9 seconds total, do not modify timings without an explicit DDR):**
- 0 ms: spawn 35 (mobile) / 65 (desktop) free-floating "wow" + 👀 particles
- 800 ms: begin 600 ms opacity fade-out
- 1100 ms: "see who this is 👀✨" tease pops in (back.out)
- 2000 ms: tease fades out
- 2300 ms: 👋 fades in, waves three times, fades out
- 3300 ms: hero reveals + iris-close exit (clip-path)

**Read `.claude/rules/intro-overlay.md` before editing the intro or the lock
screen.** That file documents the isolation rules that keep the animation
robust against unrelated edits elsewhere in the site.
