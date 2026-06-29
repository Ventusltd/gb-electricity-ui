# UI_CHARTS_MIGRATION_SCOPE.md

Status: approved scope commit  
Repo in scope: `Ventusltd/gb-electricity-ui`  
Reference repo: `Ventusltd/globalgrid2050`  
Governance repo: `Ventusltd/globalgrid2050-hompage`  
Rule: scope first, audit second, fire third

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

## 5. Old tracker page direct CSS dependency

Old tracker page loads:

```text
/uk_energy_tracking_v6/styles/app.css?v=20260604toolbargrid1
```

## 6. Old generation-history page direct CSS dependency

Old generation-history page loads:

```text
/uk_energy_tracking_v6/styles/app.css?v=20260604toolbargrid1
```

It also contains large inline page-specific CSS inside the page itself. That inline CSS must be preserved in any faithful copy.

## 7. Old tracker page direct JavaScript dependencies

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

## 8. Old generation-history page direct JavaScript dependencies

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

## 9. Old tracker page JSON, GeoJSON and CSV dependencies

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

## 10. Old generation-history JSON dependencies

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

## 11. Absolute paths requiring rewrite for GitHub Pages

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

## 12. Page components that must be preserved

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

## 13. Files to copy later after approval

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

## 14. Files not to touch in Stage 1

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

Within `gb-electricity-ui`, the first approved commit should touch only:

```text
UI_CHARTS_MIGRATION_SCOPE.md
```

## 15. What is missing from current UI staging repo

Missing items:

```text
UI_CHARTS_MIGRATION_SCOPE.md
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
```

Current `new-build.html` files are simplified placeholder shells, not faithful ports.

## 16. Main risks

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
```

## 17. Safest first surgical commit

First approved commit should be documentation only:

```text
Add UI_CHARTS_MIGRATION_SCOPE.md
```

Suggested commit message:

```text
docs: scope UI charts migration
```

Expected result:

```text
One new scope document exists in gb-electricity-ui.
No UI files changed.
No CSS changed.
No JS changed.
No data added.
No workflow added.
No workflow run.
No live domain change.
```

Then stop for approval.

## 18. Audit report to produce after scope approval

After Vikram approves this scope, produce a Stage 1 audit report only:

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
```

Audit mode must read what exists and write reports only. Apply mode must not exist or run until the audit has been reviewed and separately approved.

## 19. Approval gate

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

Next required action after this scope commit:

```text
Prepare Stage 1 audit report only, then stop for review.
```
