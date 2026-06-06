# Blog series spec — "Building nubecita" (working doc, not published)

> Planning doc for an 8-part blog series on franciscovelazquez.com (Chirpy).
> Lives in `_drafts/` so Jekyll never publishes it. Not a post.

## Goal & audience
- **Goal:** tell the honest story of a veteran Android dev finally shipping a solo app, with AI tooling as the enabler — credible enough that engineers learn real things, human enough that a broad dev/maker audience stays hooked.
- **Audience:** Android/Kotlin engineers primarily; indie devs/makers secondarily.
- **Voice:** hybrid — narrative spine + real technical meat (code, architecture, commits, benchmarks). Approachable, first-person, specific.
- **Length:** varies by part (noted per post).
- **Sourcing:** every technical claim grounded in the real repos (`~/code/nubecita`, `~/code/atproto-kotlin`); cite real files/commits. No invented numbers.

## Narrative arc
Veteran (Android dev **since 2011**), father, full-time engineer — never shipped solo because starting + maintaining an app alone was too costly. AI tools (Claude Code + Android CLI skills) collapsed that cost. The twist: the journey started not with the app but with an **open-source SDK** (atproto-kotlin) meant for Maven Central; the app grew out of dogfooding it. Then: building the app on solid foundations, the AI workflow that made it sustainable, monetization, release automation, and honest lessons (what worked, what didn't).

## Publishing details
- **Location:** `_posts/` in this repo (Chirpy). Draft in `_drafts/` first.
- **Category:** `Nubecita` (series). **Tags** (pick per post): `android`, `kotlin`, `jetpack-compose`, `ai-tools`, `claude-code`, `bluesky`, `atproto`, `kotlin-multiplatform`, `ci-cd`, `fastlane`.
- **Filename:** `YYYY-MM-DD-<slug>.md` (dates set at publish; series order via a leading "Part N" in titles + cross-links).
- **Front matter (Chirpy):** `title`, `date`, `categories: [Nubecita]`, `tags: [...]`, optional `image: {path, alt}` cover, `toc: true`.
- **Cross-linking:** each post opens with a "This is Part N of a series" note linking the index/prev/next. Consider a pinned intro/index post later.
- **Cover images:** use real nubecita shots (see Visual Assets below); could run them through the repo's frameit/screenshot pipeline for clean device mockups. Fallback: Unsplash like existing posts.
- **Cadence:** draft one post at a time; user reviews each before the next.

## ⚠️ Facts to verify before publishing
1. ✅ **APK size — CONFIRMED 12 MB** (Play Store "About this app", nubecita v1.193.3, Pixel 10 Pro XL download size; Requires Android 9+). Use **"12 MB"**, not the old "<10 MB" guess.
2. ✅ **Official Bluesky client — CONFIRMED 148 MB** (Play Store, Bluesky v1.122.0 by Bluesky PBLLC, same Pixel 10 Pro XL download size; Android 7+). Use **"148 MB"**, not "~100 MB". **Headline stat: 12 MB vs 148 MB ≈ 12× smaller**, same device, same store metric (fairest comparison). Bluesky = React Native; nubecita = native Compose.
3. **Years of experience** — "since 2011" (~15 yrs). Use "since 2011".
4. Versions drift (Renovate): quote `gradle/libs.versions.toml` at draft time, not code comments.

---

## Part 1 — The Inspiration *(narrative, ~900–1,200 words)*
**Spine:** why now. Android dev since 2011; dad + full-time engineer; the real blocker was never skill, it was the time cost of starting *and maintaining* solo. Claude Code + Android CLI skills let me prototype fast and keep momentum in small windows. Then: what nubecita is.
**Highlights:**
- What it is: fast, lightweight, **native** Bluesky client; package `net.kikin.nubecita`.
- Positioning vs official app: **12 MB vs 148 MB — ~12× smaller** (same device/store metric); native Compose vs React Native. (Confirmed; assets `p1-appsize-nubecita.webp` / `p1-appsize-bluesky.webp`.)
- Stack teaser (full detail later): 100% Compose, **Nav 3**, **Material 3 Expressive**, Hilt, Room; atproto-kotlin (my own SDK).
- The 120 Hz scroll goal as the north star (hard requirement; verified via `dumpsys gfxinfo`).
**Takeaway:** AI tooling didn't write the app for me; it removed the activation energy that had stopped me for a decade.

## Part 2 — The Foundation: template + CI/CD *(medium, ~1,000–1,300 words)*
**Spine:** before features, build a base you can maintain alone. The `kikin81/android-template` lineage → nubecita's first commits.
**Highlights (real):**
- `ec4ed1a3` Initial commit = template scaffold (71 files; placeholder `com.example.myapp`); `67d4028c` renamed to nubecita + tightened CI.
- AGP 9, Kotlin DSL, version catalog, `includeBuild("build-logic")` (Now-in-Android pattern), 8 convention plugins.
- CI from day one: Spotless + ktlint 1.4.1, pre-commit (commitlint, gitleaks, actionlint, EOF), `lint/test/build/screenshot` jobs, **semantic-release** (`ci(release): x.y.z`), Renovate (open-turo presets).
- Conventional Commits + commitizen.
**Takeaway:** templating the boring-but-critical scaffolding is what makes solo maintenance survivable.

## Part 3 — atproto-kotlin: the SDK that came first *(deep, ~1,500–2,000 words)*
**Spine:** the original goal was an open-source KMP SDK for Maven Central — not the app. Existing Kotlin atproto clients lagged on lexicons/OAuth; I built one that didn't.
**Highlights (real):**
- Genesis `24f72a47` (2026-04-14): runtime/generator/models scaffold *before* the app; Android sample came second (dogfooding).
- The **code generator** (the heart): parses `@atproto/lex` (pinned 0.1.3) at build time; 7 passes Parse→Resolve→Context-tag→Name→Verify(INV-1..4)→Plan→Emit (KotlinPoet). Show a golden example (`StrongRef`, `PostEmbedUnion` open-union-as-interface w/ `$type` dispatch + `Unknown` fallback).
- **OAuth/DPoP** module — from-scratch OAuth 2.0 for public clients (PKCE S256, DPoP EC P-256, PAR, transparent refresh) with **no JWT lib**; the differentiator. Cite hard-won bugfixes (`1f2c6f4e` DPoP-nonce).
- **Maven Central** via vanniktech + Sonatype Central Portal; namespace rename `io.github.kikin81.atproto` for group ownership (`0b2d12b0`); current **9.x**.
- **Dokka V2 → `docs/` → GitHub Pages** (`kikin81.github.io/atproto-kotlin/api/`) — *same Pages trick as this blog* (nice callback).
- `skills/` dir: SKILL.md files for *downstream agents consuming the lib*.
**Takeaway:** building the dependency first, in the open, set the quality bar for everything above it.

## Part 4 — Building nubecita *(deep, ~1,500–1,800 words)*
**Spine:** with the SDK + template ready, build the app proper — multimodule, separation of concerns.
**Highlights (real):**
- **50 modules**: `:app` thin shell, 20 `:core:*`, `:data:models`, `:designsystem`, `:benchmark`, `:feature:*` with **api/impl split** (`:api` = `NavKey` only; `:impl` = screens/VMs/Hilt `@IntoSet EntryProviderInstaller`). Show `Feed.kt` (`data object Feed : NavKey`).
- **Two-shell Nav 3** (`@OuterShell`/`@MainShell` qualifiers); `NavDisplay`.
- **MVI** story: `MviViewModel<S,E,F>` (StateFlow + `Channel` effects). Original `633d2467` (#15) shipped an `Async<T>` wrapper — **deleted in the same PR** to "pivot to flat UI-ready state and effect-based errors"; now a hard non-goal (no `Async`/`Result` at VM→UI boundary). Foundation = 4 files (~60 LOC). Later moved to `:core:common` (`78eba4f6`).
- Skeleton arc (commits): scaffold → rename → OpenSpec → **Hilt** (`434eeeae`) → **beads** (`6cffe631`) → **MVI** (`633d2467`) → atproto SDK (`80fea86b`).
- **Performance/120 Hz:** `:benchmark` macrobenchmarks — `FeedScrollBenchmark` (fling, `FrameTimingMetric`, p95 = regression target), `StartupBenchmark`, `BaselineProfileGenerator` (committed profiles). CI `macrobench.yaml` → gh-pages trend, `fail-on-alert 150%`.
- **Lightweight, honestly:** R8 + resource shrink, Material Symbols subsetting (14.9 MB → ~340 KB), Downloadable Fonts, per-PR APK-size deltas. **Final download size: 12 MB** (vs Bluesky's 148 MB — confirmed via Play Store).
**Takeaway:** boundaries (api/impl, SDK-agnostic cores) are what let one person hold a 50-module app in their head — and what let agents work safely in it.

## Part 5 — The AI Toolkit *(deep, ~1,400–1,800 words)*
**Spine:** the workflow that made solo dev sustainable in small time windows.
**Highlights (real):**
- **Claude Code** as the driver; `CLAUDE.md` (28 KB) as the architecture bible; `AGENTS.md` for shell/agent rules.
- **Compose-expert skill** used as a **review gate**: `bd-workflow` detects `@Composable` diffs → invokes compose-expert Review Mode (recomposition/stability/modifier/M3-motion/list-keys checklist). [link the skill]
- **OpenSpec** (spec-driven): `specs/<capability>/` canonical + `changes/<name>/` proposals; pre-commit `openspec validate`; slash commands `opsx/*`; 28 archived changes.
- **brainstorming** workflow: captured sessions (`.superpowers/brainstorm/...`) feeding real design docs ("Brainstormed with Claude").
- **beads (`bd`)**: issue tracker backed by a **Dolt** versioned SQL DB; decoupled from git (parallel PRs kept conflicting on JSONL) — Dolt remote is source of truth; slug IDs (`nubecita-crmi`) referenced in commits/specs. [link steveyegge/beads]
**Takeaway:** the leverage isn't "AI writes code" — it's specs + task tracking + review gates that keep a solo project coherent.

## Part 6 — MCPs & Billing *(medium, ~1,000–1,300 words)*
**Spine:** adding a Pro in-app purchase fast, via the RevenueCat MCP.
**Highlights (real):**
- **RevenueCat MCP** to set up + verify dashboard state (project/entitlement/product IDs; $1.99/mo, $19.99/yr) — `docs/pro-launch-checklist.md` "verified via RevenueCat MCP"; `design.md` notes the MCP enabled.
- **`:core:billing` boundary** (design D1–D3): RevenueCat SDK (10.8.0) never leaks past the module; consumers see `EntitlementRepository`/`BillingRepository` + `:data:models`. Anonymous Play identity, **never the Bluesky DID**. Inert on blank key (keyless/bench builds).
- Custom Compose paywall (`:feature:paywall`), not RC's `purchases-ui`.
**Takeaway:** MCPs turn "integrate a billing dashboard" into a conversation; the engineering discipline (clean boundary) is still yours to own.

## Part 7 — Release Automation *(deep, ~1,400–1,800 words)*
**Spine:** ship continuously, solo, without leaking secrets.
**Highlights (real):**
- **`bench` flavor** (`app/build.gradle.kts`): in-process fakes (FakeSession/FakePrefs), network-free, deterministic — powers macrobench, baseline profiles, and **marketing screenshots**. Named `bench` to avoid `benchmark` plugin collision.
- **fastlane**: `screenshots` lane (screengrab + Hilt test runner + frameit), `internal` (Play internal track), `screenshots_tablet`, `upload_screenshots`.
- **WIF migration** (great deep-dive): SA-JSON → **Workload Identity Federation** for *both* Firebase App Distribution and Play. Commits `35925966` (switch FAD to WIF, `id-token: write`), `e4d08d55` (FAD → firebase-tools CLI; FAD Gradle plugin rejected `external_account`), `33ea1ae1` (pin firebase-tools 15.18.0). The subtle trick: **leave `json_key_file` unset** so fastlane `supply` falls through to `Google::Auth.get_application_default` and accepts `external_account`. Auth is `google-github-actions/auth@v3`; WIF IDs are `release`-environment secrets.
**Takeaway:** removing long-lived secrets (WIF) is the kind of "boring infra" that's now approachable solo, with AI help reading the error trails.

## Part 8 — Lessons in AI-Assisted Solo Dev *(reflective, ~1,200–1,500 words)*
**Spine:** what actually worked, and what didn't.
**Highlights:**
- Skills + workflows as reusable leverage; specs+beads keeping context across short sessions.
- **Claude mobile app** to schedule/kick off tasks from my phone (dad-time-friendly).
- **What didn't work:** GitHub agents — lacked the environment/tooling (Android SDK/JDK) to be useful for this stack. **Antigravity** — token limits too restrictive.
- Honest reflection: AI as activation-energy remover, not author; the human owns architecture, taste, and the boundaries.
- What's next for nubecita.
**Takeaway:** the meta-lesson — tooling that lowers activation energy is what finally turned 15 years of "someday" into a shipped app.

---

## Visual assets — `~/Desktop/nubecita/` (133 PNGs, Feb 14 – May 27 2026)
Analyzed a chronological sample. The folder is a **mix of polished app UI and behind-the-scenes dev captures** — both useful, for different posts. Needs curation (strays + near-dupes) and **web optimization** (many are 2–5 MB / 2560×1600; resize + compress before committing to `assets/img/`).

**App UI (product shots):**
- OAuth consent screen (`Screenshot 2026-05-16…`) — Bluesky authorize via `nubecita.app/oauth/client-metadata.json`. → Parts 1, 3.
- Profile, **light** theme (`nubecita_01.png`) — clean, hero-worthy. → Part 1 / cover.
- Profile, **dark** theme (`Screenshot_20260521_004026`). → Parts 1, 4 (dark mode).
- **Tablet two-pane list-detail**, dark (`Screenshot_20260519_193825`) — strong adaptive-layout shot. → Part 4 (adaptive). Cover-worthy.
- Feed/timeline phone shots throughout (Feed/Search/Chats/You nav).

**Icon:** launcher montage (`Screenshot_20260521_143732`) shows the **Nubecita cloud icon** ("nubecita" = little cloud ☁️) beside Bluesky/X/etc. → Parts 1, 8.
> ⚠️ User mentioned "evolution of the icon" — I confirmed the current cloud icon but did NOT verify multiple earlier icon versions in the sample. Need a focused pass (or user pointers) to assemble an icon-evolution montage.

**Behind-the-scenes (great for the AI/automation posts — more valuable there than generic UI):**
- **beads (`bd`) terminal** creating `nubecita-kf6k` (`Screenshot 2026-05-22 8.29.58 AM`). → Part 5.
- **Chrome DevTools** debugging the OAuth redirect (`Screenshot 2026-05-25 8.48.03 AM`). → Part 3.
- **Play Console** auto-translating 429 strings via Gemini (`Screenshot 2026-05-27 12.15.21 AM`). → Parts 7/8 (i18n/release).

**Skip (strays):** system Contacts app (`Screenshot_20260520_220056`); an insurance "Incident Details" UI (`…2026-02-14…`, predates the project).

**Recommendation:** do a full curation pass → pick ~15–20 "best of" mapped to posts → resize/compress → commit under `assets/img/nubecita/`. I sampled ~11 of 133; a complete pass will surface more dev-process gems and any icon-version history.

### ✅ Curation done (2026-06-05) — committed to `assets/img/nubecita/`
Full pass over all 133 PNGs (5 parallel reviewers). Picked **19**, resized to ≤1400px, converted to webp (≈1.1 MB total). Source → committed-slug → post:

| Committed file (`assets/img/nubecita/`) | Source PNG (`~/Desktop/nubecita/`) | Post |
|---|---|---|
| `p1-profile-light.webp` | nubecita_01.png | P1 **(cover candidate)** |
| `p1-appsize-nubecita.webp` | Play Store "About this app" (12 MB) | P1/P4 **(size stat)** |
| `p1-appsize-bluesky.webp` | Play Store "About this app" (148 MB) | P1/P4 **(size stat)** |
| `p1-oauth-consent.webp` | Screenshot 2026-05-16 at 2.08.15 PM | P1/P3 |
| `p1-feed-dark.webp` | Screenshot 2026-05-22 at 5.51.35 PM | P1/P4 |
| `p1-profile-dark.webp` | Screenshot 2026-05-23 at 7.01.11 PM | P1 |
| `p1-feed-light.webp` | Screenshot 2026-05-24 at 8.39.30 AM | P1 |
| `p1-notifications-dark.webp` | Screenshot 2026-05-25 at 11.01.36 PM | P1 |
| `p3-devtools-oauth-redirect.webp` | Screenshot 2026-05-23 at 3.00.51 PM | P3 |
| `p4-tablet-two-pane-dark.webp` | Screenshot_20260519_193825.png | P4 **(adaptive hero)** |
| `p4-macrobenchmark-metrics.webp` | Screenshot 2026-05-22 at 10.35.15 AM | P4 (120 Hz/perf) |
| `p4-skeleton-loading-diff.webp` | Screenshot 2026-05-24 at 8.37.26 AM | P4 |
| `p4-settings-dark.webp` | Screenshot 2026-05-23 at 11.20.03 AM | P4 |
| `p5-beads-board-dark.webp` | Screenshot 2026-05-22 at 2.52.06 PM | P5 (beads) |
| `p5-openspec-compose-expert.webp` | Screenshot 2026-05-26 at 8.15.34 PM | P5 (OpenSpec) |
| `p6-entitlement-pro.webp` | revenuecat/products.png | P6 **(entitlement + products hero)** |
| `p6-anonymous-customer.webp` | revenuecat/userid.png | P6 **(anon ID + sandbox sub hero)** |
| `p6-mcp-commands.webp` | revenuecat/mcp-details.png | P6 (MCP NL commands) |
| `p6-mcp-overview.webp` | revenuecat/mcp.png | P6 (MCP intro) |
| `p7-semantic-release-notes.webp` | Screenshot 2026-05-21 at 8.01.25 PM | P7 |
| `p7-play-console-autotranslate.webp` | Screenshot 2026-05-27 at 12.15.21 AM | P7/P8 (i18n) |
| `p7-play-console-release.webp` | Screenshot 2026-05-27 at 12.09.24 AM | P7 |
| `p8-icon-01-cloud-bow.webp` | Screenshot 2026-05-21 at 1.50.05 PM | P8 (icon montage) |
| `p8-icon-02-blue-square.webp` | Screenshot 2026-05-21 at 2.47.08 PM | P8 (icon montage) |
| `p8-icon-03-home-screen.webp` | Screenshot_20260521_160202.png | P1/P8 (final icon) |

**Findings from the pass:**
- ✅ **Icon evolution EXISTS** — a clear May-21 progression was captured: cloud mascot → + pink bow → blue-square / color & shape variants → final icon on the home screen. Three picked above; more variants available (`…2.23.11 PM` Figma, `…4.03.50 PM` mono-green, `…5.19.51/5.20.39 PM` shape grid, `…10.45.54 AM` iterations) if we want a richer before→after montage. **This answers the icon-evolution open question — no extra screenshot pass needed.**
- ✅ **Part 6 (MCPs & Billing) — mostly resolved (2026-06-05).** User added `~/Desktop/nubecita/revenuecat/` (4 real shots), committed above: the `pro` entitlement with `nubecita_pro:annual`/`:monthly` attached, an anonymous-customer profile (`$RCA…` App User Id + a USD 1.99 sandbox sub — proves "never the Bluesky DID"), and the RevenueCat MCP docs (NL command examples). Privacy-clean (sandbox data, no API keys, anon ID truncated). **Still missing:** (a) the in-app `:feature:paywall` Compose screen, and (b) the **$19.99/yr annual price** shown explicitly (only the $1.99/mo appears, on the customer shot) — a grab of Product catalog → Products would nail both prices side-by-side.
  - _Earlier note (resolved): the original "RevenueCat" shots in the main folder were actually GitHub Actions billing, not the IAP dashboard._
- 🗑️ **Strays correctly dropped:** system Contacts app (the aurora-background "profile" shots), macOS About, MacBook purchase page, Social-Security graph, Fly.io/DNS/WAF infra, insurance UI.
- Many near-dupe clusters (e.g. five 7:01 PM profile frames, 9:02 PM Play-listing trio) — kept one each.

## Decisions (resolved 2026-06-05)
- ✅ **Cover images:** real nubecita shots run through the repo's **frameit/screengrab** pipeline for clean device mockups. (Unsplash only as last-resort fallback.)
- ✅ **Intro post:** YES — add a short **pinned index/intro post** (Part 0) that introduces the series and links all 8 parts.
- ✅ **Category & tags:** keep as specified — category `Nubecita`; tags `android`, `kotlin`, `jetpack-compose`, `ai-tools`, `claude-code`, `bluesky`, `atproto`, `kotlin-multiplatform`, `ci-cd`, `fastlane`.
- ✅ **Screenshot curation:** YES — do a full pass now: pick ~15–20 best from the 133 PNGs in `~/Desktop/nubecita/`, resize/compress for web, commit under `assets/img/nubecita/`.

## Still open — need user input
- **APK size** numbers (Part 1 & 4) and the official-client size comparison — confirm real figures (build/bundle analyzer) before stating any number.
- **Icon evolution** — point me to the earlier icon versions (or confirm I should hunt for them in the screenshot pass) so we can build a real before→after montage.
