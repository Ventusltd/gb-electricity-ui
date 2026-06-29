# UI_CHARTS_MIGRATION_SCOPE.md

Status: approved scope commit, amended with data pre-port gate  
Repo in scope: `Ventusltd/gb-electricity-ui`  
Reference repo: `Ventusltd/globalgrid2050`  
Governance repo: `Ventusltd/globalgrid2050-hompage`  
Rule: scope first, audit second, fire third

## 0. Pre-port rule: data must sit clean before charts move

The data dependencies must be resolved before the old charts are ported forward. The project order is: make the data sit clean, audited and documented first; then bring the chart structure across; then wire charts only after the upstream data products have proof.

This prevents the UI repo from becoming another monolith, another hidden data contract, or a chart shell that looks correct while sitting on unproven data.

Before chart porting proceeds beyond scope and audit, resolve these upstream blockers:

```text
1. data-gb-electricity: prove exact chart dependencies for FUELINST, FUELHH and prices.
2. data-gb-electricity: create any missing derived browser/rollup products needed by the charts.
3. data-interconnectors: write contract, source register, audit and apply clean Parquet for named interconnector flows.
4. PVLive solar: decide whether it belongs in data-gb-electricity, a separate solar data repo, or a deferred/no-data chart state.
5. Frequency, carbon, oil, road fuel and EV feeds: either scope separate data products or leave those panels as no-data structure in the first UI copy.
6. Only after the data products are proven should gb-electricity-ui receive live data wiring.
```

The UI repo must consume data products that already sit clean. It must not port charts first and then retrofit data underneath them. Data before charts remains the governing rule.

## 0A. Data dependency precondition before chart migration

Charts must not be treated as ready for data wiring until the upstream data dependencies are updated, inventoried and proven.

First blocker: `Ventusltd/data-interconnectors` must be updated before generation-history chart wiring. The old generation-history page uses named interconnector import/export logic and annual link-direction views. That data must land first as a clean data product in `data-interconnectors`, not as copied monolith JSON inside this UI repo. The required separate data scope is the Data Interconnectors Port Scope: Elexon BMRS FUELINST INT rows, ten BMRS interconnector codes, positive signed MW as import to GB, negative signed MW as export from GB, flows not domestic generation, compact zstd Parquet, declared key `periodStartUTC` plus `bmrsCode`, and audit/apply workflow discipline. That scope belongs in `Ventusltd/data-interconnectors`, not in this UI repo.

Second blocker: `Ventusltd/data-gb-electricity` must be audited for the exact chart dependencies before any chart data wiring. The UI must not assume the data exists merely because the repo exists. The audit must confirm actual partition coverage, declared keys, duplicate-key checks, canary rows, row counts, and whether the monthly updater has completed a controlled proof run.

Known `data-gb-electricity` chart dependencies to check:

```text
Tracker live generation snapshot and generation mix:
  Needs FUELINST provisional generation, or a derived browser-ready/live latest product from data-gb-electricity.

Generation-history MW chart:
  Needs FUELHH settled generation for historic views and possibly FUELINST/recent generation for short windows.

Generation MWh annual, monthly and day/night panels:
  Needs FUELHH-derived MWh rollups from data-gb-electricity. If PVLive solar remains part of the chart, solar/PVLive must be separately scoped because it is not yet established here as a proven data-gb-electricity product.

Electricity price history chart:
  Needs Elexon system-price Parquet from data-gb-electricity plus any required daily/annual rollups or browser delivery products.

Frequency, oil, road fuel and EV panels:
  The old tracker uses monolith JSON, CSV and GeoJSON feeds. These are not automatically covered by data-gb-electricity. Either exclude them from first data wiring, leave them as no-data structure, or create separate source scopes before live wiring.

Interconnector link-direction views:
  Needs data-interconnectors first. Interconnector flows must stay outside domestic generation totals.
```

For this UI scope, these are blockers and dependency records only. The UI repo consumes data contracts. It does not author data contracts. Do not create `DEPENDENCIES.md`, rewrite data paths, wire Parquet, or point the new build at monolith data until the upstream data repos have produced approved audit evidence.

## 1. Purpose

Prepare the UI charts migration from the old `globalgrid2050` monolith into the new `gb-electricity-ui` repo.

This is Stage 1 only.

No code change.  
No UI change.  
No workflow run.  
No data wiring.  
No deletion.  
No live domain change.  
No custom domain cutover.

## 2. Current files confirmed in `gb-electricity-ui`

Confirmed current staging files:

```text
.nojekyll
README.md
UI_CHARTS_MIGRATION_SCOPE.md
index.html
assets/app.css
assets/blank-charts.js
docs/CHARTS_UI_SCOPE_AUDIT_COPY_OLD_MAKE_NEW_PARQUETS.md
uk_energy_tracking_v6/index.html
uk_energy_tracking_v6/monolith-reference.html
uk_energy_tracking_v6/new-build.html
uk_energy_tracking_v6/generation_history/index.html
uk_energy_tracking_v6/generation_history/monolith-reference.html
uk_energy_tracking_v6/generation_history/new-build.html
```

Not found at current inspection:

```text
DEPENDENCIES.md
CHANGELOG.md
package.json
reports/UI_CHARTS_MIGRATION_AUDIT_LATEST.md
reports/json/UI_CHARTS_MIGRATION_AUDIT_LATEST.json
```

The formal audit must re-check the full tree before any apply action.

## 3. Current route structure

Current UI repo routes:

```text
/
  index.html

/uk_energy_tracking_v6/
  index.html
  monolith-reference.html
  new-build.html

/uk_energy_tracking_v6/generation_history/
  index.html
  monolith-reference.html
  new-build.html
```

Current behaviour:

```text
/uk_energy_tracking_v6/
  side-by-side comparison page

/uk_energy_tracking_v6/monolith-reference.html
  iframe reference to live old monolith tracker

/uk_energy_tracking_v6/new-build.html
  simplified blank staging shell

/uk_energy_tracking_v6/generation_history/
  side-by-side comparison page

/uk_energy_tracking_v6/generation_history/monolith-reference.html
  iframe reference to live old monolith generation-history page

/uk_energy_tracking_v6/generation_history/new-build.html
  simplified blank staging shell
```

Current staging files are useful, but they are not final architecture. They are comparison scaffolds and placeholder shells.

## 4. Old monolith source pages

These are the two old pages to copy or reference later:

```text
globalgrid2050/uk_energy_tracking_v6/index.md
globalgrid2050/uk_energy_tracking_v6/generation_history/index.md
```

Live reference URLs:

```text
https://globalgrid2050.com/uk_energy_tracking_v6/
https://globalgrid2050.com/uk_energy_tracking_v6/generation_history/
```

These old pages must not be edited.

## 5. Old monolith data dependency evidence

The old tracker page and generation-history page prove that the UI is not just a chart-copy problem. The pages include CSS, JavaScript and data dependencies. The UI can faithfully copy structure first, but data wiring is blocked until the data repositories prove the required products.

Required upstream order before live chart data wiring:

```text
1. data-gb-electricity inventory and controlled proof for FUELINST, FUELHH and prices.
2. data-interconnectors contract, audit and Parquet apply for named interconnector flows.
3. Decision on non-electricity tracker feeds: frequency, oil, road fuel and EV.
4. UI faithful copy and no-data structure.
5. UI data wiring only after upstream data proof.
```

## 6. Old tracker page direct CSS dependency

Old tracker page loads:

```text
/uk_energy_tracking_v6/styles/app.css?v=20260604toolbargrid1
```

## 7. Old generation-history page direct CSS dependency

Old generation-history page loads:

```text
/uk_energy_tracking_v6/styles/app.css?v=20260604toolbargrid1
```

It also contains large inline page-specific CSS inside the page itself. That inline CSS must be preserved in any faithful copy.

## 8. Old tracker page direct JavaScript dependencies

Old tracker page loads:

```text
/uk_energy_tracking_v6/shared_helpers/dom_text/dom_text.js
/uk_energy_tracking_v6/live_data_pipeline/live-config.js
/uk_energy_tracking_v6/live_data_pipeline/load_json/load_json.js
/uk_energy_tracking_v6/live_data_pipeline/render_live_snapshot/render_live_snapshot.js
/uk_energy_tracking_v6/live_data_pipeline/render_generation_mix/render_generation_mix.js
/uk_energy_tracking_v6/commodity_price_signals/render_commodities/render_commodities.js
/uk_energy_tracking_v6/price_history_chart/load_price_history_data/load_price_history_data.js
/uk_energy_tracking_v6/price_history_chart/render_price_chart/render_price_chart.js
/uk_energy_tracking_v6/price_history_chart/control_price_history/control_price_history.js
/uk_energy_tracking_v6/price_history_chart/fullscreen_period_menu/fullscreen_period_menu.js
/uk_energy_tracking_v6/app_bootstrap/start_v6_app/start_v6_app.js
/uk_energy_tracking_v6/frequency_history/frequency-history-ui.js
```

## 9. Old generation-history page direct JavaScript dependencies

Old generation-history page loads:

```text
/uk_energy_tracking_v6/generation_history/live-config.js
/uk_energy_tracking_v6/generation_history/load_generation_mwh_aggregates.js
/uk_energy_tracking_v6/generation_history/render_generation_mwh_aggregates.js
/uk_energy_tracking_v6/generation_history/control_generation_mwh_aggregates.js
/uk_energy_tracking_v6/generation_history/load_generation_history_data.js
/uk_energy_tracking_v6/generation_history/render_generation_history_chart.js
/uk_energy_tracking_v6/generation_history/control_generation_history.js
/uk_energy_tracking_v6/generation_history/render_solar_daily_mwh_chart.js
/uk_energy_tracking_v6/generation_history/control_solar_daily_mwh_chart.js
```

## 10. Old tracker page JSON, GeoJSON and CSV dependencies

The tracker page and its scripts reference:

```text
/uk_energy_tracking_v6/live_grid_energy.json
/uk_energy_tracking_v6/live_grid_price.json
/uk_energy_tracking_v6/live_oil_prices.json
/uk_energy_tracking_v6/oil_price_history.geojson
/uk_energy_tracking_v6/live_uk_fuel_prices.json
/uk_energy_tracking_v6/ev_charging_prices.json
/uk_energy_tracking_v6/grid_frequency_history.csv
/uk_energy_tracking_v6/grid_frequency_weekly_health.csv
/uk_energy_tracking_v6/live_grid_frequency.json
/uk_energy_tracking_v6/live_grid_frequency_weekly_health.json
/uk_energy_tracking_v6/electricity_price_history.csv
/uk_energy_tracking_v6/electricity_price_history.json
/uk_energy_tracking_v6/electricity_price_history_daily_decade.json
/data/electricity/elexon_system_prices_YYYY.csv
```

These data files must not be copied into the UI repo.

## 11. Old generation-history JSON dependencies

The generation-history page and its scripts reference:

```text
/data/confirmed/generation_daily_mw_spine_fuelhh_candidate.json
/data/generation/elexon_generation_sources_2016.json
/data/generation/elexon_generation_sources_YYYY
/uk_energy_tracking_v6/generation_history/pvlive_solar_daily_browser.json
/uk_energy_tracking_v6/generation_history/pvlive_solar_recent_30d_30min_browser.json
/uk_energy_tracking_v6/generation_history/generation_recent_30d_30min.json
/uk_energy_tracking_v6/generation_history/generation_ecg_all_technologies_30d_30min_candidate.json
/uk_energy_tracking_v6/generation_history/generation_annual_mwh_by_technology.json
/uk_energy_tracking_v6/generation_history/generation_monthly_mwh_by_technology.json
/uk_energy_tracking_v6/generation_history/generation_seasonal_mwh_by_technology.json
/uk_energy_tracking_v6/generation_history/generation_day_night_mwh_by_technology.json
/uk_energy_tracking_v6/generation_history/generation_daily_mwh_by_technology_fuelhh_2016_2026.json
/uk_energy_tracking_v6/generation_history/interconnectors/generation_interconnector_index.json
/uk_energy_tracking_v6/generation_history/interconnectors/generation_interconnector_total_electricity_summary.json
```

These data files must not be copied into the UI repo.

## 12. Absolute paths requiring rewrite for GitHub Pages

All root-relative paths beginning with `/` must be reviewed before copying into the UI repo.

Path groups requiring rewrite or deliberate disabling:

```text
/uk_energy_tracking_v6/...
/uk_energy_tracking_v6/generation_history/...
/data/electricity/...
/data/generation/...
/data/confirmed/...
/uk_renewables_pipeline/...
```

Safe rule for GitHub Pages staging:

```text
Use relative paths for copied UI CSS and JS.
Do not rewrite old data paths into new live data paths yet.
Do not wire Parquet.
Do not create UI-owned data contracts.
Do not silently point the new build at monolith data.
```

## 13. Page components that must be preserved

Tracker page components to preserve:

```text
hero title and subtitle
latest data update area
live electricity snapshot cards
generation mix grid
electricity price history controls
year selector
start date selector
period selector
download CSV link
fullscreen chart button
range status
unit panel
Grid intelligence machine note
price-history canvas
Elexon price explainer
latest visible price card
settlement time card
visible records card
source card
captured records table
commodity price cards
oil price trend controls
oil trend canvas
oil tooltip
oil stats grid
road fuel cards
fuel breakdown panel
source links
EV charging placeholder cards
data source attribution panel
price-history fullscreen overlay
fullscreen toolbar
fullscreen period selector
fullscreen previous and next buttons
fullscreen canvas
fullscreen smallprint
frequency history panel inserted by JS
frequency weekly health panel inserted by JS
```

Generation-history page components to preserve:

```text
hero title and subtitle
intro text
deep study summary
generation MWh aggregate panel
technology selector
generation MWh status
annual MWh card
monthly MWh card
day/night MWh card
sunrise/sunset attribution
solar daily MWh panel
solar daily fullscreen button
solar daily technology selector
solar daily year selector
solar daily start date selector
solar daily period selector
solar daily status
solar daily canvas
generation output details panel
generation-history technology selector
generation-history year selector
generation-history start date selector
generation-history period selector
generation-history range status
generation-history canvas
source transparency warning
ONS annual MWh placeholder
solar daily fullscreen overlay
solar daily fullscreen toolbar
solar daily fullscreen period selector
solar daily fullscreen previous and next buttons
solar daily fullscreen canvas
solar daily fullscreen smallprint
all generation-history inline CSS
```

## 14. Files to copy later after approval

Later faithful-copy stage should copy or port these monolith files into `gb-electricity-ui`, with data fetches disabled until data proof exists.

Tracker page:

```text
globalgrid2050/uk_energy_tracking_v6/index.md
globalgrid2050/uk_energy_tracking_v6/styles/app.css
globalgrid2050/uk_energy_tracking_v6/shared_helpers/dom_text/dom_text.js
globalgrid2050/uk_energy_tracking_v6/live_data_pipeline/live-config.js
globalgrid2050/uk_energy_tracking_v6/live_data_pipeline/load_json/load_json.js
globalgrid2050/uk_energy_tracking_v6/live_data_pipeline/render_live_snapshot/render_live_snapshot.js
globalgrid2050/uk_energy_tracking_v6/live_data_pipeline/render_generation_mix/render_generation_mix.js
globalgrid2050/uk_energy_tracking_v6/commodity_price_signals/render_commodities/render_commodities.js
globalgrid2050/uk_energy_tracking_v6/price_history_chart/load_price_history_data/load_price_history_data.js
globalgrid2050/uk_energy_tracking_v6/price_history_chart/render_price_chart/render_price_chart.js
globalgrid2050/uk_energy_tracking_v6/price_history_chart/control_price_history/control_price_history.js
globalgrid2050/uk_energy_tracking_v6/price_history_chart/fullscreen_period_menu/fullscreen_period_menu.js
globalgrid2050/uk_energy_tracking_v6/app_bootstrap/start_v6_app/start_v6_app.js
globalgrid2050/uk_energy_tracking_v6/frequency_history/frequency-history-ui.js
```

Generation-history page:

```text
globalgrid2050/uk_energy_tracking_v6/generation_history/index.md
globalgrid2050/uk_energy_tracking_v6/generation_history/live-config.js
globalgrid2050/uk_energy_tracking_v6/generation_history/load_generation_mwh_aggregates.js
globalgrid2050/uk_energy_tracking_v6/generation_history/render_generation_mwh_aggregates.js
globalgrid2050/uk_energy_tracking_v6/generation_history/control_generation_mwh_aggregates.js
globalgrid2050/uk_energy_tracking_v6/generation_history/load_generation_history_data.js
globalgrid2050/uk_energy_tracking_v6/generation_history/render_generation_history_chart.js
globalgrid2050/uk_energy_tracking_v6/generation_history/control_generation_history.js
globalgrid2050/uk_energy_tracking_v6/generation_history/render_solar_daily_mwh_chart.js
globalgrid2050/uk_energy_tracking_v6/generation_history/control_solar_daily_mwh_chart.js
```

Do not copy old JSON, CSV, GeoJSON or generated data products into the UI repo.

## 15. Files not to touch in Stage 1

Do not touch:

```text
globalgrid2050/*
globalgrid2050-hompage/*
data-gb-electricity/*
data-interconnectors/*
globalgrid2050.com live pages
custom domain settings
GitHub Pages domain settings
old monolith data files
old monolith workflows
current UI route files
current UI CSS
current UI blank chart JS
```

Within `gb-electricity-ui`, this amended documentation commit should touch only:

```text
UI_CHARTS_MIGRATION_SCOPE.md
```

## 16. What is missing from current UI staging repo

Missing items:

```text
DEPENDENCIES.md
reports/UI_CHARTS_MIGRATION_AUDIT_LATEST.md
reports/json/UI_CHARTS_MIGRATION_AUDIT_LATEST.json
faithful copied tracker page structure
faithful copied generation-history page structure
copied old app.css dependency
copied old tracker JS dependency tree
copied old generation-history JS dependency tree
formal path rewrite table
formal no-data loading guard
visual parity checklist
component parity checklist
route parity checklist
data dependency log
upstream proof that data-interconnectors has clean interconnector Parquet
upstream proof that data-gb-electricity has required generation and price products
upstream decision for frequency, oil, road fuel and EV feeds
```

Current `new-build.html` files are simplified placeholder shells, not faithful ports.

## 17. Main risks

Key risks:

```text
Absolute root paths may fail on GitHub Pages project staging.
A chart loading is not proof of parity.
Blank chart renderer can create false confidence.
Copying old JS may accidentally fetch old monolith data.
Copying old JSON or CSV into the UI repo would break the federation model.
Generation-history depends on both generation data and interconnector flow data.
Interconnectors are flows, not domestic generation.
The old Imports & Exports bucket must not be mixed into generation.
The frequency panel is dynamically inserted by JavaScript and can be missed.
The generation-history page has large inline CSS that can be missed.
Jekyll Markdown front matter in old `.md` pages must not be copied blindly into static HTML.
Reference iframes are comparison aids only, not proof.
GitHub workflow green status is not proof.
No data contract belongs in the UI repo.
Data-interconnectors is a blocker before generation-history flow panels can be wired.
Data-gb-electricity must prove generation and prices before UI charts consume them.
PVLive solar and non-electricity tracker feeds may need separate source scopes.
```

## 18. Safest first surgical commit

The first surgical commit has been made as a documentation-only scope commit. This amended commit also remains documentation-only.

Expected result of this amendment:

```text
UI_CHARTS_MIGRATION_SCOPE.md records upstream data blockers first.
No UI files changed.
No CSS changed.
No JS changed.
No data added.
No workflow added.
No workflow run.
No live domain change.
```

Then stop for approval.

## 19. Audit report to produce after scope approval

After Vikram approves this amended scope, produce a Stage 1 audit report only:

```text
reports/UI_CHARTS_MIGRATION_AUDIT_LATEST.md
reports/json/UI_CHARTS_MIGRATION_AUDIT_LATEST.json
```

Audit report must include:

```text
current UI file tree
current route map
old source page SHAs
old CSS dependency list
old JS dependency list
old JSON/CSV/GeoJSON dependency list
absolute path rewrite table
current staging artefact assessment
component parity checklist
files proposed for later copy
files explicitly not touched
risk register
first apply plan
proof that no data files are copied
proof that no Parquet is wired
proof that no data contract is authored in UI
upstream data dependency status for data-gb-electricity
upstream data dependency status for data-interconnectors
specific blocker list before generation, price and interconnector chart wiring
```

Matching JSON report must include machine-readable arrays for:

```text
uiFiles
routes
oldSourcePages
cssDependencies
jsDependencies
dataDependencies
absolutePaths
plannedCopiedFiles
untouchedFiles
risks
checks
upstreamDataBlockers
```

Audit mode must read what exists and write reports only. Apply mode must not exist or run until the audit has been reviewed and separately approved.

## 20. Approval gate

This document is not approval to change UI.

Approval required before:

```text
creating or running any workflow
copying any monolith file
rewriting any path
adding DEPENDENCIES.md
changing any route
touching any data dependency
wiring any data source
editing CSS
editing JS
deleting files
changing live pages
```

Next required action after this amended scope commit:

```text
Prepare Stage 1 audit report only, then stop for review.
```
