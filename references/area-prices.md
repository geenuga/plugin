# Area prices (€/m²)

## First: the official values already in Geenuga

Geenuga keeps an archive of **official values per zone with up to ten years of history** (in Italy the OMI quotations
of the Agenzia delle Entrate, with the official town boundaries; also France — DVF per town and the Carte des loyers
rents —, Portugal — INE per town and freguesia —, Poland — GUS per powiat, in **złoty** —, Austria — Statistik Austria
per district). Outside Italy the value is usually a
**median** (not a range), sometimes of a whole **district**, and measures different things: sale prices of deeds,
mortgage valuations, rents of contracts or of listings (`source.measures`, `source.value`, `source.currency`,
`areaType` in the answers). Tell the family exactly that. Start there:

- `home_zone_prices` without arguments lists the towns of the family's searches and homes that have values; with `town` it gives the town's range, its zones (with
  the OMI code), the history and the change over 1, 5 and 10 years; with `homeId` it compares a home with its zone
  (`home_details` → `officialAreaPrices` too).
- **The home's zone**: when you know which OMI zone the address is in (OMI map, «Ricerca per indirizzo»:
  https://www1.agenziaentrate.gov.it/servizi/geopoi_omi/index.php), save it in the home's `priceZone` with the key
  `home_zone_prices` gives (e.g. `it-A132-B1`). Without it, Geenuga guesses it from the neighbourhood or compares with
  the whole town.
- **A town that is missing** (or another country): look the values up on the official sources of the country
  (`home_searches` → `priceSources`, those with `official: true`), one page at a time, and save them for the family
  with `home_save_zone_prices`: one row per town or zone and period, min and max (a median goes in both), `source` and
  `sourceUrl`. Never portal averages here: those go in `areaPricePerSqm`.

They fill `areaPricePerSqm` when you save a home, and Geenuga uses them for the comparison with the area and the scam check
(more than 15% above the area: expensive; more than 30% below without a reason: suspicious). For a home **for rent** the
value is €/m² **per month** (one decimal, e.g. 11.5).

The sources of the family's country are in `home_searches` → `priceSources`: use those. In the analysis always
write which source you used and of which month.

## Italy

1. **Immobiliare.it Insights** (free, monthly), first choice for `areaPricePerSqm`:
   municipality `https://www.immobiliare.it/mercato-immobiliare/<regione>/<comune>/`, area
   `/<regione>/<comune>/<zona>/`. These are asking prices, not sale prices. It also has rents.
2. **Idealista price reports** (free, monthly):
   `https://www.idealista.it/sala-stampa/report-prezzo-immobile/vendita/<regione>/<provincia>-provincia/<comune>/`
   (for rent `…/affitto/…`). In small municipalities the figure may be old.
3. **OMI quotations of the Agenzia delle Entrate** (free, half-yearly, official): min–max €/m² per zone and condition,
   for sale and for rent, at https://www1.agenziaentrate.gov.it/servizi/Consultazione/ricerca.htm. Usually lower than
   the portals' asking prices.
4. **Declared values of real deeds** (Agenzia delle Entrate, needs SPID): a check the family does themselves.
5. For rent, the **territorial agreements** (canone concordato 3+2) of each municipality give rent bands per zone.

## How to use them

1. Use the figure for the **area** if there is one, otherwise the municipality.
2. Put in `areaPricePerSqm` the portals' average for the area: it is the most consistent comparison with an asking price.
3. In the analysis also give the official range (from `home_zone_prices`) and how the town changed over 5–10 years.
4. Once you have the figure for an area, reuse it for every home of that area in the session.
