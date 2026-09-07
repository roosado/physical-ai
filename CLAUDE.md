# physical-ai — Project Context

The hub of a series about machines that compute with physics. It holds two pages: a
**menu** that routes readers to a platform, and a **comparison** that tabulates what the
platforms report.

**This repo holds no physics.** It measures nothing, trains nothing, and re-implements
nothing. Every number in it was produced by a platform repo and is referred to by name.

---

## Shape

```
        physical-ai  (this repo)
        ├── /          chapter 1 — the menu, short prose per platform
        └── /compare   the comparison — always last, always the terminus
              ▲
     ┌────────┴────────┐
     │                 │
  photonn            spinn            ← platform spokes, one repo each
  (light)            (spin)             each links back to the menu only
```

A reader enters at the menu, picks a platform, reads it, returns. **Spokes do not link to
each other and carry no chapter strip** — only a route back. That is the entire nav
contract, and it is why the series is extensible: **adding a platform touches this repo
and nothing else.** No shared manifest, no vendored chapter list, nothing to drift.

All repos serve from `roosado.github.io`, so crossing is same-origin, different path:
`/physical-ai/`, `/photonn/`, `/spinn/`. With matching chrome it reads as an internal
link.

The comparison chapter is a **moving terminus**: new platforms insert before it, never
after.

---

## What each part is for

| | |
|---|---|
| **Chapter 1** (`/`) | Routing first, short prose beneath. Just enough sourced data to make each door worth opening. |
| **Platform chapters** | The dense material — energy, latency, bandwidth, state of the art — lives **inside the spokes**, not here. |
| **The comparison** (`/compare`) | A reference chapter. Reports; does not argue. |

### The comparison argues nothing, by decision

No thesis is pre-registered. If comparing platforms produces something interesting, that
gets explored **then**. An expectation of a result is not set before the measurements
exist — the same discipline that marks an unsourced value `UNSOURCED` rather than filling
it with something plausible.

Concretely: **no conclusion is written until at least two platforms have reported.**
photonn has already been burnt here — its optics sweep overturned a ceiling claim the
site was actively making.

---

## Audience

A **general technical reader**. Not a photonics specialist: that reader is served by
photonn's own pages, which stay rigorous and must continue to read as a serious study to
someone arriving cold from a search result.

---

## The comparison table — the contract with every spoke

The schema is fixed; the table fills as platforms report.

### Effective bits

Comparing tolerance across media looks impossible — radians have no spintronic
counterpart, siemens no optical one. But any analog tolerance normalises against the
device's own operating range, and the log of that ratio is a bit depth:

```
effective bits = log2( operating range / sigma )
```

**photonn already reports in this unit without having planned to.** Its docs express
phase tolerance as λ/N, which *is* the operating-range fraction, so `bits = log2(N)`:

| photonn result | as published | effective bits |
|---|---|---|
| D²NN, 5 masks | 0.3 rad = λ/21 | 4.39 |
| D²NN, 56 masks | 0.15 rad = λ/42 | 5.39 |
| MZI mesh, 36 modes | 0.03 rad = λ/209 | 7.71 |

Corroborated independently: `tolerance_d2nn.md:175` already requires "≥ 4-bit phase
control." The unit is native to the project, not imposed on it. photonn's rows therefore
need no new simulation — but verify each against its docs, not against arithmetic done
from memory.

### Columns

Platform · what a weight physically is · what performs the sum · ideal accuracy on the
shared task · which source binds · **required precision (effective bits)** · delivered
precision (same unit, or `UNSOURCED`) · margin · energy per inference · latency per
inference.

### Rules

- **No margin without a source.** Where delivered is `UNSOURCED`, the margin column does
  not ship. photonn's mesh budget set this precedent: publish the measured tolerance
  *edges*, which are properties of the architecture and will not move, and omit the
  margin rather than invent one.
- **No ranking, no totals, no winner column.** Three axes reported independently, so a
  platform can win one and lose another. Sorting the rows by a column is a ranking.
- **The task is identical across platforms; the error sources are not.** photonn's rule —
  one task, reused so results stay comparable — extends across repos. Forcing a common
  error list would measure the taxonomy rather than the hardware.
- **No number appears in two repos.** State it where it was measured; refer to it by name
  everywhere else. A duplicated figure goes stale in one place — photonn has shipped
  stale figures twice, once for four months.

---

## Environment

- Laptop-only. Nothing here needs a GPU, because nothing here simulates.
- Python for the build: standard library plus Pillow. Keep the dependency list shorter
  than photonn's.
- Output is static, self-contained HTML committed to `site/`. No runtime fetches, no
  external hosts, no build step a reader needs.
- Widgets are hand-written ES modules. No framework.

---

## Architecture

Mirrors photonn deliberately, so an agent that has navigated one can navigate the other.

```
physical-ai/
├── apps/
│   ├── build_site.py     # trimmed from photonn: CSS, topbar, TOC, MathML, figures
│   ├── pages/            # page bodies
│   └── web/              # widget ES modules, if any
├── site/                 # built output, committed
├── docs/
└── tests/                # render tests over the built pages
```

**The hub takes a trimmed copy of photonn's `build_site.py`; spokes take the full one.**
A spoke trains a model and puts it in a browser, so it needs the bundle writer/reader and
the widget mount queue. The hub needs neither.

Trim: keep the CSS block, `topbar()`, `toc()`/`section_index()`, `mathml()` and the `MATH`
table, `encode_figure()`, the `PAGES` tuple, `resolve_links()`, `next_link()`. Drop
`error_mask_bundle()`, the weight-bundle writers, every d2nn/mesh exporter.

### MathML gotcha, carried over

Never set CSS `display` or `overflow` on a `<math>` element — it drops out of MathML
layout mode and stacks every child on its own line. The wrapper carries scroll and
centring.

---

## Scope boundaries

### Do not build

- Any re-implementation of a spoke's physics. The relay exists to prevent it.
- Any performance claim not traceable to a published measurement or to a spoke.
- A margin column against an `UNSOURCED` delivered value.
- Market projections, addressable-market figures, adoption curves.
- Benchmarks between named commercial products, or predictions about which wins.
- Any invented numerical value. Mark it `# UNSOURCED` and surface it.
- A chapter strip, a shared manifest, or cross-spoke nav. The topology exists precisely
  so none of that is needed.

### Sourcing rule for state-of-the-art material

The place this series is most likely to compromise photonn's standards. Commercial
claims in this field are overwhelmingly press releases rather than measurements, and a
TOPS/W figure from a company blog is not evidence.

**Describe what an approach *claims* and what has been *peer-reviewed*, in separate
sentences.** Never quote a marketing performance figure as fact. Where only a vendor
claim exists, say so in the prose.

---

## Open decisions

Do not assume an answer; ask.

1. **Whether `/compare` eventually wants a deployment-budget widget.** An earlier design
   put one on chapter 1 — sliders for model size, sensor rate, distance to the datacentre.
   Chapter 1 no longer exists in that form, so the widget was dropped rather than carried
   forward. Revisit only if the comparison page turns out to want it.
2. **Platform three.** The topology admits one; nothing else is decided.

### Resolved

1. **Series structure** → **hub and spoke.** Hub holds chapter 1 and the comparison;
   each platform gets its own repo.
2. **Comparison argument** → **none pre-registered.** Reference chapter; explore a finding
   only after it appears.
3. **Comparison unit** → **effective bits, plus energy and latency, in one table** with no
   ranking.
4. **Second platform** → **spintronics** (`spinn`): an MTJ / domain-wall crossbar as the
   built artifact, with a spin-torque oscillator model as complementary work that does
   **not** enter the table.
5. **Chapter 1's weight** → **light.** Routing first; the dense material moved into the
   platform chapters.
6. **Build** → hub trimmed, spokes full.
7. **photonn's changes** → separate work, after its own `CLAUDE.md` rewrite. Nothing here
   depends on them beyond one back-link.

---

## Working conventions

- **Prose carries the argument; stat blocks do not.** A number repeated in a stat strip
  the prose already covers is a number to delete.
- **A page that mostly points at other work should be cut.** Prefer fewer, denser pages
  that each land a claim.
- Citations inline, next to the value they support.
- Commit messages follow photonn's `.gitmessage`: prose explaining what the diff cannot
  say, numbers that moved stated explicitly, a per-file `Files:` breakdown.
- Widgets are tested through a DOM stub in pytest, not a real browser. A driven Chrome tab
  reports itself hidden and never fires `requestAnimationFrame`, so cadence is asserted in
  the suite instead.
- Scripted edits containing backslashes get written to a file and run by path, never piped
  through a shell heredoc.
- `plans/` is gitignored and never published.

---

## Plans

`plans/01-hub-and-comparison.md` — the first build. Read it before touching anything.
