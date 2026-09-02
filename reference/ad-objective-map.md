# Ad Objective Normalization Map

Segmentation in this system runs on **platform × ad objective**, not platform × funnel stage.

**Why objective.** Every ad platform reports it natively in every export, so no manual tagging step
is required and nothing depends on whether a team maintains a funnel taxonomy. More importantly it
is *causally* closer to the thing that makes segmentation necessary: the platform optimises
delivery differently per objective, so the CTR and CPC distributions genuinely differ. Funnel stage
is a strategic label applied to a campaign. Objective is the instruction the algorithm actually
followed.

**Author:** Raneq Barber.

---

## Canonical objectives

Six. Deliberately few — more granularity fragments benchmark segments below usable volume.

| Canonical | What the platform is optimising for |
|---|---|
| `awareness` | Reach, impressions, brand lift |
| `traffic` | Clicks and landing page views |
| `engagement` | Post engagement, video views, follows |
| `leads` | Form fills, lead objects, on-platform capture |
| `sales` | Purchases, conversions, revenue events |
| `app` | Installs and in-app events |

---

## Platform mapping

### Meta

| Platform objective | Canonical |
|---|---|
| `OUTCOME_AWARENESS` | awareness |
| `OUTCOME_TRAFFIC` | traffic |
| `OUTCOME_ENGAGEMENT` | engagement |
| `OUTCOME_LEADS` | leads |
| `OUTCOME_SALES` | sales |
| `OUTCOME_APP_PROMOTION` | app |
| Legacy `REACH`, `BRAND_AWARENESS` | awareness |
| Legacy `LINK_CLICKS` | traffic |
| Legacy `CONVERSIONS`, `CATALOG_SALES` | sales |
| Legacy `LEAD_GENERATION` | leads |
| Legacy `VIDEO_VIEWS`, `POST_ENGAGEMENT` | engagement |

### LinkedIn

| Platform objective | Canonical |
|---|---|
| `BRAND_AWARENESS` | awareness |
| `WEBSITE_VISITS` | traffic |
| `ENGAGEMENT`, `VIDEO_VIEWS` | engagement |
| `LEAD_GENERATION` | leads |
| `WEBSITE_CONVERSIONS` | sales |
| `JOB_APPLICANTS` | leads |

### TikTok

| Platform objective | Canonical |
|---|---|
| `REACH` | awareness |
| `TRAFFIC` | traffic |
| `VIDEO_VIEWS`, `ENGAGEMENT`, `COMMUNITY_INTERACTION` | engagement |
| `LEAD_GENERATION` | leads |
| `CONVERSIONS`, `PRODUCT_SALES` | sales |
| `APP_PROMOTION` | app |

### Google Ads

| Campaign goal / type | Canonical |
|---|---|
| Awareness and consideration · Demand Gen | awareness |
| Website traffic | traffic |
| Video views | engagement |
| Leads | leads |
| Sales · Performance Max with purchase goal | sales |
| App promotion | app |

### Pinterest

| Platform objective | Canonical |
|---|---|
| `AWARENESS`, `VIDEO_VIEW` | awareness |
| `CONSIDERATION`, `WEB_SESSIONS` | traffic |
| `CONVERSIONS`, `CATALOG_SALES` | sales |

### Snapchat

| Platform objective | Canonical |
|---|---|
| `AWARENESS`, `BRAND_AWARENESS` | awareness |
| `TRAFFIC` | traffic |
| `ENGAGEMENT`, `VIDEO_VIEWS` | engagement |
| `LEAD_GENERATION` | leads |
| `WEB_CONVERSION`, `CATALOG_SALES` | sales |
| `APP_INSTALLS` | app |

---

## Rules

**Map at ingest, keep the original.** Store `platform_objective` verbatim alongside
`ad_objective`. The raw value is needed to audit a mapping and to handle platform renames.

**Never pool across objectives.** An awareness CTR and a sales CTR are different populations. A
benchmark spanning both is meaningless for either.

**Objective can change mid-flight.** Campaign edits change the objective while an ad keeps running.
Where the export carries objective per period, segment per period. Where it does not, flag any asset
whose objective changed and exclude it from benchmark construction — its history spans two delivery
regimes.

**Handle the collapse case.** Some accounts run everything under one objective — usually `sales` —
regardless of strategic intent. That collapses the segmentation to platform alone. Detect it: if
over 85% of spend sits under one objective, say so, and fall back to funnel stage if the account
maintains one.

**Funnel stage remains an optional override.** Where a team genuinely maintains funnel labels and
wants them, accept `funnel_stage` as the segmentation key instead. The system does not care which
label is used, only that segments are internally comparable and consistently applied.

**Unmapped values.** A platform objective with no entry in this map is assigned `unmapped`, is
reported, and is excluded from benchmark construction until mapped. Never silently bucket an
unknown objective into a canonical one.

---

*MIT © Raneq Barber. Part of Growth AI Skills.*
