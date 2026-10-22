# Area prices (€/m²)

They fill `prezzoMqZona` when you save a home, and Geenuga uses them for the comparison with the area and the scam check
(more than 15% above the area: expensive; more than 30% below without a reason: suspicious). For a home **for rent** the
value is €/m² **per month** (one decimal, e.g. 11.5).

The sources of the family's country are in `casa_leggi_ricerche` → `fontiPrezzi`: use those. In the analysis always
write which source you used and of which month.

## Italy

1. **Immobiliare.it Insights** (free, monthly), first choice for `prezzoMqZona`:
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
2. Put in `prezzoMqZona` the portals' average for the area: it is the most consistent comparison with an asking price.
3. In the analysis also give the OMI range when you looked it up.
4. Once you have the figure for an area, reuse it for every home of that area in the session.
