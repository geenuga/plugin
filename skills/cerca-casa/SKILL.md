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

`home_searches` gives the named searches (`ricerche`; the main one first), the criteria, the price cap, the areas
from the most neglected (`zoneDaQuando`), the portals of the family's country and the price sources. From the request:

- **empty** → the main search (`home_searches` without a name);
- **the name of a search** (case does not matter) → `home_searches` with `name`: use **only** those criteria;
- **`automatica`** → automatic mode (below): repeat the searches in `automatiche` one after the other;
- **`zone=A,B`** → today search **only** those municipalities, with the criteria of the named (or main) search;
- **other text** → a special request: start from the main (or named) search and take it into account.

Call the search in use **R**. Start from the areas in `zoneDaQuando` (never searched or searched longest ago): if the
session stops, the forgotten areas are done.

**Buying or renting**: `contract` of R says `vendita` (for sale) or `affitto` (for rent). For rent the prices are the
**monthly rent**, you search the rental section of the portals, and the extra rental scams in
[scams.md](../../references/scams.md) apply.

### Automatic mode (scheduled task, nobody at the screen)

- **Ask no questions and wait for no answers**: when a choice is not obvious, pick the most prudent one (do not save)
  and write it in the round's notes.
- Searches: `automatiche` of `home_searches`. If it is empty, stop at once without writing anything.
- Only the news: **page 1** of each search sorted by most recent; stop as soon as you meet listings already in Geenuga;
  at most ~8 listings opened per search.
- If the browser does not answer or a site shows an anti-bot check: do not insist, move on and note it.
- After each search: `home_log_search_run` with `automatic: true` (always, even with nothing new: the family is
  notified that you did the round).
- Instead of a chat report, **a message in the app** (`home_message`) only if there is something to say (new homes,
  price drops, a site that blocked you). Nothing new → no message.
- Close the tabs you opened.

## 1. Understand what to look for

- Use the criteria of R (`criteri`): areas, price, m², rooms, bedrooms, requirements (`mustHave`), what they dream of
  (`dream`) and what they do NOT want (`dealBreaker`): these last two are the most personal instructions.
- **Price cap** for the search: `tettoPerLaRicerca` (computed by Geenuga with the negotiation margin; for rent the
  monthly rent). Without it, use `priceMax` of R.
- If areas and maximum price are missing, **ask** (in automatic mode: skip the search and note it) for areas, maximum
  price, minimum m² and rooms, and save the answers with `home_set_search`. A new name creates a new search.
- If the family's pending requests (`home_overview` → `daFare.richieste`) are about the search (for example "also look
  in Casalecchio"), include them, then close them with `home_answer_request`.

## 2. Prepare the browser

As in [web-rules.md](../../references/web-rules.md): the family's browser with their sessions if available, otherwise
the built-in browser of your app. A new tab, one page at a time.

## 3. Portal by portal

Portals: the active ones in `home_searches` → `portali` (only the family's country). **Respect each portal's
`note`.** For each portal and each area:

1. **Search URL**: the portal's saved `urlRicerca` if there is one; otherwise build it (Italy: patterns in
   [portals-italy.md](../../references/portals-italy.md); elsewhere from the interface) with the area, the price cap,
   the minimum m² and rooms, private sellers only if `privateOnly`, **sorted by most recent**. Check that the filters
   are applied (title, count, cards). If a new URL works, save it with `home_set_portal` (`searchUrl`).
2. **Read the list**: for each card note the link, price, m², rooms, area, private/agency.
3. **Already seen?** Pass the links of the list to `home_known_links` and stop at the listings already in Geenuga, or
   after 3 pages per search.
4. **Discard from the card**: price above the cap +10%, m² below the minimum −10%, terms to exclude
   ([listing-terms.md](../../references/listing-terms.md)), obvious deal breakers, auctions unless `includeAuctions`. For
   rent also rooms and beds, short or tourist lets.
5. **Open the listing** of the remaining candidates (at most ~15 per session, most promising first) and collect all the
   fields of the `home_save` schema: price, m², rooms, bedrooms, bathrooms, floor and floors, lift, energy class,
   heating, monthly fees, condition, year, features, address, area, municipality, coordinates if present, advertiser
   (agency name), description, up to 15 large photos, cadastral data if given. For rent also lease type, furnished,
   deposit and commission in months, fees included or not, available from.
6. **Your anti-scam judgement** ([scams.md](../../references/scams.md)) → `scamRisk` and `redFlags`. Geenuga runs
   its own check too and does not save high-risk homes: count them among the discarded and mention them in the report.
7. **Area price** ([area-prices.md](../../references/area-prices.md)): once per area per session → `areaPricePerSqm`.
8. **Save** with `home_save` (up to 10 homes per call, `search` = the name of R), with all the data, the photo links
   and, for the best ones, the `analysis` (structure in [listing-terms.md](../../references/listing-terms.md)). Handle
   the outcomes:
   - `possibile_doppione` → if it is the same home, `home_listing_checked` with `otherListing` (and say if the
     price differs!); if it is a different one, repeat with `force`;
   - `gia_salvata` with a lower price → it is a price drop: report it;
   - `scartata_truffa` → not saved: fine, mention it.
9. For each home **created**: `home_save_photos` (the photos stay even if the listing disappears). Geenuga computes the
   driving times to the family's places by itself when the home has coordinates: after a minute `home_commutes` with
   the id shows them. If a home is beyond a place's maximum threshold (for example the office), do not propose it among
   the best and mention it among the discarded; with a warning, propose it but say so.

If a site blocks you (CAPTCHA, check, login, 401/403/429): stop on that site, tell the family and move to the next.

## 4. Log the round

**Always**, even if you found nothing or a site blocked you: `home_log_search_run` with the search (`searchName`),
`automatic`, the portals visited, **only** the municipalities you actually covered (`areas`: next time Geenuga starts
from the others), pages read, listings seen, new, updated and discarded with the reasons (`reasons`), the blocks met
(`blocks`), the ids of the homes created (`newIds`), the criteria used (cap, filters), the duration and short notes.
The family gets a notification with what you searched.

## 5. Report to the family (in the chat)

Short and warm, like an expert friend:
- which search you used, how many new homes and the **3–5 best**, one line each: price (for rent: monthly rent and the
  money needed to move in, from `home_details`), m², area, why it may suit them, one doubt to check, and the driving
  minutes to their most frequent place if already computed;
- price drops on homes already saved;
- suspicious listings discarded (one line);
- where to see them: the `nellApp` links Geenuga returns.

Never contact advertisers and never save searches or alerts on the portals without an explicit yes.
