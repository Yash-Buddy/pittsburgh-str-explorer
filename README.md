# Airbnb-Term Rental Data Exploration


An experimental data visualisation project exploring a hand-checked, listing-level census of short-term rentals in four City of Pittsburgh neighbourhoods: Lower Lawrenceville, Central Lawrenceville, Upper Lawrenceville and Bloomfield. The census was compiled while the City's short-term rental zoning bill (Council Bill 2026-0009) was pending.

**[View the live visualisation →](#)** <!-- replace # with your deployed link -->

<!-- Optional: add a screenshot or GIF here -->
<!-- ![Preview](docs/preview.png) -->

> **Status:** Experimental. This is a learning and exploration project, not a production application, and not a policy or legal analysis.

---

## Questions explored

1. **Where are listings concentrated?** How do Airbnb listings distribute across the four neighbourhoods and relative to their boundaries?
2. **How do rates differ by neighbourhood and season?** How do observed stay totals compare across a mid-September midweek, a late-September weekend and a December weekend?
3. **What types of listings are there?** The mix of room types and guest capacities, including hotel-style rooms that are not dwelling units.
4. **How concentrated is hosting?** How many in-scope listings do individual hosts operate?
5. **How does the mid-term market compare?** How do Furnished Finder asking rents and minimum stays (30 days or more) sit alongside short-term stay prices?

## Visualisations

| View | What it shows | Chart type |
| --- | --- | --- |
| Listing map | Airbnb listing locations, with boundary-uncertain pins visually flagged | Map / scatter plot of coordinates |
| Neighbourhood counts | Live listings per neighbourhood | Bar chart |
| Rate windows | Distribution of stay totals per date window and neighbourhood | Box plot or strip plot |
| Room type and capacity | Room type mix and guest capacity | Bar chart |
| Host concentration | Listings per host within the study area | Histogram |
| Mid-term rents | Furnished Finder asking rent against minimum stay | Scatter plot |

<!-- Adjust this table to match what you actually built. -->

## Data

The visualisation uses the Pittsburgh short-term rental census published by Van to Vault (vantovault.com). Every row is a live listing that can be opened on its platform. Nothing in the dataset is modelled or estimated.

### Files

| File | Contents |
| --- | --- |
| Airbnb listings | 275 rows: 273 live and in scope as of 2026-08-31, plus 2 kept after delisting. Includes room type, capacity, host id, the host's in-scope listing count, review count, star rating, coordinates, distance to the nearest neighbourhood boundary with a boundary-uncertainty flag, and three observed two-night, two-guest stay totals (mid-September midweek, late-September weekend, December weekend). |
| Furnished Finder listings | 42 units across 40 properties in the 30-day-plus mid-term market, with published asking rent and minimum stay. |
| Summary manifest (JSON) | Per-neighbourhood counts and rate-window medians. |

### Read this before charting anything

- **Never add the Airbnb and Furnished Finder counts together.** The two platforms are kept separate deliberately, because their listings may overlap.
- **Counts are floors, not totals.** Pagination on both platforms under-delivers against the counts displayed, so the census completed coverage with quadrant-bounded searches. True counts may be higher.
- **Pin locations are approximate.** Airbnb displaces pins by roughly 150 m. 63 of the 273 live Airbnb rows are flagged as materially uncertain about which side of a neighbourhood boundary they fall on. Treat neighbourhood assignments for those rows with caution.
- **Some listings are not dwelling units.** Eight Airbnb rows are hotel-style rooms.
- **Rates are stay totals, not nightly rates.** Each figure is the observed total for a two-night, two-guest stay on specific dates. Dividing by two gives an approximation at best, because fees and minimum-night rules are baked in.
- **Zoning-district assignment is not included.** The publisher tested it and deliberately did not publish it, so this project makes no zoning claims.
- **This is a snapshot.** It reflects one point in time (2026-08-31 for the live Airbnb count), so it can't show trends.

### Source and licence

- **Method, limits and per-neighbourhood tables:** <https://vantovault.com/library/pittsburgh-short-term-rental-census/>
- **DOI (concept DOI, resolves to the latest version):** <https://doi.org/10.5281/zenodo.22258181>
- **Source repository:** <https://github.com/vantovault-stack/open-housing-data>
- **More open housing data from the same publisher:** <https://vantovault.com/open-data/>
- **Licence:** CC BY 4.0. Credit: Van to Vault (vantovault.com).

### Data preparation

Document what Pandas does before the data reaches D3. Suggested steps:

- Load the Airbnb, Furnished Finder and JSON manifest files as separate tables and keep them separate
- Keep the boundary-uncertainty flag so it can drive styling in the map
- Decide how to treat the 2 delisted rows and the 8 hotel-style rooms (exclude, or flag and show separately) and state the choice here
- Reconcile your per-neighbourhood counts against the manifest to confirm the load is correct
- Export cleaned data as CSV or JSON for the front end

## Tech stack

| Layer | Tools |
| --- | --- |
| Data processing | Python, Pandas |
| Visualisation | D3.js |
| Front end | JavaScript, HTML, CSS |

## Project structure

<!-- Update to match your actual folders. -->

```
.
├── data/
│   ├── raw/            # original census files
│   └── processed/      # cleaned data used by the front end
├── scripts/ or notebooks/   # Pandas cleaning and exploration
├── js/                 # D3 code
├── index.html
├── style.css
└── README.md
```

## Running locally

1. **Clone the repository**

   ```bash
   git clone <your-repo-url>
   cd <your-repo-name>
   ```

2. **Prepare the data** (skip if processed data is already included)

   ```bash
   pip install pandas
   python scripts/clean_data.py
   ```

3. **Serve the page.** Browsers block local file loading, so use a local server rather than opening `index.html` directly:

   ```bash
   python -m http.server 8000
   ```

   Then open <http://localhost:8000>.

## Observations

Fill this in with what the data showed you, for example:

- Which neighbourhood has the most live listings
- How stay totals shift between September and December
- How concentrated hosting is
- How mid-term asking rents compare with short-term stay totals

## Limitations

- Counts are floors and the dataset is a single snapshot.
- Pin displacement makes neighbourhood assignment uncertain for a meaningful share of listings.
- Stay totals are for specific dates, two nights and two guests, so they don't represent average nightly rates or a full year.
- Airbnb and Furnished Finder cover different markets and can't be combined into one total.
- Nothing here is legal or zoning advice.

## Possible next steps

- Filter by neighbourhood, room type or host size
- Add tooltips and a toggle to hide boundary-uncertain listings
- Compare rate windows within each neighbourhood
- Compare the study area against other cities' open housing data

## Acknowledgements

Data: Van to Vault (vantovault.com), CC BY 4.0. Add any D3 examples or tutorials that helped.

## Licence

Add your chosen licence for the code (for example MIT). The data remains under CC BY 4.0.