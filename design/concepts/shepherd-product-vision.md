# Shepherd -- Product Vision

**Date drafted:** 2026-05-08
**Status:** Phase 2 prototype, ARIN5301 Group 5 (HKUST)
**Team:** Mridul Kumar Sharma · Zhangxizi Qiu · Haoming Liu · Wing Chun Leung

---

## What Shepherd is

An always-on, context-aware autonomous AI assistant that lives inside the apps people already use. Built on top of an "OpenClaw" (always-on agent) base, integrated into a phone's day-to-day surfaces -- social feeds, health summaries, stocks, news.

Where most apps stop at **tracking**, Shepherd starts at **intervention**. It watches the data the user is already producing, spots patterns the user can't see for themselves, and triggers small actions the moment they matter.

## Core principle

A shepherd guides and protects its flock. Shepherd does the same for users -- filtering misinformation and surfacing timely guidance in everyday life.

## Example interventions

### Calorie tracking → personalized next step

The user logs meals, or a calorie app does it for them. Shepherd reads the count plus their personal preferences and proposes one concrete action -- "you're 200 over your lunch average; skip dessert tonight, or add a 20-minute walk." Tracking apps stop at the number. Shepherd takes the next step.

### Health data → insight, then action

Apple Watch (or similar) collects steps, sleep score, heart rate, recovery, mindful minutes. Most users never read them. Shepherd does. It spots the pattern -- "sleep score has dropped three nights running and step count is 30% below baseline" -- and wires the action into the user's calendar: "you have a 15-minute gap at 3:45pm, take a walk."

### Stocks + stress → automated reassurance + safety net

The user wakes up poorly and immediately checks the markets app. Shepherd has seen this pattern before. While the user is still in the kitchen, Shepherd has already run a fresh analysis on their watchlist and can answer in plain language: "still a buy / still a hold / set a stop at $X." If the user opts in, Shepherd auto-triggers a sell at a pre-set floor so the position is safe regardless of the user's own emotional state.

## Why this matters

The data is already being collected. Apps already track. The gap is between data and action -- and that gap is where Shepherd lives.

## Honest limits

Shepherd is a Phase 2 prototype, not a shipping product. Real deployment would need to confront privacy, consent, model latency, accuracy of automated trades, and the autonomy/agency tension Shepherd's own video paper (Phase 1, "The Good, the Bad, the Trade-off") examined.

## Reuse instructions

This document is the canonical source for any future Shepherd copy on the site or in coursework. When updating the page's `productShowcase.why` text or any expanded write-up, pull the phrasing from here so the public version stays aligned with the team's internal vocabulary.

Path: `design/concepts/shepherd-product-vision.md`.
