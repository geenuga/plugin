# Projects: what the home is for

Every search belongs to a **project**, which says what the home is for. Before searching, analysing or taking stock,
read the project: `home_searches` → `project` (all of them: `home_projects`).

- **`brief`** is the project in the family's own words: it comes before everything else. If it is empty and the project
  is not the usual one, ask for two lines (who will use it, what matters, what would make them say no) and save them
  with `home_set_project`. Only their answers, never invented ones.
- The purpose fields are **the family's preferences**: use them as they are, do not add thresholds of your own.

## `live` — the home to live in

As always: journeys to their places, schools, everyday life, first-home relief (*prima casa*) if the profile has it.

## `holiday` — a holiday home

- **Costs**: it is a second home (Geenuga does the maths): no first-home relief, 9% registration tax or 10% VAT, IMU,
  higher mortgage substitute tax, lower renovation bonus. Banks often lend less (lower LTV): say so if the mortgage is
  tight.
- **Searching**: `setting` (sea, mountain, lake, countryside, city) and `settingMaxWalkMinutes` are real criteria:
  check on the map the walking distance from the beach, the ski lifts or the lake, not the listing's wording («a stone's
  throw from the sea»), and **save it** in the home: `settingWalkMinutes`. `fromPlaceId` + `maxDriveMinutes`: how far
  it is by car from their home; Geenuga works out the time from the journeys (on Saturday), and both count in the match.
- **Analysing**: what the area is like in the season they will use it (`usage`) and off season (closed services,
  transport), costs of a home closed in winter (condominium, utilities, damp), tourist use of the building. With
  `rentWhenUnused` also estimate what it can earn when they are away (as for «rental», short lets).

## `rental` — a home to let

- **Costs**: second home as above; with `cashOnly` leave the mortgage out of the reasoning.
- **Searching**: what matters depends on the tenants (`tenants`): students → university and transport; workers →
  station, hospitals, office areas; families → schools and services; tourists → centre, sea, sights.
- **The rent**: estimate it from similar rental listings nearby (with links) and **save it** in the home with
  `home_update`: `rentEstimate` (per month; for short lets the monthly equivalent = nightly price × nights booked ÷ 12)
  and `rentEstimateNote` (where it comes from). Without it Geenuga uses the middle of the official rent range (OMI).
- **Geenuga does the maths**: `home_rental_yield` gives gross, net and monthly cash flow after the mortgage, with
  second-home costs, empty months, IMU, condominium, maintenance, flat-rate tax for `rentalMode` (21% free market, 10%
  agreed rent, short lets 21% on the first home and 26% on the second) and furnishing in the money put in; pass `rent`,
  `rentalMode`, `vacantMonths` to try scenarios. The return counts in the match against `targetYieldPct`.
- **Analysing**: add a **«How much it earns»** part to the analysis with those numbers (and why that rent);
  - the risks: town and building rules on short lets, real demand, works before letting, unpaid rent. Rates and rules
    must be checked against official sources with their date: if you are not sure, say so and suggest an accountant
    (*commercialista*).
