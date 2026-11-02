# Heat Risk Hessen: the design

**Heat, deaths and who lives where it's hottest, 2000–2025** · Project E of the
Master Plan · German title: *Hitzerisiko Hessen*

*Confirmed on 02.11.2026, after a draft of 01.11.2026 and revisions on
02.11.2026. Only the death counts have been opened, and only to check them (D1
below). Nothing has been modelled. The repository's first commit fixes this
design before any other data is pulled. Later changes go under Amendments,
dated, and the text above them is not edited. Build window: 01.–21.02.2027
(Master Plan §2.6).*

## The question

How has summer heat in Hessen changed since 2000, and where in its cities does
it concentrate? How many deaths does it cause, and has the population grown
less sensitive to it? And who lives where the heat is greatest?

## How the work runs

1. **Data and checks.** Pull the open sources, run the data rules D1–D6 and
   clean the data by C1.
2. **Exposure.** Weight station temperature by where people live.
3. **Heat over time.** H1: hot days and tropical nights since 2000.
4. **Checks on the method.** B1 reproduces RKI's Hessen figures, and M1 and
   M2 test the model's predictions. B1 comes before anything about deaths.
5. **Heat and deaths.** H2: hot nights, on the daily deaths. H3: the change in
   sensitivity.
6. **Heat in the cities.** E1 and E2 explore three of the five cities in the
   summers 2018–2021 and lead to at most two follow-ups, F1 and F2. The other
   two cities and the summers 2022–2025 stay closed until then. Then
   everything is opened: H4 (the cities against their surroundings), H5 (rent)
   and H6 (older residents) are tested in all five cities, and the follow-ups
   in the two held out.
7. **Reporting.** Yearly figures, maps and the README, with exploratory results
   labelled as such.

*Restructured on 02.11.2026: the two tests of the method are now the checks
M1 and M2, and the hypotheses are claims about Hessen, numbered in the order
of the work. Revised after review the same day: two cities held out as well
as four summers, 2 km blocks, H2 on the daily deaths, one fixed set of
stations, three outcomes for every test, an exact threshold rule, and tropical
nights read over DWD's night window.*

## What is known before the rules

Everything the design draws on, and everything seen before the rules were
fixed, is listed here with its source, so that each idea can be traced and
credited.

- **RKI's estimates.**
  - Germany's heat-related deaths were about 3,100 in 2023 (95% prediction
    interval 1,400–4,900) and 2,800 in 2024 (1,000–5,000), from a model of
    weekly deaths.
  - An appendix gives every federal state for 1992–2024 (an der Heiden, 2025).
  - For 2026, RKI estimates about 16,000 (14,400–17,600) between 6 April and
    13 September, the most since 1992 (Apotheken Umschau).
  - The method section was read on 02.11.2026, for the model and its heat
    threshold. The Hessen figures in the appendix have not been opened.
  - Two news headlines, seen on 02.11.2026 while checking the project's name,
    give RKI figures for Hessen: about 2,200 heat deaths in a year the headline
    does not name, and 1,350 so far in 2026 (hessenschau). The articles were
    not opened.
- **Heat in Hessen.** The annual mean temperature was 8.23 °C in 1961–1990 and
  9.26 °C in 1991–2020, and warm and hot days have become much more frequent
  (Umweltbundesamt, 2025).
- **Definitions.**
  - A hot day has a maximum air temperature of 30 °C or more (DWD).
  - A tropical night has a minimum air temperature of 20 °C or more, read
    between 18 and 06 UTC (DWD).
  - The heat-wave day, three or more hot days in a row, is chosen here, as
    definitions vary.
  - DWD's glossary entries were read on 02.11.2026, from screenshots supplied
    by the author.
- **Heat at DWD's stations, from the same entries.**
  - At most DWD stations, tropical nights average fewer than one a year. The
    most are at Heidelberg: 3.3 a year in 1961–1990 and 4.5 in 1981–2010.
  - Frankfurt/Main-Westend had 12 tropical nights in 2003 and again in 2015.
  - Frankfurt am Main averaged 13 hot days a year in 1981–2010.
  - The entries' other figures are for stations outside Hessen or for years
    before 2000.
- **Risk.** The title uses "risk" as the IPCC does: risks result from the
  interaction of climate-related hazards with the exposure and vulnerability
  of the people affected (IPCC, 2022). Here the hazard is the heat (H1, H4),
  the vulnerability is how deadly it is and whether that has changed (H2, H3),
  and the exposure is who lives in it (H5, H6).
- **Earlier studies.**
  - Across several countries, heat risk fell between 1993 and 2006 in most,
    but not all, of them (Gasparrini et al., 2015).
  - In Switzerland, after allowing for the day's mean temperature, nights of
    20 °C or more went with higher mortality in some cantons, lower mortality
    in others and no change in the rest (Rippstein et al., 2023).
  - Thermal remote sensing has long shown city surfaces warmer than their
    surroundings (Voogt & Oke, 2003).
  - In most of 25 cities worldwide (72%), poorer neighbourhoods had more
    surface heat (Chakraborty et al., 2019).
- **Data limits.**
  - Deaths are published weekly only down to the federal state (Destatis, the
    source used here). Below that, Hessen's district deaths are published by
    year, not by week (Hessisches Statistisches Landesamt, 2025). So no test
    uses district deaths, and the district map is modelled.
  - DWD's description of its daily data does not say over which hours the
    daily minimum is read. Its station metadata record changes of reference
    time, and many stations switched from manual to automatic measurement
    between the late 1990s and 2009 (DWD, daily station data). So tropical
    nights are counted from hourly readings (D4).
- **The death counts, opened on 01.11.2026 for D1 only.** Completeness was
  checked, along with the yearly totals and how the tables agree with one
  another (D1, D2). No week has been set against temperature.
- **Not yet looked at:** the station series beyond DWD's figures above, the
  Zensus cells and the Landsat scenes for Hessen.

## Data, all open

| Source | What | Resolution |
|---|---|---|
| Destatis | Deaths in Hessen by age group (0–64, 65–74, 75–84, 85+), every calendar week 2000–2025, final; 2026 provisional to week 39. Daily deaths, all ages, 2000–2025. In `raw/`, with hashes in `raw/manifest.csv` | Week; day |
| Hessian State Statistical Office | Population by single year of age, per district, yearly. The figures switch from the 2011 to the 2022 census basis | Year |
| DWD Climate Data Center | Daily mean, maximum and minimum air temperature (TMK, TXK, TNK), and hourly air temperature (TT_TU) for the tropical nights, at the stations in and around Hessen | Day; hour |
| Zensus 2022 | Residents, the share aged 65+ and the average net cold rent per m² per grid cell | 100 m |
| BKG, VG250 | Hessen, its 26 districts and the five district-free cities | 1:250,000 |
| USGS, Landsat 8 and 9, Collection 2 Level-2 | Cloud-free summer scenes (June to August) 2018–2025 from Microsoft's Planetary Computer catalogue: daytime surface temperature (`lwir11`), plus red and near-infrared reflectance for a vegetation index | 30 m |
| RKI (the benchmark) | Heat-related deaths by federal state, 1992–2024 | Year |

DWD's data are under CC BY 4.0, VG250 under dl-de/by-2-0, and RKI's appendix
under CC BY 3.0 DE. The Zensus grid is open data with attribution.

## Data rules

- **D1, the deaths: passed on 01.11.2026.** Hessen's deaths are present in all
  four age groups for every calendar week 2000–2025, all final. The daily
  deaths are present for every day 2000–2025. If D1 had failed, H3 would have
  been dropped rather than tested on other years.
- **D2, checks before any model runs.**
  - Every week must have a number for all four age groups. `X` may appear only
    in a week 53 that does not exist that year.
  - The age groups, and the days of each week, must each match the published
    weekly total to within 1%.
  - A failure stops the run.
  - The 1% was set after D1 had shown the gaps. Up to 2024 the tables agree
    exactly. In 2025, the age groups miss the weekly total in 35 weeks, and the
    days miss it in 41 weeks, by at most 8 deaths in a week (about 0.6%). The
    days also sum to 12 more than the published year total. The workbook does
    not say why; differences in both directions suggest each table was
    adjusted separately.
- **C1, cleaning.**
  - Counts are used as published, never rescaled or filled in, so they stay
    whole numbers for the Poisson model.
  - Where a total over ages is needed, it is the sum of the four groups, so the
    series adds up by construction. The published weekly total serves only D2.
  - The weekly series by age feeds B1, M1, M2 and H3. The daily series feeds H2
    and the robustness model. The two are never mixed in one model.
  - The published year total is not used.
- **D3, provisional data.** 2026, to week 39, is reported and flagged as
  provisional. It enters no test and no check against RKI.
- **D4, stations.**
  - One fixed set of DWD stations is used throughout: those with data for at
    least 95% of the days, and for at least 95% of the nights, in every year
    2000–2025. A night counts as read if at least 11 of its 13 hourly readings
    from 18 to 06 UTC are present.
  - Each cell is linked to its nearest station in the set once, so no cell
    switches station between years. The number of stations left out is
    reported.
  - Each station's recorded changes within 2000–2025 (site, instruments,
    automation, reference time) are listed from its metadata.
  - DWD's gridded daily temperature, HYRAS (DWD, HYRAS-DE), was considered and
    not used. It is interpolated from the stations reporting each day, so it
    can shift as the network changes, and it smooths the city heat that
    tropical nights depend on.
- **D5, cells.**
  - A cell's surface temperature counts only if the cell has at least three
    cloud-free summer scenes in each period it is used for.
  - Tests about residents (H5, H6, E1, E2 and the follow-ups) also need at
    least 20 residents in the cell, and H5 and E2 a rent value.
  - H4 compares surfaces scene by scene, so it uses every land cell that is
    clear in a scene.
  - Cells left out are counted per city, never filled in.
- **D6, tropical nights against DWD's own count.** At Frankfurt/Main-Westend,
  the tropical nights counted here must come within one night of the 12 that
  DWD reports for 2003, and of the 12 for 2015. A larger gap is found and
  stated before H1 and H2 are reported. If the station has no hourly readings
  for those years, that is stated instead.

## Exposure and models

- **Exposure, weighted by where people live.** Each populated 100 m cell takes
  the temperature of its station in the fixed set (D4). Hessen's daily
  temperature is the mean over cells, weighted by residents, then averaged to
  weeks. This weighted series is the main one. RKI averages stations per state,
  so the plain mean over the same stations is computed too. B1 uses it, as RKI
  does, and M2 compares the two.
- **The mortality model.** It follows RKI's method, for comparison (an der
  Heiden, 2025). Weekly deaths in calendar weeks 15–40 are fitted with a
  quasi-Poisson additive model. Its terms are a smooth function of weekly mean
  temperature and of the three weeks before it, a long-term trend and the
  season. As in RKI's method, the trend may bend more from 2020, for the
  pandemic. B1, H3 and the yearly heat deaths are fitted per age group; M1
  and M2 use residents aged 65+, the sum of the three groups. The fit uses
  statsmodels.
- **Heat deaths and the threshold.** Heat deaths are the modelled deaths minus
  the deaths with every week capped at the heat threshold. The threshold is
  RKI's delayed-risk temperature, near 20 °C in RKI's estimates. Here it is
  found in each fitted model, per age group and decade, by a fixed rule:
  - the reference is the weekly temperature of lowest risk, with the risk
    summed over the week and the three before it, searched between the 1st and
    99th percentiles of the model's weekly temperatures;
  - against that reference, the relative risk of the week itself and that of
    the week after are read on a 0.1 °C grid;
  - the threshold is the lowest temperature above the reference from which
    both stay above 1, up to the 99th percentile;
  - if there is none, that model finds no heat deaths.
- **The prediction models (M1, M2).** The model is fitted on 2000–2019, with
  the population aged 65+ as an offset and a linear trend so it can
  extrapolate. It then predicts weeks 15–40 of 2023–2025, which lie after the
  pandemic waves, and is scored on the summer weeks 23–35, where temperature
  can make a difference.
- **The daily model (H2).** The daily deaths, all ages, in weeks 15–40 are
  fitted with a quasi-Poisson distributed lag non-linear model (Gasparrini et
  al., 2010). Its terms are the daily maximum temperature over the day and the
  three before it, a smooth function of the day of the year, a long-term trend
  and the day of the week. H2 adds the tropical nights over the same four days.
- **A robustness check.** The daily model, with the daily mean temperature in
  place of the maximum, is fitted on its own and set beside the weekly model.
  It is reported only.
- **Uncertainty.** 10,000 draws of the model's coefficients from their
  estimated covariance give a 90% interval for every figure. The point estimate
  must lie inside its interval, and the interval must move by less than 1% of
  its width at 20,000 draws, as in Project A. All random draws (the coefficient
  draws, the bootstraps and the choice of held-out cities) use the seed
  20261102.

## Where geopandas carries the work

1. **Boundaries.** Hessen, its districts and the five cities come from VG250.
   They are reprojected to ETRS89-LAEA (EPSG:3035), the Zensus grid's system.
2. **The population grid.** The Zensus cells become 100 m squares, clipped to
   Hessen by a spatial join. Each holds its residents, its 65+ share and its
   rent per m².
3. **Exposure weights.** `sjoin_nearest` links every cell to a station in the
   fixed set. The table of weights is kept, so the weekly exposure can be
   recomputed and checked.
4. **Surface temperature and green cover per cell.** Each scene is loaded from
   the STAC catalogue (stackstac) and masked for cloud. Within a scene, a
   cell's surface temperature is taken as its difference from its city's mean
   in that scene, so that cells seen on different days stay comparable. These
   differences, and a vegetation index, are averaged over every cell (zonal
   statistics), per period. Within each city, the cells are ranked into tenths
   by surface temperature (H6), and the residents are split into quarters by
   rent per m² (H5).
5. **City rings.** Each city's boundary is buffered 5 km outward. The city, the
   other four cities and water are removed, for H4.
6. **Spatial statistics.** PySAL works on the cells' neighbour weights. In the
   three explored cities, it finds E1's hot spots (Getis–Ord Gi*, `esda`) and
   fits E2's spatial regression (`spreg`).
7. **The test units.** Within each city, cells are grouped into 2 km blocks,
   each of four 1 km parent cells, for the bootstraps of H5, H6 and the
   follow-ups. The same tests on 1 km and 4 km blocks are shown beside them.
   Cells are also dissolved into districts for the map.
8. **The output.** A GeoPackage holds cells and districts with exposure,
   surface temperature, green cover, rent and age shares. The maps are drawn
   from it.

## Checks on the method

These test the model, not Hessen. They are fixed now, and each is reported
whatever it shows.

- **B1. The model reproduces RKI's Hessen estimates.**
  - *Test.* Over the years both cover, 2000–2024, the yearly heat deaths
    correlate with RKI's at r ≥ 0.8, and their total lies within 25% of RKI's.
  - The correlation mostly shows that the hot summers line up; the total is the
    stricter test.
  - B1 is the gate for everything about deaths. If it fails, two days go into
    finding the difference, as with Project A's cross-checks. If it is still
    unexplained, the results about deaths are reported, labelled "benchmark not
    met".
- **M1. The model predicts years it has not seen.**
  - *Test.* Fitted on 2000–2019, it predicts weekly deaths among residents aged
    65+ in 2023–2025 with a lower mean absolute error than the same model
    without temperature, scored on the summer weeks 23–35. The 90% interval of
    the difference, from a bootstrap over 4-week blocks, lies below zero.
  - Neither window contains 2020–2022.
- **M2. Weighting by where people live improves the prediction.**
  - *Test.* On the same windows, the model with the population-weighted
    temperature beats the same model with the plain station mean, by the same
    measure as M1.
  - Hessen's stations stand mostly where people live, so the two series may
    hardly differ and this can fail. RKI's simpler approach would then be
    enough.
- **M1 and M2 are not gates.** A failed M1 is stated beside H2 and H3.

## Hypotheses about Hessen

Each is one claim about Hessen, with its test underneath, and each stands on
its own. They follow the risk in the title: the hazard (H1, H4), how deadly it
is (H2, H3) and who is exposed (H5, H6). A one-line question leads into each.
Every question is asked on its own, never as "if so", so an early failure stops
nothing after it.

- **Three outcomes, set in advance.** A claim is supported when its 90%
  interval lies wholly on the claimed side, contradicted when it lies wholly on
  the other side, and inconclusive when it spans both. An inconclusive result
  is never reported as "no effect".
- **Claims with parts** (H1, H4) are supported only if every part is, and
  contradicted if any part is.
- **The same three outcomes** apply to M1, M2 and the follow-ups.
- **Every result is reported.** If none of the six claims were true, the chance
  that at least one would be supported anyway is up to about one in four (six
  tests, each one-sided at 5%). The README says so beside the results.
- **Worded as effects.** H2 and H3 speak of heat raising deaths, as time-series
  studies of weather and health usually do: once season and long-term trend
  are allowed for, short-term swings in weather are largely unrelated to the
  other short-term causes of death (Bhaskaran et al., 2013). Air pollution,
  which rises with heat, is not adjusted for here, so part of any effect may
  run through it.

### Heat over time

*Is summer heat rising where people live?*

- **H1. Hot days and tropical nights have become more frequent in Hessen since
  2000, at the places where people live.**
  - *Test.* For each year 2000–2025, count the hot days (maximum ≥ 30 °C) and
    the tropical nights (minimum ≥ 20 °C between 18 and 06 UTC) at each
    populated cell's station, averaged over residents. For each count, the 90%
    interval of Sen's slope (Sen, 1968) lies above zero. H1 is supported if
    both do.
  - *Can fail:* 26 years is short against the swings between summers, and at
    most DWD stations tropical nights average fewer than one a year (DWD).
  - Heat-wave days (three or more hot days in a row) are reported beside H1,
    not tested.

### Heat and deaths

*How deadly is the heat, and do hot nights add to it?*

- **H2. Hot nights add deaths in Hessen beyond what hot days explain.**
  - *Test.* The daily model, all ages, weeks 15–40 of 2000–2025, adds the share
    of residents whose station recorded a tropical night (minimum ≥ 20 °C
    between 18 and 06 UTC), over the day and the three before it. A night
    counts for the day it ends on. The 90% interval of the rate ratio for a
    tropical night for every resident against none, summed over the four days,
    lies above 1.
  - Refitted without 2020–2022. If the two disagree, that is stated with the
    result.
  - Daily deaths are not split by age, so H2 covers all ages. Most deaths are
    at 65 and over.
  - *Can fail:* hot days and hot nights still come together, tropical nights
    are rare at most stations (DWD), and in Switzerland the effect
    differed in sign between cantons (Rippstein et al., 2023).

*Is the same hot week now less deadly for older people than in the 2000s?*

- **H3. Residents aged 65+ have grown less sensitive to heat: the same hot week
  raises their deaths less in 2015–2024 than in 2000–2009.**
  - *Test.* The mortality model is fitted to each decade and to each age group
    (65–74, 75–84, 85+). In each, take the relative risk of death at the 95th
    percentile of weekly temperature against the median, summed over the week
    and the three before it. Both percentiles come from weeks 15–40 of
    2000–2024, pooled. Each group's ratio, later decade over earlier, is
    combined on the log scale with fixed weights: the group's share of deaths
    at 65+ over 2000–2024. The 90% interval of the combined ratio lies below 1.
  - Each age group's ratio is reported.
  - Refitted without 2020–2022. If the two disagree, that is stated with the
    result.
  - H1's counts show how much more heat people faced in the later decade.
  - *Can fail:* across countries the risk fell in most but not all (Gasparrini
    et al., 2015), and a decade holds only a handful of very hot weeks, so the
    interval may be wide.

### Heat in the cities

The cities are Hessen's five district-free cities: Darmstadt, Frankfurt am
Main, Kassel, Offenbach am Main and Wiesbaden. These hypotheses use all five
cities and the summers 2018–2025, and are tested only once everything is
opened (see the held-out rule below).

*Are the cities hotter than the land around them?*

- **H4. Each of the five cities has a hotter surface by day in summer than the
  land around it.**
  - *Test.* For each summer scene, take the city's mean surface temperature
    minus that of its ring: the 5 km band outside the city, without the other
    four cities. Water, the cells with a vegetation index below zero, is left
    out of both. A scene counts where at least 80% of both the city and its
    ring are clear. In each city, the 90% interval of the mean difference lies
    above zero, from a bootstrap that resamples whole summers and then the
    scenes within them. H4 is supported if it does in all five.
  - *Can fail:* harvested fields heat up strongly by day, Frankfurt's city
    forest lies inside its boundary, and Wiesbaden's ring holds Mainz.

*Who lives in the heat: are the places with cheaper rent hotter?*

- **H5. Within the five cities, among places with rented homes, those with
  cheaper rent are hotter.**
  - *Test.* In each city, split the residents of cells with a rent value into
    quarters by that rent per m². Pooled over the five cities, weighted by
    residents, the cheapest quarter lives on hotter ground than the dearest.
    The 90% interval of the difference in surface temperature, from a
    bootstrap over 2 km blocks within each city, lies above zero. The same
    test on 1 km and 4 km blocks is shown beside it.
  - Each city's difference is reported, with the share of its residents living
    in cells that have a rent value. Owner-occupied homes have no rent, so
    places with few rented homes fall outside H5.
  - *Can fail:* rents are often highest in the centres, which are also the
    hottest.

*And are older people more often in the hottest places?*

- **H6. Within the five cities, residents aged 65+ are over-represented in the
  hottest places.**
  - *Test.* In each city, take the hottest tenth of populated cells by mean
    summer surface temperature. Pooled over the five cities, weighted by
    residents, residents aged 65+ make up a larger share of the people there
    than of the city as a whole. The 90% interval of the difference, from a
    bootstrap over 2 km blocks within each city, lies above zero. The same test
    on 1 km and 4 km blocks is shown beside it.
  - Each city's difference is reported.
  - *Can fail:* the hottest cells are often centres and industrial areas.

## Exploration in the cities (E1, E2)

- **The held-out rule.**
  - Two of the five cities are held out: Kassel and Wiesbaden. They were drawn
    on 02.11.2026, before any scene was opened. Each city, in alphabetical
    order, took the next number from Python's `random.Random(20261102)`, and
    the two lowest were held out.
  - E1 and E2 use the other three cities, Darmstadt, Frankfurt am Main and
    Offenbach am Main, in the summers 2018–2021 only. The held-out cities, and
    the summers 2022–2025, stay closed until F1 and F2 are written, dated and
    committed.
  - Anything the README calls a finding from E1 or E2 must have passed as F1
    or F2. Everything else is labelled exploratory.
  - H4–H6 are fixed before any scene is seen, so they use all five cities and
    all eight summers. They are tested only once everything is opened.
- **E1, hot spots.** Within each explored city, where surface heat clusters
  among the populated cells (Getis–Ord Gi*; Getis & Ord, 1992).
- **E2, what goes with surface heat.** Within the explored cities, how surface
  temperature varies with green cover, rent per m² and the 65+ share together.
  It uses a spatial regression, so neighbouring cells don't count as
  independent evidence.
- **F1, F2, the follow-ups.** At most two, written from E1 and E2 and dated
  before anything held out is opened.
  - Each must be able to hold in cities it was not found in: a claim about land
    use, green cover, rent or age, not about a named place.
  - Each is a single test in Kassel and Wiesbaden, in the summers 2022–2025,
    decided by a 90% interval, on 2 km blocks where the test is spatial.
  - If the exploration points nowhere, there are none.

## What is reported, and how

- **Reported, not tested:**
  - heat deaths in Hessen for every year 2000–2025 with their intervals, 2025
    going beyond RKI's report;
  - 2026, weeks 15–39, flagged as provisional;
  - heat-wave days per year, beside H1;
  - E1 and E2, labelled as exploratory;
  - the daily robustness model;
  - the district map. It shows the share of residents aged 65+ living in hot
    cells. It also shows heat deaths per district, modelled from Hessen's model
    with each district's temperature and age structure, and labelled as
    modelled.
- **Every check and every hypothesis is reported with its outcome, whatever it
  is.**
- **Stated with every figure:**
  - these are associations in observational data, not the causes of
    individual deaths;
  - the mortality models are weekly, except H2's, which is daily;
  - Landsat measures the surface, not the air, at a mid-morning overpass;
  - a cell's share describes the cell, not the people in it.

## Three weeks

1. **Days 1–2.** Pull population, DWD stations, Zensus grid, VG250 and RKI's
   appendix, then run D2 to D4 and D5's resident count.
2. **Days 3–4.** Build the grid, the station weights and the weekly exposure,
   and set them against the plain station mean. Count the tropical nights from
   the hourly readings and run D6.
3. **Days 5–6.** H1, with the heat-wave days.
4. **Days 7–8.** Reproduce RKI's model and check B1.
5. **Days 9–11.** M1, M2 and H3 on the weekly deaths; H2 and the robustness
   model on the daily deaths.
6. **Days 12–15.** Landsat 2018–2021 for Darmstadt, Frankfurt am Main and
   Offenbach am Main, with D5's scene count, then E1 and E2. Then F1 and F2 are
   written, dated and committed.
7. **Days 16–18.** Open the rest: all five cities, 2018–2025, with D5's scene
   count. Then test H4, H5 and H6, and F1 and F2 in Kassel and Wiesbaden.
8. **Days 19–21.** Maps, figures and README.

If the work runs late, it is cut in this order: the daily robustness model,
then E1, then the follow-ups. B1, M1, M2 and H1–H6 are never cut. Whatever is
cut is listed in the README under "What is not built".

## Confirmed on 02.11.2026

The author confirmed these ten points on 02.11.2026, with the revisions noted
below the list.

1. The question in four parts: the heat itself, the deaths, the change over
   time, and who lives where the heat is greatest.
2. The order of the work: heat over time, the checks on the method, heat and
   deaths, then the cities.
3. The data rules:
   - the 1% tolerance;
   - one fixed set of stations, each with 95% of its days and of its nights
     in every year 2000–2025, and each cell linked once;
   - tropical nights read from hourly readings between 18 and 06 UTC, as DWD
     defines them, and checked against DWD's count for Frankfurt/Main-Westend
     (D6);
   - three clear scenes per period for a cell, and 20 residents where a test
     concerns residents.
4. C1: counts as published, totals as the sum of the age groups; the weekly
   series for B1, M1, M2 and H3, the daily series for H2 and the robustness
   model, never mixed.
5. The checks: B1's bar (r ≥ 0.8, the total within 25%) as the gate for
   everything about deaths, with two days to explain a gap; M1 and M2 scored
   on the summer weeks, reported, not gates.
6. H1: hot days and tropical nights 2000–2025, both slopes above zero.
7. H2 and H3:
   - H2 on the daily deaths, all ages: hot nights beyond hot days, over the
     day and the three before it;
   - H3 on residents aged 65+, each age group fitted on its own and combined
     with fixed weights: a week at the 95th percentile against one at the
     median, 2015–2024 against 2000–2009;
   - each refitted without 2020–2022.
8. H4–H6 in all five cities, on the summers 2018–2025:
   - each city against its 5 km ring, resampling whole summers;
   - the cheapest rent quarter against the dearest;
   - the 65+ share in the hottest tenth;
   - H5 and H6 on 2 km blocks, with 1 km and 4 km shown beside them.
9. The exploration: E1 and E2 in Darmstadt, Frankfurt am Main and Offenbach am
   Main on 2018–2021; at most two follow-ups, tested in Kassel and Wiesbaden on
   2022–2025.
10. Six hypotheses, each standing on its own, each ending supported,
    contradicted or inconclusive, and all reported.

*Revised on 02.11.2026 after the review: items 3–5 and 7–10, the threshold
rule, H5's wording and the cut order. Item 3 was revised again the same day,
for DWD's night window.*

## Amendments

Changes after the first commit are added here, each with its date and reason.
The text above is not edited.

None yet.

## References

an der Heiden, M. (2025). Hitzebedingte Mortalität in Deutschland 2023 und 2024.
*Epidemiologisches Bulletin*, 19/2025, 3–9. https://doi.org/10.25646/13135

Apotheken Umschau. On RKI's estimate of heat deaths in 2026.
https://www.apotheken-umschau.de/gesund-bleiben/klima/hitze/hitzetote-2026-1553029.html

Bhaskaran, K., Gasparrini, A., Hajat, S., Smeeth, L., & Armstrong, B. (2013).
Time series regression studies in environmental epidemiology. *International
Journal of Epidemiology*, 42(4), 1187–1195. https://doi.org/10.1093/ije/dyt092

Chakraborty, T. C., Hsu, A., Manya, D., & Sheriff, G. (2019). Disproportionately
higher exposure to urban heat in lower-income neighborhoods: a multi-city
perspective. *Environmental Research Letters*, 14(10), 105003.
https://doi.org/10.1088/1748-9326/ab3b99

Deutscher Wetterdienst. Glossar: Heißer Tag, and Glossar: Tropennacht. Read on
02.11.2026 from screenshots supplied by the author.
https://www.dwd.de/DE/service/lexikon/Functions/glossar.html?lv3=101162&lv2=101094
and
https://www.dwd.de/DE/service/lexikon/Functions/glossar.html?lv2=102672&lv3=102802

Deutscher Wetterdienst. HYRAS-DE, gridded daily data for Germany: description
of the 1 km grid, version 6.
https://opendata.dwd.de/climate_environment/CDC/grids_germany/daily/hyras_de/humidity/DESCRIPTION_HYRAS-DE-HURS_V6_en.pdf

Gasparrini, A., Armstrong, B., & Kenward, M. G. (2010). Distributed lag
non-linear models. *Statistics in Medicine*, 29(21), 2224–2234.
https://doi.org/10.1002/sim.3940

Gasparrini, A., Guo, Y., Hashizume, M., Kinney, P. L., Petkova, E. P.,
Lavigne, E., Zanobetti, A., Schwartz, J. D., Tobias, A., Leone, M., Tong, S.,
Honda, Y., Kim, H., & Armstrong, B. G. (2015). Temporal variation in
heat–mortality associations: a multicountry study. *Environmental Health
Perspectives*, 123, 1200–1207. https://doi.org/10.1289/ehp.1409070

Getis, A., & Ord, J. K. (1992). The analysis of spatial association by use of
distance statistics. *Geographical Analysis*, 24(3), 189–206.
https://doi.org/10.1111/j.1538-4632.1992.tb00261.x

hessenschau. Headlines only, seen on 02.11.2026 and not opened: "Opposition
fordert Sofortprogramm: Rund 2.200 Hitzetote in Hessen – wie reagiert die
Landesregierung?"
https://www.hessenschau.de/politik/rki-meldet-fast-2200-hitzetote-in-hessen---wie-reagiert-die-landesregierung-v1,hitze-tote-102.html
and "Hitzesommer 2026. RKI: Bereits 1.350 Hitzetote in Hessen in diesem Jahr"
https://www.hessenschau.de/panorama/robert-koch-institut-bereits-1350-hitzetote-in-hessen-in-diesem-jahr-v1,hitzetote-hessen-100.html

Hessisches Statistisches Landesamt (2025). *Bevölkerungsvorgänge in Hessen im
Jahr 2024*. Statistischer Bericht A II 1 – j/24.
https://statistik.hessen.de/sites/statistik.hessen.de/files/2025-07/aii1_j24.pdf

IPCC (2022). Annex I: Global to Regional Atlas [H.-O. Pörtner, A. Alegría,
V. Möller, E. S. Poloczanska, K. Mintenbeck & S. Götze (eds.)]. In *Climate
Change 2022: Impacts, Adaptation and Vulnerability. Contribution of Working
Group II to the Sixth Assessment Report of the Intergovernmental Panel on
Climate Change* (pp. 2811–2896). Cambridge University Press.
https://doi.org/10.1017/9781009325844.028

Rippstein, V., de Schrijver, E., Eckert, S., & Vicedo-Cabrera, A. M. (2023).
Trends in tropical nights and their effects on mortality in Switzerland across
50 years. *PLOS Climate*, 2(4), e0000162.
https://doi.org/10.1371/journal.pclm.0000162

Sen, P. K. (1968). Estimates of the regression coefficient based on Kendall's
tau. *Journal of the American Statistical Association*, 63(324), 1379–1389.
https://doi.org/10.2307/2285891

Umweltbundesamt (2025). *Regionale Klimafolgen in Hessen*. Last updated
21.01.2025.
https://www.umweltbundesamt.de/themen/klima-energie/klimafolgen-anpassung/folgen-des-klimawandels/klimafolgen-deutschland/regionale-klimafolgen-in-hessen-0

Voogt, J. A., & Oke, T. R. (2003). Thermal remote sensing of urban climates.
*Remote Sensing of Environment*, 86(3), 370–384.
https://doi.org/10.1016/S0034-4257(03)00079-8

The software (geopandas, PySAL, statsmodels, stackstac) is credited in the
README with the versions used.

Data:

- [Destatis, deaths 2000–2020](https://www.destatis.de/static/DE/dokumente/5126108209005_SB.xlsx) and [2021–2026](https://www.destatis.de/DE/Themen/Gesellschaft-Umwelt/Bevoelkerung/Sterbefaelle-Lebenserwartung/Tabellen/sonderauswertung-sterbefaelle.html)
- [Hessian State Statistical Office, population by age](https://statistik.hessen.de/sites/statistik.hessen.de/files/2026-07/ai6_j25.pdf)
- [DWD, daily station data](https://opendata.dwd.de/climate_environment/CDC/observations_germany/climate/daily/kl/DESCRIPTION_obsgermany-climate-daily-kl_en.pdf)
- [DWD, hourly air temperature](https://opendata.dwd.de/climate_environment/CDC/observations_germany/climate/hourly/air_temperature/)
- [Zensus 2022 grid](https://www.destatis.de/DE/Themen/Gesellschaft-Umwelt/Bevoelkerung/Zensus2022/_inhalt.html)
- [BKG, VG250 terms](https://sg.geodatenzentrum.de/web_public/gdz/lizenz/eng/nutzungsbedingungen_vg250_eng.pdf)
- [Planetary Computer, landsat-c2-l2](https://planetarycomputer.microsoft.com/api/stac/v1/collections/landsat-c2-l2)
- [RKI, Epid Bull 19/2025 and its appendix](https://edoc.rki.de/handle/176904/12682)
