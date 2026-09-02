# The Creative Measurement System

Every formula, threshold and decision rule the measurement skills in this collection share.
Load this alongside any skill whose frontmatter names it as a companion.

**Author:** Raneq Barber. **Framework:** Performance Creative Intelligence.

---

## Why this exists

Traditional platform metrics — CTR, CPC, ROAS, CPA — measure outcomes that happen *downstream* of
the creative, in environments the creative did not control. A strong creative on a bad landing page
looks like a failure. A mediocre creative in a hyper-qualified retargeting audience looks like a
winner.

This system answers a narrower question: **what did the creative do?** Measured in the environment
it actually ran in, against a fair baseline.

## Metrics vs. indicators — the distinction everything rests on

**Metrics measure what happened.** Quantitative outputs derived from data. CAS, CDV, CFC.

**Indicators interpret risk and route action.** Composite assessments derived from patterns across
metrics. CFR, CCR, DSC, CLS.

Conflating the two produces metric soup — a dashboard of numbers with no way to know which matter,
in what order, or why. Metrics feed indicators. Indicators route action. Action generates data.

## Four layers that never break

Each layer activates independently on the data available. The system is always at least partially
functional and never fails to produce output.

| Layer | Contains | Requires |
|---|---|---|
| **1 · Signal + Efficiency** | CAS, CDV, CSR, CCE, CFC | Impressions, clicks, spend, a benchmark. Nothing else |
| **2 · Predictability + Intent** | SSS, IQP | Windowed data Layer 1 already produces |
| **2.5 · Lifecycle** | CBR, CLS, POR | Accumulated history. Activates automatically |
| **3 · Business Impact** | mConv, mCPA, mRM, PCS · CVLS, CPALS, ROASL | Modeled by default; overlays actuals when available |

Portfolio Health (CFR, CCR, DSC) and Portfolio Diversity (CDS, CDI, PWDS) run continuously across
all four, monitoring the system rather than the asset.

## The benchmark is the fairness mechanism

Every normalized metric requires a benchmark representing expected performance for that platform,
ad objective, offer type and vertical. Comparing a LinkedIn B2B awareness campaign to a Meta DTC
retargeting campaign on raw CTR is not a comparison — it is noise.

Prefer account-specific segmented benchmarks. Use `custom-performance-benchmark-builder` to build
them. Benchmark quality is tracked by PCS and propagates into every modeled output's confidence.

---

## Layer 1 — Signal and efficiency

Always available. Derived entirely from impressions, clicks and spend.

| Metric | Full name | Formula | Answers |
|---|---|---|---|
| **CAS** *(also CAI)* | Creative Action Strength | `CTR ÷ Benchmark CTR` | Is this creative earning action above or below expectation for its environment? >1.0 outperforming |
| **CDV** | Creative Dollar Value | `Clicks × Benchmark CPC` | What is the market value of the demand generated? Not revenue — a valuation of demand |
| **CSR** | Creative Spend Return | `CDV ÷ Spend` | How much demand value came back per $1 spent? >1.0 = generated more value than it cost |
| **CCE** | Creative Cost Efficiency | `(Benchmark CPC − Actual CPC) ÷ Benchmark CPC` | How much cheaper are these clicks than benchmark? Positive = reducing cost of demand |
| **CFC** | Creative Financial Contribution | `(Benchmark CPC − Actual CPC) × Clicks` | The actual dollars saved or wasted. The number that enters a budget conversation |

**How they read together:** CAS is the signal. CDV is the scope. CSR is whether the spend was worth
it. CCE is the unit cost story. CFC is the financial verdict.

---

## Layer 2 — Predictability and intent

### SSS · Signal Stability Score (0–100)

Splits spend into early (first 30%) and expanded (last 30%) windows. Computes CAS, CSR, CCE in each.

```
V_CAS = |ΔCAS|   V_CSR = |ΔCSR|   V_CCE = |ΔCCE|
Penalty = 0.50·V_CAS + 0.35·V_CSR + 0.15·V_CCE
SSS = 100 × (1 − min(1, Penalty))
```

| SSS | Reading |
|---|---|
| ≥ 80 | Stable scaler — performance holds across spend levels |
| 60–79 | Moderate — watch before scaling aggressively |
| < 60 | Fragile — do not scale without investigation |

Returns **"Not enough data"** below minimum exposure. Never estimate it.

### IQP · Intent Quality Proxy (0–100)

```
CAS_norm = min(CAS, 2) ÷ 2
CCE_norm = min(max(CCE, 0), 0.50) ÷ 0.50
SSS_norm = SSS ÷ 100
Curiosity Risk = 0.45·CAS_norm + 0.35·CCE_norm + 0.20·(1 − SSS_norm)
IQP = 100 × (1 − Curiosity Risk)
```

| IQP | Reading |
|---|---|
| ≥ 70 | Likely high-intent clicks |
| 40–69 | Mixed — monitor landing behavior |
| < 40 | Clickbait risk. Do not scale until message-landing match improves |

The clickbait signature is high CAS + high CCE + unstable performance: clicks from people
interested enough to click, not interested enough in what follows.

---

## Layer 2.5 — Lifecycle

### CBR · Creative Burn Rate

Three spend windows (25/50/25). `Peak_CAS` = max across windows.

```
CAS_Decline      = Peak_CAS − CAS_C   (0 if CAS_C ≥ Peak_CAS)
Spend_after_Peak = total spend − spend through peak window
CBR = (CAS_Decline ÷ Spend_after_Peak) × 1000
```

| CBR | Label | Production implication |
|---|---|---|
| 0–5 | Slow burn | Long useful life, scale steadily |
| 5–15 | Moderate | Normal variant cadence |
| 15–30 | Fast burn | Variants into production **now** |
| 30+ | Spike | Do not scale. Mine for hook and visual DNA, rebuild with substance |

### CLS · Creative Lifecycle Stage

| Stage | Conditions | Action attached |
|---|---|---|
| **Emerging** | CAS rising, SSS < 60, CFR not triggered | Monitor |
| **Peak** | CAS ≥ 1.0, SSS ≥ 70, ΔCAS neutral/positive, CFR clear, CBR slow–moderate | **Scale and build variants now** |
| **Plateau** | CAS above benchmark, ΔCAS −0.05 to −0.15, SSS 50–70, CBR moderate–fast, CFR approaching | Urgent variant development, freeze spend increases |
| **Declining** | CAS below benchmark OR ΔCAS < −0.15, CFR triggered, SSS < 50 | Rotate out, mine for pattern DNA |
| **Spike** | Peaked in window A, CBR very high, declining on low total spend | Do not scale, rebuild the concept |

**Variant production begins at Peak, not Plateau.** By Plateau the window has already closed. CBR
supplies the velocity so production is calibrated to the time actually available.

### POR · Pattern Overlap Risk

Fires when **(A)** two or more creatives in the same pattern show correlated simultaneous CAS
decline, **AND (B)** each had healthy prior CAS (Peak > 1.0), **AND (C)** no portfolio-wide CSR drop
explains both — which rules out a media or auction cause.

Asset-level fatigue is CFR. Pattern-level saturation is POR. POR + high CCR on the same pattern is a
portfolio emergency.

---

## Layer 3 — Business impact

### Modeled

| Metric | Formula | Notes |
|---|---|---|
| **mConv** | `Clicks × Benchmark CVR` | Foundation for all modeled impact |
| **mCPA** | `Spend ÷ mConv` | Hero metric for B2B |
| **mRM** | `(mConv × Value per Conversion) ÷ Spend` | Hero metric for DTC |

### PCS · Profitability Confidence Score (0–100)

```
DSS = 0.60·min(1, Clicks ÷ 300) + 0.40·min(1, Spend ÷ 500)
Stability = SSS ÷ 100
BFS = 1.00 account-specific segmented · 0.80 account-specific unsegmented
      0.60 industry benchmark · 0.40 unknown fallback
PCS = 100 × (0.45·DSS + 0.35·Stability + 0.20·BFS)
```

| PCS | Use |
|---|---|
| 0–40 | Directional only — no budget decisions |
| 40–70 | Moderate — creative decisions and planning, validate before major shifts |
| 70–100 | High — safe for scaling decisions and stakeholder reporting |

**A modeled CPA without its confidence band is not actionable.** Always show PCS beside it.

### Actual, when conversion data exists

| Metric | Formula |
|---|---|
| **CVLS** | `Actual CVR ÷ Benchmark CVR` |
| **CPALS** | `Benchmark CPA ÷ Actual CPA` |
| **ROASL** | `Actual ROAS ÷ Benchmark ROAS` |

Actuals take precedence. Modeled steps back to supporting context — never shown as competing.

---

## Portfolio health indicators

### CFR · Creative Fatigue Risk

Triggers on **(A+B+C)** or **(A+C+D)**:

- **A** — CAS < 0.90
- **B** — meaningful exposure met (see thresholds below)
- **C** — CAS below portfolio peer median for same platform and funnel by ≥ 15%
- **D** — pattern saturation: high CCR on the pattern + multiple assets declining together

| Platform | Meaningful exposure |
|---|---|
| Meta | ≥ 10,000 impressions **or** ≥ $250 spend |
| LinkedIn | ≥ 5,000 impressions **or** ≥ $500 spend |
| TikTok | ≥ 20,000 impressions **or** ≥ $300 spend |
| YouTube / Google Display | ≥ 15,000 impressions **or** ≥ $200 spend |

These are **v1 defaults, not laws.** They update as account-specific data accumulates — a brand with
enough history should replace them with its own observed point of stability. Treat them as a
starting position for an account with no history of its own.

**CFR does not fire below these thresholds.** An underperformer below threshold is *Emerging*, not
fatigued. Emerging underperformance may resolve as delivery optimizes; fatigued underperformance
will not. Never increase spend on a High-CFR asset regardless of current CAS.

### CCR · Creative Concentration Risk

```
CCR_value = Top N pattern CDV ÷ Total portfolio CDV
CCR_spend = Top N pattern spend ÷ Total portfolio spend
```

| CCR | Tier | Mandate |
|---|---|---|
| ≤ 40% | Low | Optimize freely |
| 40–60% | Moderate | One exploratory pattern test per week |
| ≥ 60% | High | Structurally fragile. 20% of output and testing capacity to new patterns |

### DSC · Domain Signal Confidence

Two independent reads, five states — this is what routes the intervention to the right team.

| State | Meaning |
|---|---|
| Creative strong + media strong | Healthy |
| Creative weak + media strong | Creative revision needed |
| Creative strong + media weak | Media optimization needed — do not brief new creative |
| Creative weak + media weak | Likely offer, funnel or strategy — neither creative nor media |
| **Signal unclear** | Portfolio < 30 days, most SSS < 60, most PCS < 50, or spend too low |

State 5 matters most. A system honest about its own uncertainty is trusted; one that diagnoses
prematurely destroys trust the first time it is wrong.

---

## Portfolio diversity

**Eight dimensions:** format · concept angle · hook type · social proof · persona signal ·
CTA construct · visual aesthetic · ad objective.

| Metric | Formula | Threshold |
|---|---|---|
| **CDS** | Shannon diversity across all 8, weighted by strategic importance, 0–100 | 80+ excellent · 65–79 healthy · 50–64 moderate · <50 concentrated/critical |
| **CDI** | `(Unique categories ÷ Max possible) × Evenness factor`, per dimension | **< 0.40 on any dimension triggers a roadmap prompt** |
| **PWDS** | `CDS × (Active diverse assets ÷ Total diverse) × (Budget to diverse ÷ Total budget)` | Target ≥ 55 |

PWDS catches paper diversity — a library with range that carries no spend.

---

## Winner taxonomy

| Classification | Trigger | Next |
|---|---|---|
| **Creative Winner** | CAS strong + CCE/CSR/CFC positive. Business impact unconfirmed | Replicate the pattern. Three variants on the same trait DNA. Do not scale hard yet |
| **Business Winner** | mCPA under target OR mRM above threshold OR strong actuals | Scale at current levels. SSS ≥ 80 → promote to Scale Winner. SSS < 80 → Validation Queue |
| **Scale Winner** | Business Winner **and** SSS ≥ 80 | Increase budget. Sister variants on exact trait DNA. Highest-confidence lever in the system |
| **Lead** | CAS high **but** PCS < 50 or SSS < 60 | +50% controlled increase to gather stability. Reclassify after |
| **Clickbait Risk** | CAS high + CCE high + **IQP < 40** | Do not scale. Fix message-landing match. Reassess IQP first |

## Action queues

Every asset sits in exactly one. Nothing falls through without a directive.

| Queue | From | Action |
|---|---|---|
| **Scale** | Scale Winners | Increase budget. Sister variants on exact trait DNA |
| **Replication** | Creative Winners | Three variants on the pattern — enough executional variation to test whether the pattern is robust |
| **Validation** | Business Winners with SSS < 70 or PCS < 50 | Controlled +50% to validate stability before committing |
| **Optimization** | Clickbait Risk, or persistently low IQP | Tighten message-landing match. No spend increase until IQP > 50 |
| **Refresh** | CFR Medium/High, or POR triggered | Rotate out. New executions if the pattern is alive; a new pattern if POR is active. Brief the trait DNA forward before retiring |

---

## Traditional platform metrics — when to use which

Traditional metrics are not replaced by this system. They are contextualised by it. They are the
**inputs**; the metrics above are the **outputs**. The goal is not to remove CTR and CPC from a
practitioner's vocabulary — it is to make sure a familiar number is read correctly rather than in
isolation.

| Platform metric | Use it for | Use the system metric when |
|---|---|---|
| **CTR** | The raw rate, for platform reporting | **CAS** — when you need to know whether that CTR is *good for the environment it ran in*. A 0.4% CTR on LinkedIn and on Meta are not the same result |
| **CPC** | Absolute cost, for budget planning | **CCE** for whether that cost is efficiency or waste; **CFC** for what it is worth in total dollars. CPC is the cost, CFC is the verdict |
| **CPM** | Diagnosing delivery and auction | **Never for creative quality.** High CPM with healthy CAS is a media problem — expensive delivery, working creative. Low CPM with low CAS is both. CPM feeds DSC's media signal |
| **Impressions** | Reach and exposure | Determines whether CFR and SSS exposure thresholds are met. Below them, an underperformer is *Emerging*, not fatigued |
| **Reach / frequency** | Saturation context | Frequency above ~5–7 with declining CAS makes an asset a CFR candidate before impression thresholds are reached. Frequency is the mechanism; CFR is the diagnosis |
| **Spend** | Investment | Denominator in CSR and CFC, and the basis for every spend window in SSS, CBR and CLS |
| **ROAS** | Absolute revenue reporting | **ROASL** — to compare fairly across campaigns. 3× in warm retargeting and 3× in cold prospecting are not equivalent |
| **CVR** | Landing page performance | **CVLS** — 2% means very different things on cold awareness and warm retargeting |
| **CPA** | Absolute profitability | **CPALS** for fair comparison; **mCPA** with PCS when actuals are unavailable or not yet reliable |
| **Hook rate** (2–3s view) | Whether a video's opening earns attention | Low hook rate with healthy CAS suggests the ad earns clicks *despite* a weak opening — a strong visual hook is worth testing |
| **Video completion** | Whether the message holds | Low VCR with high CAS: the hook works, the message loses people before the ask |
| **Thumb stop rate** | Visual salience, especially for statics | Feeds visual-hierarchy analysis in `creative-autopsy-runner` |
| **Engagement rate** | Social resonance | High engagement with positive CAS suggests audience fit beyond click behavior |
| **Bounce rate** | Post-click diagnosis | The corroborating signal for IQP. High bounce + low IQP confirms clickbait behavior; high bounce + high IQP points at the landing page instead |

---

## Where these metrics come from

None of them are invented from scratch. Each is a creative-side translation of a problem another
discipline already solved — which is also why the thresholds behave sensibly.

| Metric | Borrowed from | The borrowed idea |
|---|---|---|
| CAS, CVLS, CPALS, ROASL | **Medicine** — lab reference ranges | A raw value is meaningless until normalized against a reference range for that context. A cholesterol number alone tells a doctor nothing |
| CDV | **PR** — advertising value equivalency | Valuing what you generated at what it would have cost to buy |
| CSR | **Finance** — return on investment | Value back per dollar in |
| CCE, CFC | **Accounting** — variance analysis | Expected cost vs actual, as a percentage and in absolute dollars |
| CCR | **Finance** — portfolio theory | Concentration risk: exposure if the top holding fails |
| SSS | **Engineering** — signal processing | Whether a signal holds under load |
| IQP | **Fraud detection** — anomaly detection | Constellation-of-signals logic for spotting suspicious performance |
| PCS | **Actuarial science** — Bayesian inference | Confidence scoring on modeled rather than observed outputs |
| CFR | **Epidemiology** — predictive diagnostics | Reading early indicators of decline before symptoms appear |

---

## The seven-step decision sequence

Sequential but forgiving. Every conditional has a defined insufficient-data state; ambiguous cases
route to the most conservative response rather than forcing false confidence.

1. **Signal present?** CAS > 1.20 strong · 0.90–1.20 moderate · < 0.90 weak → CFR check. Below exposure → Emerging.
2. **Generating value?** CCE and CFC positive → efficiency confirmed. Both negative → Optimization or Refresh, check DSC first.
3. **Will it hold?** SSS ≥ 80 stable · 60–79 monitor · < 60 fragile → Validation Queue.
4. **Clicks worth having?** IQP ≥ 70 fine · 40–69 monitor · < 40 → Clickbait Risk regardless of Layer 1.
5. **Lifecycle stage?** CLS from CAS trajectory, SSS, CBR, CFR.
6. **Profitable?** mCPA/mRM with PCS confidence, or actuals where available.
7. **Portfolio fit?** CFR, CCR, DSC modify the asset-level action.

---

*MIT © Raneq Barber. Part of Growth AI Skills.*
