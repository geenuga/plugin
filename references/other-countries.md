# Outside Italy: the portals of the family's country

Every family has a **country** (set in their Geenuga account, or with `home_set_country` when they ask you). It
decides **where** you search: portals, price sources, official car checks. Italy is fully supported; the other countries
are in beta.

## Where to start

- `home_overview` → `mercato` (country, currency, status).
- `home_searches` → `portali` (the portals of the country, with the family's search links and notes) and
  `fontiPrezzi` (where to look up area prices).
- `car_searches` → the car sites, the price sources and the official checks of the country.

## Rules

- **Only the portals of the family's country.** Never search Italian portals for a family in Spain, or the other way
  round.
- **Search URLs**: there are no written patterns as for Italy. Set the filters in the site's interface, read the URL
  the site generates, check title, number of results and cards, then save it on the portal with
  `home_set_portal` (`searchUrl`): next time you start from there.
- **The web rules are the same everywhere** ([web-rules.md](web-rules.md)). Many foreign portals ask for cookie
  consent: always refuse the non-essential cookies.
- **Area prices**: from the country's sources, official ones first (DVF in France, Land Registry in the UK, Notariado
  in Spain, WOZ in the Netherlands…). Write the source in the analysis.
- **Currency**: Switzerland (CHF), Poland (PLN) and the UK (GBP) are not in euros. Save the price as you find it and
  write the currency in the analysis and in your answer; never convert silently.
- **What is still Italian** in beta countries: for homes the buying costs, mortgage, path and guides; for cars the road
  tax, ownership transfer and new-driver rules. When you give Geenuga's figures in a beta country, say in one line that
  they are indicative.
- **Used cars**: before a viewing, the country's official checks (MOT history in the UK, RDW and NAP in the Netherlands,
  Car-Pass in Belgium, HistoVec in France, Historia pojazdu in Poland, Informe DGT in Spain). Germany, Austria and
  Switzerland have no public mileage register: ask for inspection reports and the service book.
- **Scams**: the schemes in [scams.md](scams.md) apply everywhere. Phone prefixes and regulated professions are the
  country's: do not judge a foreign listing with Italian rules.
