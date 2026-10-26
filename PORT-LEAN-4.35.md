# Lean 4.35 port of the Poisson random measure modules (branch `lean-4.35-poisson-subset`)

This branch is a fork of [slink/LeanLevy](https://github.com/slink/LeanLevy) by Shmuel Link,
based on upstream commit `7e73fd9b23ad52956ec2756a815889a783131ce4` (Lean 4.32, Mathlib
`1f165a5`). All code is upstream's and remains under the upstream MIT license; the file headers
and `LICENSE` are unchanged. The port was made for a research formalization that depends on
LeanLevy, and is offered back to upstream.

## Scope

Ported to Lean `v4.35.0` and Mathlib `56a005855ea17e0b0958f73ec801ae140b0a1679` and built by
default (the 12 `roots` in `lakefile.toml`):

- `Fourier/MeasureFourier`, `Probability/Characteristic` (unchanged)
- `Levy/LevyMeasure`, `Levy/CompensatedIntegral`
- `Probability/IndependenceGrouping`, `Probability/Poisson`
- `RandomMeasure/PoissonPointFamily`, `PoissonRandomMeasure`, `PoissonCompensated`,
  `PoissonFiltration`, `TimeSpacePoisson`, `PoissonMartingale`

The other upstream modules are kept unchanged in the tree but are **not ported and not built** on
this branch (`LeanLevy.lean` and `Main.lean`, which import them, are not built either).

## Changes

- Porting fixes only: renamed or re-signatured Mathlib lemmas (measurable maps, finset sums,
  `ite` lemmas, `Set.ofPred` API), probability-measure instances, `MemLp`/`eLpNorm` API, deprecated
  imports. No public statement changed.
- One private lemma, `TimeSpacePoisson.eLpNorm_two_sq`, now assumes `AEStronglyMeasurable`:
  current Mathlib's `eLpNorm` is `∞` on functions that are not a.e. measurable. Its two callers
  discharge it.
- `lakefile.toml`: Mathlib required under the standard name `mathlib` (upstream: `mathlib4`) at the
  pin above; the `repl` dependency and the `leanlevy` executable are dropped on this branch;
  `autoImplicit = false`.

## Checks (October 26, 2026)

- `lake build`: success (3257 jobs).
- Axiom audit over every declaration of the 12 modules (`Lean.collectAxioms`): 446 declarations,
  only `propext`, `Classical.choice`, `Quot.sound`.
- No `sorry`, `admit` or `native_decide`; no deprecation warnings.
