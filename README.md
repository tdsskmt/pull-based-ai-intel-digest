# Pull-Based AI Intelligence Digest
<img width="1917" height="917" alt="image" src="https://github.com/user-attachments/assets/ca71953e-c63a-436f-8d3c-b38fc07257d4" />

**Languages: English · [日本語](./README.ja.md)**

> A stateful AI-news & startup-investment analyst that runs entirely inside one Claude Project — no backend, no database, no background jobs. Its memory lives in a single version-tracked state file, and a disciplined "state loop" carries what the system knows from one session to the next.

## Overview

Most people use a large language model as a *stateless* tool: you open a chat, paste a prompt, get an answer, and everything the model "learned" about your preferences evaporates when the tab closes. This project treats the same model as a *stateful, pull-based analyst*. Every working session begins by reading one canonical state file — the system's memory of which startups it has already analyzed, how the reader's preferences have shifted, and what it decided last time — and ends by rewriting that file in full, so the next session inherits the change.

Operationally it produces two things. A **Daily Digest** — the day's top five AI developments, each scored 0–100 on a fixed six-dimension rubric, with one slot always reserved for a technical "Breakthrough Spotlight" so a genuinely novel result is never buried by a weighted average. And **Startup Investment Reports** — structured teardowns (thesis, moat, risks, score, verdict) backed by a de-duplicating registry, so the same company is never re-introduced as if it were new. Everything is consumed through a single bilingual (EN / JA) dashboard: no email, no newsletter, no second source of truth.

This repository is published as a **reference example** for anyone building a serious workflow inside a Claude Project. The interesting part is not the news summaries — it is the architecture: how you give a stateless model durable memory, and how you keep a human in the loop as the one who commits state. The design decisions and the annotated version history below are the point.

## Key Design Decisions

Four choices shaped this system. A theme runs through the last two: **keep the score a clean, comparable measure of intrinsic importance; solve freshness and outlier-rescue one layer up, at selection.**

**Pull-based, not push**
- Every session reads the state file, reasons over it, rewrites it in full; a human commits the swap.
- Rejected: a scheduled push pipeline — infrastructure someone must own, un-forkable, hides its own memory.
- Cost: no real-time alerts; nothing happens unless I run the loop.

**Business Impact weighted highest (25)**
- I score for *how the economics get rewritten* — pricing, unit economics, cost structure — because that's the lens I care about most.
- I rank a company's *intrinsic advantages* — technology and business model — above market size, which is more a framing investors put around a story than a driver of the thing itself. Market sits at 15.
- Trade-off: a business-first weighting can underrate a pure research result before its commercial implication shows — closed by the Spotlight rule below.

**Recency at selection, not in the score**
- Freshness is enforced in acquisition (3-day window, auto-widening to 7 on quiet days), never as a decay term in the formula.
- Rejected: a recency term in the score — it would break cross-day comparability; a 72 should mean the same thing regardless of age.
- Exception: the Spotlight gets a looser window, since novel technical work often ripples out days later.

**Plain weighted average, rescued by a Spotlight slot**
- The score is a straight weighted average — no bonus terms.
- Rejected: a max/bonus term to lift breakthroughs — it muddies what the number *means*.
- Instead: one of five daily slots is reserved for the highest max(Technology, Novelty), scored with no inflation. Metric stays pure; the breakthrough still surfaces.

## Design Philosophy

The role is not "news summarizer" — it is *analyst*. Every item is pushed through six lenses: what happened, why it matters, who benefits, who is threatened, what happens next, and the investment/business implication. A summary that stops at "what happened" is treated as a failure.

Two doctrines sit underneath:

- **Primary sources first.** Company filings, papers, SEC documents, first-tier reporting — aggregators are supporting material, never the basis. Claims that can't be sourced don't ship; speculation is labeled as speculation.
- **Contrarian by default.** Consensus is the starting point to argue *against*, not to repeat. Every digest is expected to surface at least one "who is threatened" or overlooked implication the headline missed.

## Features

- **Daily Digest** — the day's Top 5 AI developments, each scored 0–100 on a fixed six-dimension rubric, bilingual (EN / JA).
- **Breakthrough Spotlight** — one of the five slots is always reserved for the highest-novelty technical result, so it is never buried by a weighted average.
- **Startup Investment Reports** — structured teardowns (thesis · moat · risks · score · verdict) against a defined investment rubric.
- **De-duplicating registry** — a company is matched by normalized domain before analysis, so it is never re-introduced as new.
- **Six-lens analysis** — every item is driven past summary all the way to its investment/business implication (the six lenses are set out under Design Philosophy).
- **Feedback-driven preferences** — chat feedback accumulates into the state file and reshapes future selection, scoring, and tone.
- **Single bilingual dashboard** — one artifact is the only consumption surface; no email, no diverging copies.

## Architecture: The State Loop

The system is three static layers plus one loop that moves state through time.

### Three layers

```
┌─────────────────────────────────────────────────────────────┐
│  1. INSTRUCTIONS  (Claude Project → Instructions field)      │
│     Role · doctrine · hard constraints · output contracts    │
│     The rules. Rarely changes.                               │
├─────────────────────────────────────────────────────────────┤
│  2. KNOWLEDGE BASE  (Claude Project → Knowledge)             │
│     intelligence-state.md   ← the only source of truth       │
│     evaluation-framework.md ← scoring rubrics (confirmed)    │
│     category-taxonomy.md    ← the 18 categories              │
│     The memory + the measuring stick.                        │
├─────────────────────────────────────────────────────────────┤
│  3. DASHBOARD  (single artifact, EN / JA)                    │
│     Daily digest · weekly view · startup registry           │
│     The only place output is consumed. No email, no copies.  │
└─────────────────────────────────────────────────────────────┘
```

**Why the split matters.** Instructions are the constitution — stable, and the model cannot silently rewrite them. The Knowledge Base is working memory — `intelligence-state.md` is the single source of truth, deliberately separated from `evaluation-framework.md` so that *what the system has learned* (preferences, registry) never contaminates *how it measures* (weights, thresholds). The Dashboard is the one surface where results live, so there is never a second, diverging copy to reconcile.

### The loop

```
   ┌──────────────────────────────────────────────────┐
   │                                                  │
   ▼                                                  │
 READ state ──► REASON ──► EMIT full replacement ──► HUMAN swaps file
 (start of      (score,     (Claude writes the        (canonical
  session)      select,     entire updated state,      commit —
                de-dupe)    not a diff)                the version
                                                       increments)
```

Three rules keep the loop honest:

- **Read first.** Every session opens by reading `intelligence-state.md`, so the registry (de-duplication), preferences, and last decisions are all in hand before any work begins.
- **Emit the whole file, never a diff.** When state changes, Claude outputs a complete, copy-paste-ready replacement — not "edit line 42." A model cannot reliably patch its own memory; it can reliably rewrite it.
- **The human commits.** Claude cannot write the canonical file. A person swaps it in and the version increments. That single manual step is what makes the memory trustworthy and the history auditable.

> This repository documents the **manual loop** as canonical, because it is the portable, environment-independent version anyone can fork. My own running instance later automated the file-swap step; that is an optimization layered on top, not a change to the model above.

## Operating Model & Setup

The files are just parts. The product is the loop you run on them. Read this section before the file list — it is what makes the repo runnable rather than a static specimen.

### The daily / weekly cycle

The system is *pull-based*: it does nothing on its own. You run it.

**Daily (one chat per day):**
1. Open a new chat in the Project. Claude reads `intelligence-state.md` first (registry, preferences, last decisions).
2. Claude selects and scores the day's Top 5 — four by Importance Score, one reserved for the Breakthrough Spotlight — and writes them into the dashboard's `DIGEST`, bilingual.
3. Claude reports completion in chat: one line, plus the day's single "AI Signal." The digest body is *not* re-pasted into chat — the dashboard is the only source of truth.
4. You read the result on the dashboard, and reply with feedback in chat ("more AI-security," "track this company," "that item was noise").
5. If feedback or new analysis changed anything, Claude emits the full replacement `intelligence-state.md`; you swap it in and the version increments.

**Weekly (one chat per week):**
- Not a re-ranking of the week's news. Claude analyzes *what changed* — major trends, emerging companies, technology shifts, investment signals, what to watch next — and updates the dashboard's weekly view.

> **Why one chat per unit of time.** Artifacts are scoped to a conversation, so a fresh chat per day (and per week) keeps each dashboard clean and each state swap unambiguous. Mixing days in one chat muddies which state is current.

### One-time setup

To stand up your own instance:

1. **Create a Claude Project.** Paste `PROJECT-INSTRUCTIONS.md` into the Project's *Instructions* field.
2. **Load the Knowledge Base.** Upload `intelligence-state.md`, `evaluation-framework.md`, and `category-taxonomy.md` to the Project's *Knowledge*.
3. **Seed the dashboard.** Open `ai-intelligence-dashboard.jsx` as an artifact; it renders the digest, weekly view, and startup registry.
4. **Run day one.** Start a chat and ask for the daily digest. Claude reads state, produces the Top 5, and updates the dashboard.
5. **Commit state.** When Claude emits an updated `intelligence-state.md`, replace the file in your Knowledge Base. That manual swap *is* the state loop — see the Architecture section above.

## How to Adapt / Fork

This system is built to be forked. The key is knowing which layers are *yours to change* and which carry the design integrity — change the wrong one and the loop quietly breaks.

### Safe to change — make it yours

- **Preferences** (`intelligence-state.md` §2). This is *my* learned taste — business-model lens, robotics-as-hardware, control-side signals. Yours will differ. Clear it and let your own feedback accumulate; that is the system working as intended.
- **Startup Registry** (`intelligence-state.md` §1). Sample analyses (Voliro, NEURA, Apptronik) are illustrative. Empty the table and start your own.
- **Dashboard theme** (`ai-intelligence-dashboard.jsx`). Colors, layout, language — the `DIGEST` / `STARTUPS` data contracts matter; the styling around them does not.
- **Domain focus.** Nothing here is AI-specific in principle. Repoint the role and categories at climate tech, biotech, defense — the state loop is domain-agnostic.

### Change deliberately — these encode the design

- **Scoring weights & Verdict thresholds** (`evaluation-framework.md`). Business Impact 25, Technology-first investment lens, the Verdict bands — these are *decisions*, not defaults. Re-weight them to match your own thesis, but do it as a considered choice, and record it (the framework is version-controlled for exactly this reason).
- **The 18 categories** (`category-taxonomy.md`). Coherent as a set. Add or merge deliberately, not ad hoc.

### Don't break — the load-bearing invariants

- **Read state first, emit the whole file, human commits.** The three loop rules (see Architecture) are what make the memory trustworthy. Automate the swap if you like, but keep the discipline.
- **One source of truth.** The dashboard's `DIGEST` is canonical; anything else is a mechanical render of it. Don't let a second copy diverge.
- **Separation of memory from measure.** Keep `intelligence-state.md` (what it learned) apart from `evaluation-framework.md` (how it measures). Collapsing them re-introduces the bias the split exists to prevent.

> Rule of thumb: the **Knowledge Base data** (preferences, registry, theme) is yours to fill; the **rubrics** are yours to re-decide *once*, on purpose; the **loop invariants** are the part you inherit intact.

## Files

| File | What it is |
|------|-----------|
| `PROJECT-INSTRUCTIONS.md` | The Project's Instructions field — role, doctrine, constraints, output contracts. The canonical **manual** state loop. |
| `intelligence-state.md` | The single source of truth — startup registry, preferences, watchlist, version log. Sample preferences and registry included as illustration. |
| `evaluation-framework.md` | Scoring rubrics — importance weights, investment rubric, Verdict bands, the Spotlight rule. |
| `category-taxonomy.md` | The 18 news categories. |
| `ai-intelligence-dashboard.jsx` | The consumption surface — daily digest, weekly view, startup registry. Bilingual. Embeds one representative recent day (the samples below are a separately curated subset, so the dashboard's day need not match them). |
| `samples/daily-digest-*.md` | Three curated real days, each led by a major event and each showing a different selection mechanism (7-day window expansion · Spotlight auto-satisfy · control-lens inclusion). Bilingual. See `samples/README.md`. |
| `samples/neura-teardown.md` | One full startup investment report — including the 73 → 67 re-score under the hardware lens. |
| `LICENSE` | MIT. |

## Version History

This project was not designed up front — it was corrected into shape. The state file's log records every change; below are the moments where I *found a flaw and fixed it*. That trail, more than any single feature, is the point.

| Version | What I observed | What I changed |
|---------|-----------------|----------------|
| **0.2** | The digest read as generic; my actual lens (business model, unit economics) wasn't reflected. | Added Preferred Analysis & Presentation Preferences — the feedback loop's first real input. |
| **0.5** | I'd scored a robotics startup (NEURA) as hardware, but my Business and Competition reads were too generous — leaning on platform/network-effect upside that wasn't yet proven by installed base. | Tightened the same lens to per-unit economics (margin, BOM, yield) and discounted unproven platform narrative: 73 → 67. Codified "discount platform stories until installed base proves them" as a standing preference. |
| **0.10** | On quiet news days the 3-day window left too few candidates, tempting weak filler. | Added a **Selection Preference layer**: 3-day acquisition window, auto-widening to 7 only when fewer than five good candidates exist — separate from the scoring formula. |
| **0.12** | The digest was capability-heavy; the *control* side of "capability vs. control" (safety, governance, org retreats) was getting dropped. | Added a **control lens** to candidate selection — a governance signal can now take a Top-5 slot even when a pure-tech item scores higher. |
| **0.15** | Manual state-swapping every session was friction, once the loop was proven. | Approved direct file-writes for my own instance — an *operational* change only. The public canonical loop stays manual (portable); this is layered on top. |

Two things this history is meant to show. First, the corrections cluster around one instinct: **when a scoring problem appeared, I fixed it at the selection layer rather than contaminating the score** (0.10, 0.12) — the same principle stated in Key Design Decisions. Second, I was willing to **re-score my own analysis down when the basis was too generous** (0.5), which is the discipline the whole state loop exists to enforce.

> The full, unedited log lives in `intelligence-state.md` §4 — including routine daily-digest runs omitted here. This table is the curated "design evolution" view.

## Provenance & Honesty Note

A few clarifications, stated plainly:

- **The scores are illustrative.** They reflect a defined rubric applied on a given date, not investment advice. Verdicts age; each carries its "as of" date.
- **The sources are real.** Every digest item cites a primary or first-tier source; nothing is fabricated.
- **The daily analysis is AI-generated.** The digests and teardowns are produced by Claude *operating under a framework I designed*. My contribution is the architecture, the rubrics, the feedback loop, and the judgment calls encoded in state — not hand-written equity research. That distinction is the honest and, I think, more interesting claim.

## License

Released under the **MIT License** — free to use, fork, and adapt, with attribution. See `LICENSE`.
