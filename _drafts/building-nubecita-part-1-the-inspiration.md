---
title: "Building nubecita, Part 1: The Inspiration"
description: Why a 15-year Android veteran finally shipped a solo app — and how AI tooling removed the activation energy that had stopped me for a decade.
date: 2026-06-05 12:00:00 -0700
categories: [Nubecita]
tags: [android, kotlin, jetpack-compose, ai-tools, claude-code, bluesky]
image:
  path: /assets/img/nubecita/p1-profile-light.webp
  alt: "Nubecita — a fast, native Bluesky client, profile screen in light theme"
toc: true
---

> **Building nubecita** is a series about shipping a solo Android app — a native [Bluesky](https://bsky.app) client — with AI tooling as the enabler. This is **Part 1: The Inspiration**. The next part covers the foundation: the template and CI/CD that make solo maintenance survivable.
{: .prompt-info }

## The blocker was never skill

I've been writing Android apps since 2011. Fifteen years of `Activity` lifecycles, support-library migrations, the long march from Eclipse to Android Studio, AsyncTask to coroutines, XML to Compose. I do this full-time, and I'm a dad. Both of those facts matter for this story.

In all that time I never shipped a solo app of my own. Not because I didn't have ideas — every engineer has a graveyard of them — but because the cost was never just _writing_ the app. It was **starting and then maintaining** one, alone, in the margins of a full life. The dependency bumps. The CI that rots. The release pipeline you set up once and forget how to run. The crash that surfaces three weeks after you've paged out the entire mental model. For a solo dev with a few hours a week, that ongoing tax is the thing that quietly kills projects. It killed all of mine.

What changed wasn't my skill. It was the **activation energy**. Tools like [Claude Code](https://www.anthropic.com/claude-code) and the Android CLI skills let me prototype fast _and_ — more importantly — keep momentum across small, broken-up windows of time. Twenty minutes between meetings. An hour after the kid's asleep. The cost of re-entering a project dropped far enough that "someday" finally became a commit history.

This series is the honest story of that. Not "AI wrote my app" — it didn't — but how the right tooling removed the friction that had stopped me for a decade. Let me start with what came out the other end.

## What nubecita is

**nubecita** — Spanish for "little cloud" ☁️ — is a fast, lightweight, **fully native** Bluesky client for Android. Package name `net.kikin.nubecita`. It talks to the real Bluesky network: you sign in with your actual account through Bluesky's own OAuth consent screen.

![Bluesky's OAuth consent screen, authorizing nubecita via nubecita.app](/assets/img/nubecita/p1-oauth-consent.webp){: width="360" .shadow }
_Real account, real network — nubecita authenticates through Bluesky's OAuth, identifying itself via `nubecita.app`._

"Native" is the whole positioning. The official Bluesky app is a fine app, but it's built with React Native and it carries the weight you'd expect. nubecita is 100% [Jetpack Compose](https://developer.android.com/jetpack/compose). Here's the difference, measured the only way that's truly fair — the **Play Store download size, on the same device**:

| ![Nubecita: 12 MB download](/assets/img/nubecita/p1-appsize-nubecita.webp) | ![Bluesky official: 148 MB download](/assets/img/nubecita/p1-appsize-bluesky.webp) |
|:--:|:--:|
| **nubecita — 12 MB** | **Bluesky (official) — 148 MB** |

_Same device, same store metric: **12 MB vs 148 MB — roughly 12× smaller.**_

That gap isn't an accident, and it isn't free; getting there is a recurring theme in this series (R8, resource shrinking, font subsetting, per-PR size budgets — that's a later post). But it's the kind of thing one person _can_ own when the tooling lets you keep your hands on every layer.

## The stack, at a glance

I'll go deep on the architecture later — there's a lot of it, more than fifty Gradle modules' worth — but here's the teaser for the engineers already squinting at the screenshots:

- **100% Jetpack Compose** — no XML layouts, a single `Activity`.
- **Navigation 3** — the new `NavDisplay` / `NavKey` world, not the old fragment-and-graph setup.
- **Material 3 Expressive** — the latest M3 motion and components, via `MaterialExpressiveTheme`.
- **Hilt** for dependency injection, **Room** for local persistence.
- **[atproto-kotlin](https://github.com/kikin81/atproto-kotlin)** — the AT Protocol SDK that nubecita is built on... which I also wrote. That's not a footnote; it's where this whole journey actually _started_, and it's the subject of Part 3.

![Nubecita feed in dark theme on a Pixel 10 Pro XL](/assets/img/nubecita/p1-feed-dark.webp){: width="360" .shadow }
_The timeline in dark theme. Native Compose, top to bottom._

## The north star: 120 Hz

If I had to name one hard requirement that shaped everything, it's this: **the feed has to scroll at 120 Hz.** Not "feel smooth" — actually hold the frame budget on a high-refresh display, with no dropped frames under a fast fling.

That's not a vibe I check by eyeballing it. It's a number I hold myself to with macrobenchmarks: a dedicated `:benchmark` module runs a `FeedScrollBenchmark` that flings the timeline and records `FrameTimingMetric`, and I watch the p95 frame time as a regression target in CI. A high-velocity flick is exactly what stresses a 120 Hz pipeline, so that's what the benchmark reproduces. If a change pushes p95 over budget, the build tells me before I ever ship it.

Holding a bar like that solo, across months of small sessions, is precisely the kind of discipline that used to be too expensive for me to sustain. Now the benchmark _is_ the discipline — it remembers the standard so I don't have to.

## What's next

That's the inspiration and the shape of the thing. The rest of this series is the how, honestly told:

- **Part 2 — The Foundation:** the template and CI/CD that make solo maintenance survivable.
- **Part 3 — atproto-kotlin:** the open-source SDK that came _before_ the app.
- **Part 4 — Building nubecita:** fifty-some modules, and how one person holds them in their head.
- **Part 5 — The AI Toolkit:** the workflow that made short sessions add up.
- **Part 6 — MCPs & Billing**, **Part 7 — Release Automation**, and **Part 8 — Lessons** round it out.

The meta-lesson, stated up front so you can hold me to it: the leverage here isn't a machine writing code. It's tooling that lowers the activation energy of starting and maintaining — enough to finally turn fifteen years of "someday" into a shipped app.

> Got questions about any of this, or building something solo yourself? The comments are open — I'd love to hear what's stopping _you_.
{: .prompt-tip }
