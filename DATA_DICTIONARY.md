# Data dictionary — `components.json` / `components.csv`

One JSON array; one object per active component. `components.csv` is the same rows flattened for spreadsheet/analyst use — identical columns, with the nested `specs` object JSON-encoded in a single column.

| Field | Meaning |
|---|---|
| `id` | Stable component key (BRAND-MODEL-DATE form) |
| `subsystem` | propulsion / energy / electronics / payload (airframes/structural hardware are deliberately excluded from this dataset — see README scope note) |
| `brand`, `model` | Manufacturer and product |
| `technology` | Canonical technology/type (e.g. BLDC_OUTRUNNER, LIPO, LIDAR_SCANNER) |
| `hs_code` | HS-2022 6-digit trade classification (where assigned) |
| `country_of_manufacture` | Manufacturing country, ISO3 only. Absent when unknown or when our sources conflict — absence is honest, not an oversight |
| `country_designed` | Design-HQ country (brand's engineering base), ISO3 only. Differs from `country_of_manufacture` for fabless/offshored brands (e.g. AMD: designed USA, made TWN) |
| `specs` | Object of raw vendor-stated specs |
| `max_power_w`, `weight_g`, `wh_per_kg`, `capacity_mah`, `voltage` | Normalized specs where available |
| `price_usd`, `price_min`, `price_max`, `price_is_range` | Price. When `price_is_range` is true, `price_usd` is the MIDPOINT of `[price_min, price_max]` — an estimate, not a quoted price |
| `source_url` | Cited source (vendor product/datasheet page) |
| `source_authority` | What kind of source backs the row: `vendor_datasheet`, `vendor_marketing`, `faa_uasdoc` (FAA UAS DoC public API), `fcc_oet_grant`, etc. |
| `source_status` | Link health: `ok` (verified deep link) · `bot_blocked` (page exists, blocks crawlers; content not re-verified) · `homepage` (resolves to vendor domain only) · `dead` (404/unreachable) · `none` (no source). ABSENT when the link has not been audited |
| `blue_uas_status` | `framework_listed` (component on the DCMA Blue UAS Framework, snapshot 2026-07-02 — component-level authoritative) · `cleared_list` (aircraft on the DCMA Blue UAS Cleared List) · `component_of_cleared_platform` (named for a cleared aircraft; NOT a component clearance) · `vendor_claimed` (manufacturer claim, NOT verified against either list) · `none` |
| `confidence_tier` | Evidence tier: `Tier 0 Anchor` (complete provenance, direct observation) / `Tier 1 Verified` (complete provenance from an authoritative government source — FAA DoC, FCC grant, DCMA list) / `Tier 2 Triangulated` (at least one INDEPENDENT corroborating source beyond the vendor listing — platform usage, exact FCC grantee/equipment match, or FAA DoC) / `Tier 3 Inferred` (single-source or inferred) |

**Coverage is reported honestly.** Not every field is populated for every row, and `source_status`
tells you exactly which provenance links are live. Treat `confidence_tier` and `source_status` as
first-class: this DB is reliable for individual product lookups; aggregate/share queries should weight
by tier and source health. The database is a curated convenience sample (AUVSI-derived + targeted
additions), NOT a census of the UAS component market — coverage varies by subsystem (see below), and
share statistics inherit that selection.

## Explorer embeds

The interactive explorers inline their own copy of this dataset plus display fields:
technology-hierarchy grouping (`_technology`, `_technology_class`, `_technology_leaf`,
`_propulsion_role`), region grouping/colors (`_country_group`, `_country_color`,
), triangulation summaries (`n_sources`, `src_datasheet`, `src_fcc`,
`src_faa`), public regulatory corroboration detail (`fcc_grantees`, `fcc_equipment_grant`,
`faa_doc` — FCC/FAA public records), extra raw spec fields (motor/ESC/engine/propeller
dimensions), and price *estimates* (`price_estimate_*` — modeled, never mixed with observed
`price_usd`). Explorer rows carry `row_id` matching `id` in this file; the two surfaces
describe the SAME set of components.

## Coverage (auto-generated 2026-07-21 from this export, n=1,627)

**Evidence tiers:** Tier 3 Inferred: 934 (57%) · Tier 0 Anchor: 349 (21%) · Tier 2 Triangulated: 333 (20%) · Tier 1 Verified: 11 (<1%)

**Per-field coverage:**

| Field | Present | % |
|---|---|---|
| `technology` | 1,616 | 99% |
| `hs_code` | 1,480 | 91% |
| `country_of_manufacture` | 1,483 | 91% |
| `specs` | 1,381 | 85% |
| `source_url` | 1,516 | 93% |
| `source_status` | 1,496 | 92% |
| `price_usd` | 790 | 49% |
| `weight_g` | 1,086 | 67% |
| `max_power_w` | 325 | 20% |
| `blue_uas_status` | 1,384 | 85% |

**Dispersion warning — pooled stats hide subsystem differences** (this includes country concentration: a pooled 'share from country X' is not representative of any single subsystem — always stratify):

| Subsystem | n | source ok | Tier 0 share | top mfg country |
|---|---|---|---|---|
| electronics | 580 | 77% | 13% | CHN (37%) |
| energy | 103 | 83% | 7% | USA (25%) |
| payload | 349 | 70% | 8% | USA (34%) |
| propulsion | 595 | 91% | 40% | CHN (58%) |
