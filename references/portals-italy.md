# Italian property portals: how to search

Use this only when the family's country is Italy (`home_overview` → `market`). For any other country see
[other-countries.md](other-countries.md).

The patterns below are shortcuts, checked in October 2026. After every page load **check the title, the number of
results and the cards**: if a filter had no effect, set it from the site's interface and read the URL the site
generates. When a search URL works, save it on the portal with `home_set_portal` (`searchUrl`): next time you
start from there. A saved `searchUrl` (in `home_searches` → `portals`) always wins over these patterns. Respect
the `notes` the family wrote on each portal.

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
- **All the photos**: in the page data (`<script id="__NEXT_DATA__">`), `props.pageProps.detailData.realEstate.properties[]`
  → `multimedia.photos[]` and `multimedia.floorplans[]`, in the listing's order. Use `urls.large`
  (`https://pwm.im-cdn.it/image/<id>/xxl.jpg`, floor plans `https://pic.im-cdn.it/plan/<id>/xxl.jpg`). The "N foto" on
  the page sometimes counts fewer items than the data: the data wins. "Non è presente sul nostro sito o non è più
  disponibile" = removed.
- Since April 2026 many Immobiliare.it agency listings also appear on Subito: expect duplicates.

### Idealista

- Municipality `https://www.idealista.it/vendita-case/<comune>-<provincia>/` (e.g. `bologna-bologna`), area
  `/vendita-case/<comune>/<zona>/`. Filters in the path after `con-`, comma-separated: `prezzo_350000` (max),
  `dimensione_80` (min m²), `trilocali-3`, `terrazza`, `pubblicato_ultima-settimana`. Example:
  `https://www.idealista.it/vendita-case/bologna-bologna/con-prezzo_320000,dimensione_75,trilocali-3/?ordine=pubblicazione-desc`
- No private-only filter: `?ordine=da-privati-asc` puts private sellers first.
- Listing: `https://www.idealista.it/immobile/<id>/`. Phishing campaigns use the idealista name: never trust emails with
  QR codes or links outside `idealista.it`.
- **All the photos**: the gallery loads only a few, but the page scripts (`adMultimediasInfo`, `fullScreenGalleryPics`)
  list them all. Look for `https://img4.idealista.it/blur/<FORMAT>/0/id.pro.it.image.master/<aa>/<bb>/<cc>/<number>.jpg`
  and keep each `<aa>/<bb>/<cc>/<number>` once, in order of appearance (floor plans included). Save the
  `WEB_DETAIL_TOP-L-L` format (850 px); `WEB_DETAIL` is 1500 px and three times heavier. The "N foto" under the gallery
  may differ by one or two (floor plans). "Ci dispiace, questo annuncio non è più pubblicato" = removed.

### Casa.it

- `https://www.casa.it/vendita/residenziale/<comune>/`, pages `?page=N`; price, m², rooms and private sellers from the
  interface. Listing: `https://www.casa.it/immobili/<8-digit id>/`.

### Subito

- `https://www.subito.it/annunci-<regione>/vendita/appartamenti/<provincia>/<comune>/`, size in the path
  (`/trilocale/`), price `ps` (min) and `pe` (max), pages `&o=2`, private sellers `/da-privati/`. Example:
  `https://www.subito.it/annunci-emilia-romagna/vendita/appartamenti/bologna/bologna/trilocale/da-privati/?pe=320000`
- High scam risk for private listings: no ownership check; brand-new accounts with few listings are a warning sign.
- **All the photos**: in the page HTML, `https://images.sbito.it/api/v1/sbt-ads-images-pro/images/<aa>/<uuid>?rule=…`.
  Only those with the gallery rules (`gallery-desktop-…`, `fullscreen-1x-auto`) belong to the listing;
  `vertical-mini-card-…` are other suggested ads. Save `?rule=gallery-desktop-2x-auto` (1240 px, about 130 KB;
  `fullscreen-1x-auto` comes as a PNG of a few MB). The gallery counter ("1/6") says how many; many listings have at
  most 6. "Questo annuncio non esiste più" = removed.

### Agency networks (only if the family turned the `agency_sites` portal on)

Tecnocasa `https://www.tecnocasa.it/annunci/immobili/<regione>/<provincia>/<comune>.html`, Gabetti
`https://www.gabetti.it/vendita/<comune>`, RE/MAX `https://www.remax.it/vendita-appartamenti/<comune>`,
Professionecasa `https://www.professionecasa.it/vendita/casa/<comune>?uso=abitativo`. Condominium fees are often
yearly there: divide by 12.

### Auctions (only if the search has `includeAuctions`)

The official source is the PVP of the Ministry of Justice (`pvp.giustizia.it`), plus astegiudiziarie.it. Different
procedure (online offer with PEC and digital signature, no warranty for defects, often occupied). No intermediary is
needed; the deposit goes only to the account in the notice of sale. The offer form has a captcha: you stop there.

## For rent (searches with `contract` = `rent`)

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

## Photos already saved and new photos

A home saved some time ago may have its photos in a different size or crop from what the portal gives today (e.g.
600×450 against 850×563 of the same picture): to find the missing ones, **compare the images by looking at them**, not
by file name or bytes. Add only the missing ones, as [listing-terms.md](listing-terms.md) explains (Foto).

## The same property on several portals

Compare municipality and street, m² ±5%, rooms, floor, energy class, fees, price ±5%, the agency reference code,
distinctive phrases of the description, cover photo and floor plan. Geenuga flags the likely duplicates itself
(`possible_duplicate` in `home_save`); when it is the same home, link the other listing with `home_listing_checked`
(`otherListing`) and point out price differences above 3–5% or "private" on one portal and "agency" on another.

## No price

"Trattativa riservata", "Prezzo su richiesta": estimate m² × area €/m² and write it in the analysis as **estimated**
(not in `price`). "Da …" / "a partire da" usually means an auction or a new development: flag it.
