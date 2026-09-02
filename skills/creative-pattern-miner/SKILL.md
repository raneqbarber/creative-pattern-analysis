---
name: creative-pattern-miner
title: Creative Pattern Miner
version: 1.0
author: Raneq Barber
framework: Performance Creative Intelligence (PCI)
type: computation
runtime: any — Claude Project · Custom GPT · Gemini Gem · API system prompt (search step needs code execution)
inputs: a tagged ad sheet with per-ad performance and benchmarks
outputs: candidate patterns with lift, volume, replication and the search size that produced them
companion: reference/metric-system.md · reference/evidence-standards.md
---

# Creative Pattern Miner

Searches a tagged portfolio for the creative choices that outperform the rest of their own segment —
every trait value, every pair, every trio — and hands the survivors to
`creative-pattern-validator` with the one number that decides whether any of them can be trusted:
**how many patterns were tested to produce them.**

This skill finds candidates. It does not decide what is real. Splitting those two jobs is
deliberate: a miner that also adjudicates its own output will always find something.

---

## 1 · Operating contract

- **Never emit a pattern without `patterns_tested_total`.** That number is the input the validator
  needs to correct the threshold. A candidate list without it cannot be validated and must not be
  presented as findings.
- **Never present mined output as findings.** Everything leaving this skill is a candidate. The
  words "insight", "finding" and "winner" do not appear in its output.
- **Compare within segment, never across.** A pattern's comparison group is the ads in the *same
  platform × ad objective × period* that do not carry it.
- **Recompute group rates from summed totals.** Never average per-ad ratios.
- **Declare the search families before running.** Singles, pairs, trios. Declared after the fact,
  they are decoration.
- **Never let the search silently drop rows.** Report ads excluded by the volume floor and why.

---

## 2 · When to run this

Run after `ad-creative-tagger` and `ad-performance-scorecard-builder`, on a complete period. Its
output goes straight to `creative-pattern-validator`, and nothing between the two should be read as
a result.

Do not run it on fewer than ~100 scored ads. Below that the search space dwarfs the evidence and
everything it returns will be cut downstream anyway.

---

## 3 · Required inputs and preflight

### Must have

| Input | Notes |
|---|---|
| Tagged ad rows | One row per ad, with trait values from `ad-creative-tagger` |
| Per-ad performance | Clicks, impressions, spend |
| Per-ad lift index | CAS, from the scorecard builder |
| `platform`, `ad_objective`, `period` | The segment keys |
| `campaign_id` | Required — the validator's cluster test depends on it |

### Improves the output

`asset_id` and `copy_id` (enables the matched analysis later), month per ad, and spend share.

### Preflight

1. **Tagging complete?** Any ad with missing trait values distorts every group it should have
   joined. *Halt* and name the count.
2. **Lift index present?** Raw CTR cannot be pooled across segments. *Halt if absent.*
3. **Campaign IDs present?** *Halt.* Without them the downstream cluster test cannot run, and a
   pattern that is really one campaign will look like a portfolio truth.
4. **Segment sizes.** Report ads per platform × objective × period. Segments under 20 ads are
   searched but flagged — findings there will rarely survive correction.
5. **Volume floor applied?** 200 impressions and 10 clicks per ad. Report how many ads it removes
   and what share of spend they represent.

---

## 4 · The procedure

### Step 1 — Aggregate to one row per ad per segment

One row per `(ad, platform, ad_objective, period)`. Keep only ads clearing the volume floor.

An ad with 40 impressions tells you nothing — one lucky click swings its entire score. On a real
run this took 787 ads down to 446, covering 90% of spend. Losing 43% of rows and 10% of spend is
the correct trade.

### Step 2 — Enumerate the search space

Within each segment, enumerate:

- **Singles** — every trait value
- **Pairs** — every co-occurring pair of trait values
- **Trios** — every co-occurring trio

Apply **apriori pruning**: a pair cannot have more ads than its rarest component, so any combination
whose rarest half is already below the minimum ad count is skipped without evaluation. This is what
makes the search finish.

**Count everything enumerated, including what pruning skipped after evaluation began.** That total
is the single most important number this skill produces.

### Step 3 — Score each pattern against its own segment

For each pattern, build two groups inside the same segment: ads carrying it, and ads not carrying
it. Compute for both, from summed totals:

```
group CTR = Σ clicks ÷ Σ impressions
group CAS = group CTR ÷ benchmark CTR
lift      = (pattern CAS ÷ peer CAS) − 1
```

Because both halves sit under the same benchmark, the benchmark cancels — which is why lift is the
defensible basis for briefing creative. A benchmark revision cannot move it.

The comparison is never "these ads did well." It is **"these ads did better than their peers."**

### Step 4 — Compute the evidence attributes

Per pattern: ad count, distinct campaigns, distinct months, distinct assets, distinct copy, spend
share, and concentration — the share of its ads sitting in its largest campaign.

Concentration is the early warning for the cluster problem. A pattern at 80% concentration is a
campaign wearing a trait's name, and it will fail the validator's permutation test.

### Step 5 — Run the significance test

Mann-Whitney U on the **per-ad** lift values, pattern group against the rest of the segment.
Rank-based because ad performance is skewed and groups are small and unbalanced.

Emit the raw p-value. **Do not correct it here** — correction depends on the total search size and
belongs with the validator, which knows the declared families.

### Step 6 — Prune redundancy

Drop any pattern whose ad set is **identical** to a simpler pattern's. A pair covering exactly the
same ads as one of its components is that component wearing a longer name, and keeping both inflates
the family size, which makes the correction harsher for everything else.

### Step 7 — Emit candidates with the search size

Every candidate carries its evidence attributes and its raw p-value. The header carries
`patterns_tested_total` per family.

Say plainly, in the output: these are candidates, not findings, and the next step is validation.

---

## 5 · Computation rules

- **Group rates from summed totals. Never average per-ad ratios.** Averaging weights a $50 test ad
  equally with a $50,000 evergreen. Within one quarter the two methods usually agree; across a
  longer window they diverge by up to 26%.
- **Comparison group is same-segment, without the trait.** Never the whole account.
- **Apriori pruning before evaluation.**
- **Volume floor before enumeration**, so excluded ads never enter a group.
- **Count every pattern enumerated**, including those later pruned.
- **Emit raw p-values only.** Correction is the validator's job.

---

## 6 · Output contract

```
PATTERN CANDIDATES · 2026-Q2 · not findings — for validation only

SEARCH
  ads scored           787 → 446 after volume floor (90% of spend retained)
  segments             6 (platform × ad objective)
  families declared    singles · pairs · trios
  patterns_tested      singles 366 · pairs 6,587 · trios 37,634 · TOTAL 44,587
  redundancy pruned    1,204 patterns with ad sets identical to a simpler pattern

CANDIDATES — sorted by lift, uncorrected
  pattern                       ads camps mons conc   lift    p (raw)
  cd_03 = soft                   62     9    6  31%   +28%   0.0014
  cs_05 = customer_testimonial   42     7    5  38%   +14%   0.0000
  cs_01 = question              130     5    6  22%   +14%   0.0001
  vd_09 = none                   22     6    4  44%   −41%   0.0130
  urgency_framing                31     2    3  74%   +21%   0.0110   ⚠ concentrated

⚠ CONCENTRATION WARNING
  urgency_framing draws 74% of its ads from two campaigns. Expect it to fail
  the cluster-robustness test downstream.

NEXT STEP — REQUIRED
  These are candidates. 44,587 patterns were tested, so roughly 2,200 would
  clear p<0.05 by chance alone. Run creative-pattern-validator before any of
  this reaches a brief.
```

---

## 7 · Failure and degraded states

| Situation | What to do |
|---|---|
| Incomplete tagging | Halt. Name the count and the traits affected. |
| No campaign IDs | Halt. The cluster test downstream cannot run without them. |
| Fewer than 100 scored ads | Run, but lead the output with the warning that almost nothing will survive correction. |
| A segment under 20 ads | Search it, flag it, and report its size beside every candidate from it. |
| No lift index | Halt. Raw CTR cannot be pooled across segments. |
| Search too large to finish | Report how far it got and which family is incomplete. Never emit a partial `patterns_tested_total` as if complete — the correction would be wrong in the dangerous direction. |
| No code execution | Emit the procedure and the segment sizes. State that no search was run. |

---

## 8 · Worked example

A portfolio of 446 scored ads across six segments yields 44,587 enumerated patterns and 201 with
raw p < 0.05.

The naive read is 201 findings. The correct read is that **roughly 2,200 patterns would clear p <
0.05 with no real effect behind them**, so 201 is *fewer* than chance alone would produce across the
full space — which tells you immediately that most of the search space is genuinely null and that
only tightly-corrected survivors are worth anything.

That is why this skill does not adjudicate. It emits 201 candidates and the number 44,587, and the
validator decides. Downstream, 16 survive all three gates.

The `urgency_framing` row is the instructive one: strong lift, respectable p-value, and 74%
concentration in two campaigns. Everything about it looks like a finding except the attribute that
matters most.

---

## 9 · Self-check

1. Is `patterns_tested_total` reported per family and in total?
2. Were families declared before the search ran?
3. Were group rates recomputed from totals rather than averaged?
4. Is every comparison within-segment?
5. Are campaign, month and concentration attributes attached to every candidate?
6. Were redundant patterns pruned, with the count reported?
7. Does the output call these candidates and require validation before use?

---

*MIT licensed.*
