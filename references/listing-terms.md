# Reading an Italian listing

The fields and their allowed values are in the input schema of `home_save`: follow it, and leave out what you do not
know (never invent; `null` = unknown). Notes on the Italian terms:

- **Locali and camere**: `rooms` (locali) are the main rooms (living room and bedrooms, sometimes an eat-in kitchen), bathrooms
  and kitchenette excluded. Monolocale / bilocale / trilocale / quadrilocale = 1/2/3/4 locali. Count the bedrooms
  (`bedrooms`) from the floor plan and the description.
- **m²**: usually "commerciali" (walls and shares of balconies and cellars included); the walkable area is smaller.
- **Piano**: `"T"` ground floor, `"R"` raised ground floor, `"S"` semi-basement, otherwise the number. "3/5" = 3rd floor
  of 5 → `floor: "3"`, `totalFloors: 5`.
- **Spese condominiali**: always **per month** in Geenuga. If the listing gives them per year, divide by 12.
- **Titolo**: short and human, in the family's language, no shouting capitals.
- **Foto**: **all** the listing's photos (up to 100), links to the large images (not thumbnails), in the listing's
  order, floor plans included. The page often shows only the first few: take the full list from the gallery or the
  page data, and compare the count with the one the portal shows (e.g. «68 foto»). Then `home_save_photos` copies
  them into Geenuga, so they stay even if the listing disappears.
- **Planimetrie** (floor plans): when the portal lists them apart, their links go in `floorPlans`, not in `photos`;
  Geenuga adds them to the photos and marks them as floor plans. The ones you recognise by looking: see
  [photos.md](photos.md).
- **Coordinates**: put `lat`/`lng` when the page gives them (even approximate): Geenuga then computes the driving times
  to the family's places by itself.

## Do NOT save (unless the search includes them)

Which cases to include is up to the family, in the search (`searchRules.alsoInclude` of `home_searches`). When they are
included, save them with the warnings in `redFlags` and explain them in the analysis.

- **Nuda proprietà** (bare ownership: the usufructuary keeps living there, usually for life) — `alsoInclude.bareOwnership`.
- **Quota / quota indivisa** (only a share of the property) and **cessione quote** — `alsoInclude.shares`.
- **Multiproprietà** (timeshare): not a home but a period of the year in one; never saved.
- Anything the family wrote in their deal breakers (`dealBreaker` of the search).

## Flag (in `redFlags` and in the analysis)

- **Asta** (judicial auction): official procedure, no warranty for defects, maybe occupied, 10% deposit. Save it only
  if `alsoInclude.auctions` is on, with `sellerType: "auction"`.
- **Rent to buy / affitto con riscatto**: part of the rent counts towards the price; risk of losing it.
- **Cessione quote**, **diritto di superficie**, **edilizia convenzionata / PEEP**: constraints on price and buyer.
- **Occupato, locato**: not free at the deed.
- **Saldo e stralcio**: the creditor bank must agree.
- **Abusi, sanatoria, condono, senza agibilità**: risk for the mortgage and the deed.
- **Nuova costruzione** from a building company: VAT instead of registration tax → `sellerType: "builder"`.
- **Lavori straordinari già deliberati**: extra cost for the buyer.

## Renting

- `price` is the monthly rent. "Spese incluse" → `utilitiesIncluded: true`.
- `leaseType`: `free_market` (libero, 4+4 years), `agreed_rate` (concordato, 3+2, rent within the municipality's
  bands), `transitional` (transitorio, up to 18 months, with a written reason), `student` (studenti, 6–36 months).
- **Cedolare secca**: chosen by the landlord; for the tenant no registration tax and no inflation increases.
- `depositMonths`: deposit in months (at most 3 by law); `commissionMonths`: agency commission in months + VAT;
  `furnished`: `yes` | `no` | `partial`; `availableFrom`: YYYY-MM-DD.
- Flag: "no residenza" (they cannot move their residence there), "solo studenti" or "solo lavoratori a tempo
  indeterminato", "uso foresteria", short or tourist lets, "senza contratto", "subaffitto".

## The analysis (Markdown, in the family's language)

```markdown
## In short
Two or three sentences: why it may (or may not) suit them, against their criteria and budget.

## Price and area
€/m² of the home against the area (source and month). Room for negotiation? Price drops already?
(For rent: rent per m² per month against the area; money needed to move in; lease type and deposit.)

## Strengths
- …

## To check
- … (damp, extraordinary fees, compliance, noise, exposure…)

## Questions for the agency (or the owner)
1. …

## Anti-scam check
Result (low/medium/high) and why. If high: never send money or documents. For rent, always: no payment before having
seen the home and read the contract.
```

Include what to ask to see: energy certificate (APE), floor plan, condominium fees and minutes, title of provenance.
