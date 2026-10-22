# Geenuga Auto: sites, details, checks, scams

The real monthly cost, road tax, ownership transfer, APR and the checks that follow from the data are computed by
Geenuga's code (`auto_scheda`, `auto_costi`, `auto_confronta`, `auto_offerte`). Here is what you look at yourself.
Italian rules checked in October 2026; for other countries see [other-countries.md](other-countries.md).

## Sites in Italy

| Site | For what | Watch out |
|---|---|---|
| AutoScout24 | The largest: dealers and private sellers, price rating | low risk |
| Subito (Motori) | Many private sellers, good prices | the most frequent scam listings |
| Automobile.it | Dealers and private sellers, detailed filters | — |
| AutoSupermarket | Dealers only, many "km 0" | — |
| Approved used of the brands (Spoticar, Das WeltAuto, Toyota Approved, Renew…) | Guaranteed used cars | slightly higher prices |
| Online dealers (Brumbrum, Aramis, Carvago) | Home delivery, returns | fixed prices, check the delivery fees |
| Facebook Marketplace | Private sellers nearby | fake profiles: only in person, only links the family gives you |
| AutoUncle, Quattroruote | Market price and valuations | not for buying |

The family's sites are in `auto_leggi_ricerche`. Human pace, one page at a time; stop at CAPTCHAs or login walls
([web-rules.md](web-rules.md)).

## What to collect

Title, link and listing id, who sells (private, dealer, rental) with name and town; make, model, version; new / km 0 /
used; registration year and month; km; price and whether it is negotiable; fuel, gearbox, body; **power in kW** (if
only in hp, `cv`); engine size, doors, seats; kerb weight (for new drivers); Euro class; declared combined consumption
(l/100 km, or kWh/100 km), range and battery; owners; documented services; accidents; warranty; useful equipment;
description; large photos. Follow the input schema of `auto_salva`.

Then what you add: `prezzoMercato` and `fonteMercato`, `redFlags`, `rischioTruffa`, `domande` (questions for the
seller), `analisi`.

## Market price

5–15 identical listings: same model and engine, year ±1, km ±20%, same fuel and gearbox. The median is the market
price. Geenuga reads it: more than 25% below = suspicious; 7–25% below = good deal; ±7% = in line; above = room to
negotiate. A "km 0" is compared with discounted new cars, not with used ones.

## Checks before the viewing (Italy)

- **Km**: fewer than 3,000 a year on a car older than 3 years needs an explanation. The km of the last inspection are
  on the Portale dell'Automobilista ("Verifica ultima revisione", with the plate); the service history counts as much as
  the km.
- **PRA search** (ACI, online with the plate): owners, administrative stops, liens.
- **Registration documents** in the seller's name (ID in hand).
- **Recalls** of the model and known faults.
- **Warranty**: from a dealer to a consumer the legal warranty is 24 months, for used cars reducible to no less than 12.
  From a private seller there is no legal conformity warranty, only hidden defects with short deadlines.
- **Diesel up to Euro 5**: traffic bans in many cities and in the Po valley regions; check the family's area.
- **New drivers** (licence B from 14/12/2024, for 3 years): no cars over 105 kW nor over 75 kW per tonne. Geenuga
  checks it if the profile says so.
- **Imported cars**: documents of origin, registration costs, mileage history of the country of origin.

## Scam signs

Stop (not saved unless the family explicitly asks): price far below market without a reason; seller "abroad",
"military", "transferred"; shipping or a courier offered; top-ups, money transfer, crypto or an "escrow/custody
service"; stolen photos (same car in other listings with another plate or city).

To clarify: deposit before seeing it; only messages, no calls; documents "on the way"; liens mentioned; km 0 with
thousands of km; many owners; accident without invoices.

Payment only at delivery, traceable (instant transfer or a banker's draft verified with the bank), with the ownership
transfer done or signed at that moment.

## Typical questions for the seller

Why are you selling? How long have you had it? Services with invoices or booklet? Accidents or bodywork repairs? When
were tyres and brakes changed? Timing belt done? For hybrids and EVs: battery health and remaining warranty. Can we see
it in daylight, with a cold engine, and take it to a mechanic?
