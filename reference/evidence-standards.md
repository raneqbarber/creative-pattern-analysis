# Evidence Standards

What each kind of finding in this system is allowed to claim, and the language that goes with it.

Every measurement skill in this collection cites this file. It exists because the fastest way to
destroy a creative intelligence system's credibility is to let an observational pattern and a
randomized test result sound equally authoritative in the same document.

**Author:** Raneq Barber.

---

## The hierarchy

A finding earns a tier. The tier decides what may be claimed and what happens next.

| Tier | Design | You may claim | Action |
|---|---|---|---|
| **1 · Verified** | Randomized controlled creative test | An estimated causal effect, in the tested population | Scale it. Make it a team rule |
| **2 · Clean matched contrast** | One focal trait changes between arms, strong time overlap, visual or copy held constant | Strong directional quasi-experimental evidence | Prioritise for a controlled test |
| **3 · Mixed matched contrast** | Several traits change together between arms | A promising *formula* or execution contrast — not an individual trait effect | Write a controlled brief that isolates one variable |
| **4 · Screened association** | Per-ad test, FDR-controlled, replicated across campaigns and months | A correlated pattern that survived screening | Generate hypotheses. Feed the roadmap |
| **5 · Descriptive** | Meets volume floor only | An observed pattern | Investigate. Do not operationalise |

Nothing in an observational system reaches Tier 1. That is not a weakness — it is the reason
Tier 2 findings are worth generating, because they tell you which of your scarce tests to spend.

---

## Language

### Do not use

**"Natural experiment."** A natural experiment requires variation created by something outside the
decision process — a policy change, an outage, an eligibility cutoff. A marketer choosing which
caption runs with which visual is a deliberate decision, and deliberate decisions carry the
confounders you are trying to escape.

Also avoid: *near-experimental proof · the trait caused · control group · treatment effect ·
the matched analysis confirms · proven.*

### Use

*Matched observational contrast · within-asset comparison · visual-held-constant evidence ·
copy-held-constant evidence · directional matched evidence · corroborated hypothesis ·
a localised performance difference · a test priority.*

### The difference in practice

Too strong:

> Soft CTA increases CTR by 18%.

Accurate, and no less useful:

> Across 9 matched sets holding the visual constant, variants carrying a soft CTA had a median
> within-set CTR difference of +18%, favourable in 7 of 9. Variants were not randomly assigned and
> 5 of 9 changed other copy traits alongside the CTA. Treat as directional corroboration and
> prioritise for a controlled test.

The second sentence is longer and stronger. It survives a skeptical reader; the first does not.

---

## What a matched contrast does and does not remove

**Holding the visual constant removes visual confounding.** By construction, the image is identical.
It removes nothing else.

Two captions on the same visual typically differ in claim strength, audience framing, offer,
length, CTA, emotional register, proof device, and landing-page congruence — and may also differ in
targeting, bid strategy, budget or optimisation state. The contrast is strong evidence that *the
full copy execution* performed differently. It is not evidence that one tagged trait did.

Hence the clean / mixed distinction. Every matched result must carry:

```
n_traits_changed          how many tagged traits differ between the arms
switch_class              clean (1) · near-clean (1 + minor) · mixed (2+)
time_overlap_months       arms must share at least one active month
match_quality             composite of overlap, spend similarity, duration similarity
```

A trait-level claim may only be made from **clean** contrasts. Mixed contrasts support
formula-level claims — which are often the more useful finding anyway, because creatives brief in
recipes.

---

## What a p-value does and does not control

It controls **one** failure mode: selecting a pattern by chance from noisy data.

It does not control confounding, tagging error, metric quality, delivery bias, or whether the
finding matters commercially. A screened association is a hypothesis that survived one specific
test, not a fact.

### Three standing requirements

**Report adjusted q-values per finding**, not a family cutoff. `p = 0.018 · BH q = 0.071` is stable
and legible; a moving threshold flips findings when the population changes slightly and nobody can
see why.

**Declare the test families before looking at results.** Correcting separately within singles,
pairs and triples is a defensible product decision — a family of 37,000 triples would otherwise
annihilate a family of 366 singles. But it means FDR is controlled *within each declared family*,
not across the union. Say that precisely; do not claim one global guarantee.

**Treat ad-level independence as an assumption, not a fact.** Ads within a campaign share
targeting, budget, bid strategy and optimisation state. The generalisation gate — 3+ segments, no
segment over 70%, 3+ campaigns, 3+ months — is a heuristic guard against this. A campaign-level
permutation or cluster bootstrap replaces the heuristic with a measurement, and any finding that
clears it should carry a `campaign-robust` tag.

---

## Small samples

With 5–15 observations, statistical machinery is weaker than the raw pattern. Report both.

- **Sign counts read better than p-values.** "7 of 9 sets improved" is more decision-useful to a
  practitioner than *p* = 0.07, and it is harder to misread.
- **Prefer exact tests or permutation** over normal approximations.
- **Give an interval**, even a descriptive bootstrap one, clearly labelled as descriptive when n is
  tiny.
- **Never read non-significant as no effect.** At this sample size it means *not distinguishable
  from noise yet*. Write it that way.
- **Set the minimum set count at or above what the test requires** to return a meaningful p-value.

---

## Combinations are not interactions

Comparing `A+B` against everything else answers: *is this recipe associated with better
performance than the rest of the segment?*

It does **not** establish that A and B work together. That requires comparing `A+B` against
`A`-only and against `B`-only. Until you have run those comparisons, synergy classification is
**formula classification** — a descriptive label — and must be written as one. Never as evidence
that two creative choices amplify or fight each other.

---

## Benchmark leakage

Every normalised metric depends on a benchmark. If an ad's own performance contributed to the
benchmark it is scored against, the score is circular and the whole inference layer is void.

Keep a permanent regression test asserting:

- No row's outcome contributes to its own benchmark.
- No current-period data influences that period's benchmark.
- Changing one ad's CTR cannot move the benchmark used in its own test.
- Reconstructed or backfilled rows cannot leak into the wrong benchmark period.

This is cheap, and it is the failure that silently makes a sophisticated system meaningless.

---

## The output rule

Every published finding carries its tier, its evidence, and its limits in the same object:

```
finding        soft caption CTA
tier           2 · clean matched contrast
evidence       9 matched sets · 7 favourable · median within-set +18%
               p = 0.021 · descriptive bootstrap −4% to +34%
switch class   clean in 5 of 9 sets; 4 also changed claim specificity
robustness     campaign-robust ✓ · 5 segments · 4 campaigns · 6 months
limits         not randomly assigned; no single-trait causal claim supported
next           isolated copy-variable test, CTA held as the only change
```

A finding presented without its tier is a finding presented as stronger than it is.

---

*MIT © Raneq Barber. Part of Growth AI Skills.*
