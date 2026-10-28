# DIFRI — Interactive Dashboard

**Title:** *DIFRI: Quantifying Digital Financial Risk Through Consumer-Generated Data for Digital Resilience in Indonesia*

This repository contains a static, interactive dashboard for exploring the DIFRI results snapshot. It is designed to be published with GitHub Pages.

## Contents

- `index.html` — interactive dashboard, charts, and embedded workbook snapshot.
- `METODOLOGI_RUMUS.md` — definitions and formulas for the primary estimator and retained sensitivity analyses.
- `.nojekyll` — tells GitHub Pages to serve the static files without Jekyll processing.

## Open locally

Open `index.html` in a current browser with an internet connection. The dashboard loads Plotly.js, fonts, and MathJax from external CDNs.

## Public-data warning — review before publishing

The HTML embeds data from 19 workbook sheets, including tables that can be searched and downloaded as CSV through the dashboard. Publishing this repository or enabling GitHub Pages makes the embedded tables publicly accessible. **Do not publish until the source-data permissions, app-level disclosure, and any applicable privacy or licensing restrictions have been checked.** This repository does not include the SQLite database or raw review-text database.

## Interpretation limits

DIFRI summarizes consumer-reported digital financial risk signals in app reviews. It is not a verified incident rate, a measure of monetary loss, a direct measurement of national digital resilience, evidence of causal policy impact, or a validated early-warning system. September 2026 is partial through 19 September and should be treated as provisional.

## Snapshot and reproducibility

This is a static snapshot, not a live connection to the analysis pipeline. If the source workbook or analysis outputs change, regenerate and validate the dashboard before publishing a new version. The dashboard snapshot alone does not guarantee full computational reproducibility; the research code, environment, and permitted supporting data should be archived separately after audit.

## License

No reuse license is specified in this package. Confirm ownership and reuse permissions before adding a license or inviting reuse.
