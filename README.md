# UAS Component Database — open data

**1,627 UAS/drone components** — propulsion, energy, electronics, and payload — with
per-record **evidence tiers**, **source citations**, and **manufacturing-country** data.

Browse it: **https://manf.us/drones/**

## Files
- `components.json` — one object per component (canonical)
- `components.csv` — same rows flattened for spreadsheets (nested `specs` JSON-encoded)
- `DATA_DICTIONARY.md` — every column, the evidence-tier semantics, and coverage notes

## Evidence tiers
Tier 0 Anchor (measured/teardown) · Tier 1 Verified (authoritative government source) ·
Tier 2 Triangulated (two independent domains) · Tier 3 Inferred. Read the dictionary
before comparing across tiers.

## Corrections & additions
Open an issue here, or use the submission form at https://manf.us/drones/submit.html.
Every record carries its `source_url` — corrections with a citable source are merged fastest.

## License & attribution
Data: **CC-BY 4.0** — attribute "NO LAB UAS Component Database" and the original
manufacturers; records derived from the Tyto Robotics thrust-stand database are used
with permission and attribution. See `LICENSE`.

*A NO LAB open dataset.*
