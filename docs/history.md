# History

Chronological record of what was done and why. Newest entries at the bottom.

---

## Why this repo exists

**It is the hub of a series about machines that compute with physics.** Two pages: a menu
that routes readers to a platform, and a comparison that tabulates what the platforms
report. It holds no physics of its own — every number in it was measured in a spoke
(`photonn`, `spinn`) and is referred to here by name.

---

## 2026-09-07 — the repo was created, and the design it holds was argued out first

The series was **redesigned before any of it was built**. A prior design document in
photonn's local planning (`plans/07`) specified a four-chapter chain across two repos,
with photonn narrated as its middle chapter and a nonlinearity survey as its ending. A
long interview replaced that with a different shape, and the differences are the reason
this repo looks the way it does.

**What changed, and why:**

- **Chain → hub and spoke.** Chapters are not read in sequence. A reader enters at the
  menu, picks a platform, and returns. Spokes link back here and nowhere else. The
  consequence is the one that mattered: **adding a platform touches this repo alone** —
  no chapter strip, no shared manifest, nothing to drift between repos.
- **Two repos → a hub plus one repo per platform.** photonn is the first spoke; `spinn`
  (spintronics) is the second.
- **The growth move was named explicitly:** photonn's original scope had saturated, so go
  *up* one level of abstraction — from "a photonic classifier" to "computing performed by
  physics" — and then back *down a different path*, into a second platform rather than a
  retelling of the first.
- **The comparison chapter pre-registers no argument.** A proposed thesis — that
  linearity is one universal architectural wall — was rejected as over-claiming from two
  instances. It is a reference chapter. If comparing produces something interesting, that
  gets explored then, and **no conclusion is written until at least two platforms have
  reported.**
- **Chapter 1 became light.** The dense material (energy, latency, bandwidth, state of
  the art) moves *into* the platform chapters. An interactive deployment-budget widget
  from the earlier design was dropped rather than carried forward into a page that no
  longer exists in that form.

### The comparison unit turned out to already exist

The hard problem was comparing tolerance across media: radians have no spintronic
counterpart, siemens no optical one. The resolution is **effective bits** —
`log₂(operating range / σ)` — and photonn is already publishing in it without having
planned to. Its docs express phase tolerance as λ/N, which *is* the operating-range
fraction, so `bits = log₂(N)`:

| photonn result | as published | effective bits |
|---|---|---|
| D²NN, 5 masks | 0.3 rad = λ/21 | 4.39 |
| D²NN, 56 masks | 0.15 rad = λ/42 | 5.39 |
| MZI mesh, 36 modes | 0.03 rad = λ/209 | 7.71 |

Corroborated independently by `docs/tolerance_d2nn.md:175`, which already states a
requirement of "≥ 4-bit phase control." **photonn's rows therefore need no new
simulation.**

The table carries energy and latency alongside precision, in one table rather than two —
a deliberate choice, with the scoreboard risk mitigated by having no totals, no ranking
and no winner column.

### What was actually committed

| | |
|---|---|
| `461c1f2` | `CLAUDE.md` with the settled design, `.gitignore` (`plans/` excluded from the first commit, since a planning note is easiest to leak before the rule exists), and photonn's commit-message template |
| `f92e52f` | `.gitattributes` copied from photonn, pinning line endings to LF |

Repo initialised on `main` to match photonn, remote added over HTTPS, and both commits
pushed. **No pages, no build system, no tests yet** — the build is a trimmed copy of
photonn's generator and is step one of the local plan.

### Deliberately not done

- No conclusion written for the comparison chapter, by design.
- photonn's own reframe is separate work, gated on its owner rewriting its `CLAUDE.md`
  first. Nothing here depends on it beyond one back-link.
