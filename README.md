# gb-electricity-ui

AI start here.

This repository follows the GlobalGrid2050 Data Discipline Manual:

https://github.com/Ventusltd/globalgrid2050-hompage/blob/main/docs/DATA_DISCIPLINE_MANUAL.md

Read the manual before editing this repository.

This repo is the UI layer for GB electricity tracker pages and generation history pages.

It must not own the source data.

It should read GB electricity data from:

https://github.com/Ventusltd/data-gb-electricity

It should read interconnector flow data from:

https://github.com/Ventusltd/data-interconnectors

The UI can display, filter and chart data. It must not silently redefine the data grain or create a second source of truth.

Current migration task:

Port the existing blank UI shells from the monolith routes:

https://globalgrid2050.com/uk_energy_tracking_v6/

https://globalgrid2050.com/uk_energy_tracking_v6/generation_history/

Add a visible source note on each page showing the current monolith source and the intended new federated data source.

Do not wire live data until the data repos have passed their declared data-law checks.
