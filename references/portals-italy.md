# Italian property portals: how to search

Use this only when the family's country is Italy (`home_overview` → `mercato`). For any other country see
[other-countries.md](other-countries.md).

The patterns below are shortcuts, checked in October 2026. After every page load **check the title, the number of
results and the cards**: if a filter had no effect, set it from the site's interface and read the URL the site
generates. When a search URL works, save it on the portal with `home_set_portal` (`searchUrl`): next time you
start from there. A saved `urlRicerca` (in `home_searches` → `portali`) always wins over these patterns. Respect
the `note` the family wrote on each portal.

Always sort by **most recent** and stop when you meet listings already in Geenuga (`home_known_links`).

## Summary (for sale)

| Portal | Search by municipality | Private sellers only | Most recent | Page 2 | Listing id |
|---|---|---|---|---|---|
| Immobiliare.it | `/vendita-case/<comune>/` | `/da-privati/` | `criterio=dataModifica&ordine=desc` | `&pag=2` | `/annunci/123309745/` |
| Idealista | `/vendita-case/<comune>-<provincia>/` | no real filter | `?ordine=pubblicazione-desc` | `/lista-2.htm` | `/immobile/28853110/` |
| Casa.it | `/vendita/residenziale/<comune>/` | from the interface | `sortType=` (from the interface) | `?page=2` | `/immobili/54404734/` |
| Subito | `/annunci-<regione>/vendita/appartamenti/<provincia>/<comune>/` | `/da-privati/` | `datedesc` | `&o=2` | `…-653452994.htm` |
| Wikicasa | `/vendita-case/<comune>/` | agencies only | — | `?p=2` | `/annuncio/30059065` |

Names in the path are lowercase with hyphens (`san-lazzaro-di-savena`).

### Immobiliare.it

- `https://www.immobiliare.it/vendita-case/<comune>/` (also `/vendita-appartamenti/`, `/vendita-ville/`). Parameters:
  `prezzoMinimo`, `prezzoMassimo`, `superficieMinima`, `superficieMassima`, `localiMinimo`, `localiMassimo`, `bagni`,
  `pag=N`. Example:
  `https://www.immobiliare.it/vendita-case/bologna/?prezzoMassimo=320000&superficieMinima=75&localiMinimo=3&criterio=dataModifica&ordine=desc`
- Private sellers: `/da-privati/` segment; still check the advertiser label on every card.
- 25 listings per page. Auctions are mixed with sales: recognise them and flag them.
- Listing: `https://www.immobiliare.it/annunci/<9-digit id>/`. Read the visible blocks first (features, costs, energy,
  description, advertiser). Coordinates may be approximate.
- Since April 2026 many Immobiliare.it agency listings also appear on Subito: expect duplicates.

### Idealista

- Municipality `https://www.idealista.it/vendita-case/<comune>-<provincia>/` (e.g. `bologna-bologna`), area
  `/vendita-case/<comune>/<zona>/`. Filters in the path after `con-`, comma-separated: `prezzo_350000` (max),
  `dimensione_80` (min m²), `trilocali-3`, `terrazza`, `pubblicato_ultima-settimana`. Example:
  `https://www.idealista.it/vendita-case/bologna-bologna/con-prezzo_320000,dimensione_75,trilocali-3/?ordine=pubblicazione-desc`
- No private-only filter: `?ordine=da-privati-asc` puts private sellers first.
- Listing: `https://www.idealista.it/immobile/<id>/`. Phishing campaigns use the idealista name: never trust emails with
  QR codes or links outside `idealista.it`.

### Casa.it

- `https://www.casa.it/vendita/residenziale/<comune>/`, pages `?page=N`; price, m², rooms and private sellers from the
  interface. Listing: `https://www.casa.it/immobili/<8-digit id>/`.

### Subito

- `https://www.subito.it/annunci-<regione>/vendita/appartamenti/<provincia>/<comune>/`, size in the path
  (`/trilocale/`), price `ps` (min) and `pe` (max), pages `&o=2`, private sellers `/da-privati/`. Example:
  `https://www.subito.it/annunci-emilia-romagna/vendita/appartamenti/bologna/bologna/trilocale/da-privati/?pe=320000`
- High scam risk for private listings: no ownership check; brand-new accounts with few listings are a warning sign.

### Agency networks (only if the family turned the `agenzie` portal on)

Tecnocasa `https://www.tecnocasa.it/annunci/immobili/<regione>/<provincia>/<comune>.html`, Gabetti
`https://www.gabetti.it/vendita/<comune>`, RE/MAX `https://www.remax.it/vendita-appartamenti/<comune>`,
Professionecasa `https://www.professionecasa.it/vendita/casa/<comune>?uso=abitativo`. Condominium fees are often
yearly there: divide by 12.

### Auctions (only if the search has `includeAuctions`)

The official source is the PVP of the Ministry of Justice (`pvp.giustizia.it`), plus astegiudiziarie.it. Different
procedure (online offer with PEC and digital signature, no warranty for defects, often occupied). No intermediary is
needed; the deposit goes only to the account in the notice of sale. The offer form has a captcha: you stop there.

## For rent (searches with `contract` = `affitto`)

Same portals and rules; the price is the **monthly rent**.

| Portal | Search by municipality | Max rent |
|---|---|---|
| Immobiliare.it | `https://www.immobiliare.it/affitto-case/<comune>/?prezzoMassimo=1000&criterio=dataModifica&ordine=desc` | `prezzoMassimo` |
| Casa.it | `https://www.casa.it/affitto/residenziale/<comune>/` | from the interface |
| Subito | `https://www.subito.it/annunci-<regione>/affitto/appartamenti/<provincia>/<comune>/?pe=1000` | `pe` |
| Idealista | `https://www.idealista.it/affitto-case/<comune>-<provincia>/con-prezzo_1000/?ordine=pubblicazione-desc` | `con-prezzo_N` |

- Very low rents on Subito (99 €, 300 €) are almost always rooms, beds or bait.
- Platforms for students and expats with online payment (HousingAnywhere, Spotahome, Uniplaces): you may point them
  out, never book or pay.
- Short lets (Airbnb, Booking) are not homes to search; a long-term listing that sends you there to "view" or "pay" is
  the fake-landlord scam ([scams.md](scams.md)).
- Agencies ask the tenant for a commission too: record it (`commissionMonths`), do not discard homes only for this.

## The same property on several portals

Compare municipality and street, m² ±5%, rooms, floor, energy class, fees, price ±5%, the agency reference code,
distinctive phrases of the description, cover photo and floor plan. Geenuga flags the likely duplicates itself
(`possibile_doppione` in `home_save`); when it is the same home, link the other listing with `home_listing_checked`
(`otherListing`) and point out price differences above 3–5% or "private" on one portal and "agency" on another.

## No price

"Trattativa riservata", "Prezzo su richiesta": estimate m² × area €/m² and write it in the analysis as **estimated**
(not in `price`). "Da …" / "a partire da" usually means an auction or a new development: flag it.
