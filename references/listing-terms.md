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
- **Foto**: up to 15 links to the large images (not thumbnails), in the listing's order. Then `home_save_photos` copies
  them into Geenuga, so they stay even if the listing disappears.
- **Coordinates**: put `lat`/`lng` when the page gives them (even approximate): Geenuga then computes the driving times
  to the family's places by itself.

## Do NOT save

- **Nuda proprietà** (bare ownership: the usufructuary keeps living there, usually for life).
- **Multiproprietà** (timeshare).
- **Quota / quota indivisa** (only a share of the property).
- Anything the family wrote in their deal breakers (`dealBreaker` of the search).

## Flag (in `redFlags` and in the analysis)

- **Asta** (judicial auction): official procedure, no warranty for defects, maybe occupied, 10% deposit. Save it only
  if the search has `includeAuctions`, with `sellerType: "asta"`.
- **Rent to buy / affitto con riscatto**: part of the rent counts towards the price; risk of losing it.
- **Cessione quote**, **diritto di superficie**, **edilizia convenzionata / PEEP**: constraints on price and buyer.
- **Occupato, locato**: not free at the deed.
- **Saldo e stralcio**: the creditor bank must agree.
- **Abusi, sanatoria, condono, senza agibilità**: risk for the mortgage and the deed.
- **Nuova costruzione** from a building company: VAT instead of registration tax → `venditore: "costruttore"`.
- **Lavori straordinari già deliberati**: extra cost for the buyer.

## Renting

- `price` is the monthly rent. "Spese incluse" → `utilitiesIncluded: true`.
- `leaseType`: `libero` (4+4 years), `concordato` (3+2, rent within the municipality's bands), `transitorio` (up to
  18 months, with a written reason), `studenti` (6–36 months).
- **Cedolare secca**: chosen by the landlord; for the tenant no registration tax and no inflation increases.
- `depositMonths`: deposit in months (at most 3 by law); `commissionMonths`: agency commission in months + VAT;
  `furnished`: `si` | `no` | `parziale`; `availableFrom`: YYYY-MM-DD.
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
