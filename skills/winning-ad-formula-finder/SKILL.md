---
name: winning-ad-formula-finder
title: Winning Ad Formula Finder
version: 1.0
author: Raneq Barber
framework: Performance Creative Intelligence (PCI)
type: evaluation
runtime: any — Claude Project · Custom GPT · Gemini Gem · API system prompt
inputs: validated trait patterns with their per-ad values
outputs: named formulas, typed by briefing team, with a classification and a brief
companion: reference/metric-system.md · reference/evidence-standards.md
---

# Winning Ad Formula Finder

Finds the **combinations** that outperform, and names them so a creative team can brief from them.

Creatives do not think "I'll use a soft CTA." They think "bold copy, no product screenshot, soft
CTA." That is a formula, and it is the level at which a brief actually gets written. Single-trait
findings are the foundation underneath; formulas are the deliverable.

The hard part is not finding combinations. It is being honest about what a combination proves.

---

## 1 · Operating contract

- **A formula is a classification, not a proof of interaction.** Comparing `A+B` against everything
  else cannot establish that A and B work together. Never write "synergy" as a demonstrated effect —
  see §4 Step 3.
- **Never publish a formula that failed the validator.** Formulas inherit the tier and robustness
  tag of the pattern behind them.
- **Never brief a formula without its trait DNA.** "Make more like this" is not briefable. The
  specific trait values are the deliverable.
- **Never publish a formula under its trait codes.** `cd_06=bold + vd_02=no` never enters team
  vocabulary. Coin a real name first — see Step 5.
- **Report the ad count on every formula.** A recipe resting on five ads is a hypothesis wearing a
  name.
- **Never claim a formula generalizes beyond the segments it was observed in.**

---

## 2 · When to run this

Run after `creative-pattern-validator` on the surviving pair and trio patterns. Feeds
`creative-brief-builder` and `performance-guidelines-builder`.

Do not run it on unvalidated candidates. A named formula is far stickier than a trait code — once a
team adopts "the Confessional Hook," it survives long after the evidence for it has been retracted.
Naming an unvalidated pattern is how a false finding becomes doctrine.

---

## 3 · Required inputs and preflight

### Must have

| Input | Notes |
|---|---|
| Validated patterns | Pairs and trios that cleared the validator, with tier and robustness tag |
| Per-ad values | For each component trait individually, and for the combination |
| Component single-trait results | The individual lift of each component, needed for classification |
| `ads_in_pattern` | Minimum 5 |

### Improves the output

Spend share, segment breakdown, matched-pair results for the same traits, and the concentration
figure per formula.

### Preflight

1. **Validated?** *Halt on anything that did not clear the validator.* Naming precedes adoption.
2. **Component results present?** Classification compares the combination against its strongest
   single component. Without those, classification cannot run — report the formula unclassified
   rather than guessing.
3. **Minimum 5 ads.** Below that, report as a hypothesis, do not name it.
4. **Tier and robustness carried through?** They must appear on the formula, not be dropped in
   translation from the pattern.

---

## 4 · The procedure

### Step 1 — Assemble the recipe

Each formula is 2–5 trait values appearing together in at least 5 ads. Record the exact trait DNA —
this is what gets briefed.

### Step 2 — Compute group performance

From summed totals across the ads carrying the full combination: group lift, spend, ad count,
distinct campaigns and months, and concentration.

### Step 3 — Classify against the strongest single component

Compare the combination's lift to the lift of its **strongest individual component**:

| Class | Condition | What it means for briefing |
|---|---|---|
| **Amplifying** | Beats its strongest component by >15% | Brief the whole stack — the combination is the insight |
| **Additive** | Within ±15% | Traits work independently. Only worth naming when spend justifies it |
| **Suppressing** | Falls >15% below its strongest component | Do not combine these. Often the most valuable finding |

**Two things must be said plainly in the output.**

First, why the baseline is the strongest component and not the sum of components. The additive
baseline systematically over-predicts, because traits are correlated — ads with trait A very often
carry trait B, so summing double-counts the shared ads. Measured against an additive baseline on a
real portfolio, 54% of pairs fell below the line and "antagonistic" became the default verdict,
which is an artefact and not a finding. The strongest-component comparison also happens to be the
question a creative actually asks: *does adding this second thing help?*

Second, and more important: **this classification is descriptive.** Comparing `A+B` against the rest
of the segment answers "is this recipe associated with better performance than the segment
remainder?" It does **not** establish interaction. Establishing that requires comparing:

- `A+B` against `A`-only
- `A+B` against `B`-only

Until those comparisons exist, "amplifying" is a label, not a demonstrated effect. Where the data
supports the component comparisons, run them and say so — that upgrades the claim materially. Where
it does not, the output must say the classification is descriptive.

### Step 4 — Type by briefing team

| Type | Contains | Briefs |
|---|---|---|
| **Copy direction** | All copy traits | Copywriters |
| **Visual direction** | All visual traits | Designers |
| **Creative direction** | Both | The whole team — highest leverage, because it describes a complete recipe |

### Step 5 — Name it

Every formula ships as `[rename]` in an editable field. A human coins a real name before publishing.

This is not decoration. *"The Confessional Hook"* enters team vocabulary the first time it is used
and gets asked for by name six months later. `cd_06=bold + vd_02=no` never will. Once coined, the
name is locked for the life of the formula — a renamed formula loses its accumulated meaning.

### Step 6 — Write the brief

Per formula: the trait DNA, the classification with its baseline stated, the evidence tier, the
segments observed in, the ad count, and what a creative should actually make. Plus, explicitly, what
would need testing to move it from tier 3 or 4 to tier 1.

---

## 5 · Computation rules

- **Minimum 5 ads per formula.** No exceptions.
- **Baseline is the strongest single component**, never the additive sum. Report the additive figure
  in its own column so the choice is auditable.
- **±15% bands** for classification.
- **Group performance from summed totals.**
- **Tier and robustness tag carry through** from the validated pattern.
- **Never publish under trait codes.**

---

## 6 · Output contract

```
FORMULAS · 2026-Q2 · from 47 validated pair and trio patterns
classification baseline: strongest single component (additive shown for audit)
⚠ classification is DESCRIPTIVE — see limits below

[rename] · creative direction · tier 2 · campaign-robust
  DNA        cs_01 = question
             + cd_03 = soft
             + vd_05 = illustrated_diagram
  evidence   21 ads · 6 campaigns · 5 months · 27% concentration
  lift       +21%
  strongest component  cs_01 alone  +14%
  additive baseline    +41%  (over-predicts — components co-occur in 11 of 21 ads)
  class      AMPLIFYING — beats strongest component by 100%
  briefs     whole team
  make       an illustrated explainer opening on a reader question, closing soft

[rename] · copy direction · tier 4 · campaign-robust
  DNA        cs_02 = outcome_led + cd_04 = 76_150
  evidence   34 ads · 5 campaigns · 6 months
  lift       +16%  ·  strongest component +15%
  class      ADDITIVE — traits work independently
  note       named only because it carries 22% of spend

[rename] · visual direction · tier 4
  DNA        vd_06 = heavy + vd_09 = none
  evidence   14 ads · 4 campaigns
  lift       −31%  ·  strongest component −12%
  class      SUPPRESSING — worse together than either alone
  make       nothing. This is a guardrail: never ship heavy on-image text
             with no image CTA.

LIMITS — read before briefing
  These classifications compare each combination against the rest of its
  segment. That does not establish that the traits interact. Establishing
  interaction requires comparing A+B against A-only and against B-only.
  Where those comparisons were possible they are shown; where they were not,
  treat the class as a descriptive label and the formula as a test priority.

  component comparisons available: 2 of 3 formulas
    question + soft:  vs question-only +14pp · vs soft-only +21pp  → interaction supported
    heavy + none:     insufficient ads carrying only one component  → descriptive only
```

---

## 7 · Failure and degraded states

| Situation | What to do |
|---|---|
| Unvalidated pattern | Refuse. Explain that naming precedes adoption and adoption outlives retraction. |
| Under 5 ads | Report as a hypothesis with its DNA. Do not name, do not classify. |
| Component results missing | Report the formula unclassified. Never guess a classification. |
| Component comparisons impossible | Say so per formula. Mark the class descriptive. |
| Every formula is suppressing | Check the baseline first — this is the signature of an additive baseline having crept back in. |
| High concentration | Report it beside the class. A formula from two campaigns is those campaigns. |
| Formula spans one segment only | Say so. It is guidance for that segment, not the portfolio. |

---

## 8 · Worked example

`cs_01 = question + cd_03 = soft + vd_05 = illustrated_diagram` — 21 ads, +21% lift.

The strongest single component, `question` alone, returns +14%. The combination doubles it, so it
classifies **amplifying**.

The additive baseline would have predicted +41%, making the same formula look like a *failure*
against its own components. It is not. The components co-occur in 11 of the 21 ads, so summing their
individual lifts double-counts the same ads. This is why the baseline is the strongest component and
why the additive figure is still printed — so anyone can check the choice rather than trust it.

Then the honest part. Because the data contains enough ads carrying `question` without `soft` and
`soft` without `question`, the component comparisons could be run: +14pp against question-only and
+21pp against soft-only. Both positive, so interaction is genuinely supported here, and the output
says so.

For the `heavy + none` formula there were not enough single-component ads, so its classification
stays **descriptive** — a label indicating where to look, not evidence that the two traits fight
each other. The recommendation is still useful as a guardrail; it just is not a claim about
mechanism.

---

## 9 · Self-check

1. Is every formula backed by a validated pattern with its tier carried through?
2. Is the baseline the strongest single component, with the additive figure shown for audit?
3. Does the output state that classification is descriptive unless component comparisons were run?
4. Where component comparisons were possible, were they run and reported?
5. Does every formula carry its trait DNA, ad count, campaigns, months and concentration?
6. Is every formula shipped as `[rename]` rather than under trait codes?
7. Is each formula typed by the team that briefs it?

---

*MIT licensed.*
