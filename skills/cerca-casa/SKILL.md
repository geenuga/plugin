---
name: cerca-casa
description: Searches the property portals of the family's country for new homes for sale or for rent, with the searches, budget and portals saved in Geenuga Casa and the family's own browser; skips listings already seen, filters out scams and saves in Geenuga only the homes worth a look, with photos. Use it when the user asks to search for a home, find listings, "see if there is anything new", "look for two-bedroom flats in …", or for the automatic search round of a scheduled task ("cerca-casa automatica").
argument-hint: "[name of a saved search] [zone=Town1,Town2] | automatica | special request"
---

# Search for homes

Request (may be empty): $ARGUMENTS

Goal: **a few right homes, verified**, not a hundred listings. You work only through the Geenuga tools (`home_…`) and
the browser. Reply in the user's language; save titles, analyses and notes in that same language.

Read first: [web-rules.md](../../references/web-rules.md), [scams.md](../../references/scams.md),
[listing-terms.md](../../references/listing-terms.md), [area-prices.md](../../references/area-prices.md); for the
portals [portals-italy.md](../../references/portals-italy.md) if the country is Italy, otherwise
[other-countries.md](../../references/other-countries.md).

## 0. Which search

`home_searches` gives the named searches (`searches`; the main one first), the criteria, the price cap, which listings to
check (`listingsToCheck`), the areas from the most neglected (`areasLastSearched`), the portals of the family's country and the price sources. From the request:

- **empty** → the main search (`home_searches` without a name);
- **the name of a search** (case does not matter) → `home_searches` with `name`: use **only** those criteria;
- **`automatica`** → automatic mode (below): repeat the searches in `automaticSearches` one after the other;
- **`zone=A,B`** → today search **only** those municipalities, with the criteria of the named (or main) search;
- **other text** → a special request: start from the main (or named) search and take it into account.

Call the search in use **R**. Start from the areas in `areasLastSearched` (never searched or searched longest ago): if the
session stops, the forgotten areas are done.

**Areas inside a town** (a neighbourhood, a park: e.g. «Parco degli Acquedotti» in Rome): in `areasLastSearched` they
have `town`, `searchAs` (the names the portals use: Appio Claudio, Quadraro…) and `radiusKm`. On the portals search the
**town** and narrow it down with the portal's zones that match `searchAs` (or draw the area around the centre, if the
portal allows it). A home is in the area if its neighbourhood is one of those names or its coordinates are within the
radius: always save `neighborhood` and, when available, `lat`/`lng`. In the round's log write the area with **its own
name**, not the neighbourhoods.

**Buying or renting**: `contract` of R says `sale` or `rent`. For rent the prices are the
**monthly rent**, you search the rental section of the portals, and the extra rental scams in
[scams.md](../../references/scams.md) apply.

### Automatic mode (scheduled task, nobody at the screen)

- **Ask no questions and wait for no answers**: when a choice is not obvious, pick the most prudent one (do not save)
  and write it in the round's notes.
- Searches: `automaticSearches` of `home_searches`. If it is empty, stop at once without writing anything.
- Start from `searchRules.portalsDue` (see section 3): a portal left behind (`home_overview` → `toDo.neglectedPortals`)
  is done in this round, before repeating yesterday's; if you cannot get to it, say so in the notes of the log.
- Which listings to check is up to the search (`listingsToCheck`, see section 3): by default **all** the ones online,
  rotating through the areas; only the news if the family switched on «New listings only». How many listings to open
  per search is `searchRules.listingsPerRun`. A portal blocked in the previous round (`home_overview` → `lastRun.blocks`, read at the start) is picked
  up from there: with «new listings only», read up to 3 pages to catch up on what came out meanwhile.
- If the browser does not answer, note it in the log and stop. If a portal shows an anti-bot check (DataDome: "device
  check"), follow [web-rules.md](../../references/web-rules.md) → "When a portal stops you": wait once, then move on,
  record it in `blocks` and leave the family one message in the app (once, not at every round).
- After each search: `home_log_search_run` with `automatic: true` (always, even with nothing new: the family is
  notified that you did the round).
- Instead of a chat report, **a message in the app** (`home_message`) only if there is something to say (new homes,
  price drops, a site that blocked you). Nothing new → no message.
- Close the tabs you opened.

## 1. Understand what to look for

- Use the criteria of R (`search`): areas, price, m², rooms, bedrooms, requirements (`mustHave`), what they dream of
  (`dream`) and what they do NOT want (`dealBreaker`): these last two are the most personal instructions.
- **Trophies** (`trophies` of R, levels `bronze`, `silver`, `gold`) are the features the family would love: they
  **never discard a home**, they raise its match (a gold weighs three bronzes) and show as badges in Geenuga Casa.
  Geenuga checks the ones with a `feature` from the home's data; for the ones written by hand, pass `trophyChecks` to
  `home_save` with what the listing really says (`{"Bedroom on the ground floor": true}`, `false` if it surely does not
  have it). If the listing does not say, leave it out (it stays «to check») and add it to the questions. In the report,
  point out the homes with gold trophies.
- **Price cap** for the search: `searchPriceCap` (why in `howCapIsComputed`): it is `priceMax` of R as the family chose
  it, **even when it is above the budget**. Do not lower it: homes above the budget are saved anyway and Geenuga marks
  them «over budget»; say so in the report. Only when R has no maximum price does the cap come from the budget, with
  the negotiation margin. For rent it is the monthly rent.
- **The rules of the round**: `searchRules` of `home_searches`, all chosen by the family in the search («How Claude
  searches», or with `home_set_search`): `priceCap` (the cap, already with their price margin), `sqmFloor` (the minimum
  m² minus their margin), `listingsPerRun` (how many listings to open), `alsoInclude` (auctions, bare ownership, shares,
  high scam risk listings), `portalOrder` (the order of the portals), `portalsDue` (portal by portal, the areas not
  searched there for longer than the family's cadence, `portals[].everyDays`: 0 = at every round). **Do not add
  margins, limits or discards of your own**: if a rule looks wrong, tell the family and suggest changing it in the search.
- **Which listings to check**: `listingsToCheck` of `home_searches` (`mode`: `all` by default, `new` if the family
  switched on «New listings only» in the search, field `newListingsOnly`). It is not your choice: do not narrow down to
  the news if the search does not say so.
- If areas and maximum price are missing, **ask** (in automatic mode: skip the search and note it) for areas, maximum
  price, minimum m² and rooms, and save the answers with `home_set_search`. A new name creates a new search; `addAreas`
  and `removeAreas` change a few areas, `areaDetails` describes an area that is not a town (town, portal names, radius).
- If the family's pending requests (`home_overview` → `toDo.requests`) are about the search (for example "also look
  in Casalecchio"), include them, then close them with `home_answer_request`.

## 2. Prepare the browser

As in [web-rules.md](../../references/web-rules.md): the family's browser with their sessions if available, otherwise
the built-in browser of your app. A new tab, one page at a time.

## 3. Portal by portal

**Overdue portals first:** `searchRules.portalsDue` gives, portal by portal, the areas of R not searched there for
longer than the family's cadence (`portals[].everyDays`), the most neglected first. Start from those, in an automatic
round too and with «New listings only» too, before going back where you have already been: what counts is the portal,
not the town (an area searched yesterday on the biggest portal may still be due on the agency websites). The family
changes the cadence under each portal, on the assistant page (or you with `home_set_portal` → `everyDays`, if they ask).

Then the other portals in `searchRules.portalOrder`, in that order (the one searched longest ago first, so an
interrupted round does not always leave the same ones behind; only the family's country). **Respect each portal's
`notes`.** For each portal and each area:

1. **Search URL**: the portal's saved `searchUrl` if there is one; otherwise build it (Italy: patterns in
   [portals-italy.md](../../references/portals-italy.md); elsewhere from the interface) with the area, `priceCap`,
   `sqmFloor`, the minimum rooms, private sellers only if `privateOnly`, **sorted by most recent**. Check that the filters
   are applied (title, count, cards). If a new URL works, save it with `home_set_portal` (`searchUrl`).
2. **Read the list**: for each card note the link, price, m², rooms, area, private/agency.
3. **Already seen? How many pages?** Pass the links of the list to `home_known_links`.
   - `listingsToCheck.mode` = `all` (default): read **all** the pages of results for the area, to the end. Skip the
     listings already in Geenuga without opening them (if the price on the card changed, update it with `home_update`)
     and go on: older listings count as much as new ones. Keep the human pace (30–50 pages per portal per session): an
     area you cannot finish does **not** go in the `areas` of the log, so the next round picks it up first.
   - `listingsToCheck.mode` = `new`: only page 1 sorted by most recent; stop as soon as you meet listings already in
     Geenuga (at most 3 pages).
4. **Discard from the card**: price above `priceCap`, m² below `sqmFloor`, what
   [listing-terms.md](../../references/listing-terms.md) excludes and `alsoInclude` does not switch back on (auctions,
   bare ownership, shares), obvious deal breakers. For
   rent also rooms and beds, short or tourist lets.
5. **Open the listing** of the remaining candidates (at most `listingsPerRun` per search, most promising first; the others wait for the next round) and collect all the
   fields of the `home_save` schema: price, m², rooms, bedrooms, bathrooms, floor and floors, lift, energy class,
   heating, monthly fees, condition, year, features, address, area, municipality, coordinates if present, advertiser
   (agency name), description, all the large photos (up to 100), cadastral data if given. For rent also lease type, furnished,
   deposit and commission in months, fees included or not, available from.
6. **Your anti-scam judgement** ([scams.md](../../references/scams.md)) → `scamRisk` and `redFlags`. Geenuga runs
   its own check too: unless `alsoInclude.highScamRisk` is on, it does not save high-risk homes (count them among the
   discarded and mention them in the report); when it is on, they are saved with their red flags, and in the report
   you remind the family never to send money or documents.
7. **Area price** ([area-prices.md](../../references/area-prices.md)): once per area per session → `areaPricePerSqm`; the town's official values are already in `home_zone_prices`.
8. **Save** with `home_save` (up to 10 homes per call, `search` = the name of R), with all the data, the photo links
   and, for the best ones, the `analysis` (structure in [listing-terms.md](../../references/listing-terms.md)). Handle
   the outcomes (`outcome`):
   - `possible_duplicate` → if it is the same home, `home_listing_checked` with `otherListing` (and say if the
     price differs!); if it is a different one, repeat with `force`;
   - `already_saved` with a lower price → it is a price drop: report it;
   - `discarded_scam` → not saved: fine, mention it.
9. For each home **created**: `home_save_photos` (the photos stay even if the listing disappears). Look at **the photos one
   by one** with [photos.md](../../references/photos.md) (`home_photos`, then `home_save_photo_analysis`) as the family
   prefers (`home_overview` → `assistantPreferences.photos`): with `all` for every home created (if there are many, at
   least the ones you propose as the best; the next `geenuga` round does the others from `toDo.photosToCheck`), with
   `best` only for the ones you propose as the best, with `off` for none. A home that doesn't match its photos is not
   among the best. Geenuga computes the
   driving times to the family's places by itself when the home has coordinates: after a minute `home_commutes` with
   the id shows them. If a home is `over` a place's maximum threshold (`threshold`, for example the office), do not propose it among
   the best and mention it among the discarded; with `warn`, propose it but say so.

If a site blocks you (CAPTCHA, device check, login, 401/403/429): follow [web-rules.md](../../references/web-rules.md) →
"When a portal stops you". With the family at the screen ask **once** to pass the check in their own browser (and sign
in with their account), or to switch to their own browser with the assistant's extension if you are in a built-in or
cloud browser; meanwhile go on with the other portals and resume the blocked one at their "done".

## 4. Log the round

**Always**, even if you found nothing or a site blocked you: `home_log_search_run` with the search (`searchName`),
`automatic`, the portals visited, **only** the municipalities you actually covered (`areas`: next time Geenuga starts
from the others; if the portals did not all cover the same ones, `coverage` with the areas finished on each portal,
otherwise `portalsDue` would count as done what you did not look at), pages read, listings seen, new, updated and discarded with the reasons (`reasons`), the blocks met
(`blocks`), the ids of the homes created (`newIds`), the criteria used (cap, filters), the duration and short notes.
The family gets a notification with what you searched.

## 5. Report to the family (in the chat)

Short and warm, like an expert friend:
- which search you used, how many new homes and the **3–5 best**, one line each: price (for rent: monthly rent and the
  money needed to move in, from `home_details`), m², area, why it may suit them, one doubt to check, and the driving
  minutes to their most frequent place if already computed;
- price drops on homes already saved;
- suspicious listings discarded (one line);
- where to see them: the `appLink` links Geenuga returns.

Never contact advertisers and never save searches or alerts on the portals without an explicit yes.
