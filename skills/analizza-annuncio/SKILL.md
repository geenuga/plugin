---
name: analizza-annuncio
description: Analyses a property listing in depth (from any website) for the family — reads all the data, compares the price with the area, runs the anti-scam check, works out the real costs, saves details, photos and analysis in Geenuga Casa. Without a link, analyses all the links «da analizzare» the family saved in the app. Use it when the user sends a link to a home, pastes a listing, or asks "what do you think of this listing?", "is it a scam?", "is it worth a viewing?".
argument-hint: "[link to the listing]"
---

# Analyse a listing

Link given (may be empty): $ARGUMENTS

You work only through the Geenuga tools (`home_…`) and the browser. Reply in the user's language; the analysis you save
is in that same language.

References: [web-rules.md](../../references/web-rules.md), [listing-terms.md](../../references/listing-terms.md),
[scams.md](../../references/scams.md), [area-prices.md](../../references/area-prices.md),
[portals-italy.md](../../references/portals-italy.md) (Italy) or
[other-countries.md](../../references/other-countries.md) (any other country: `home_overview` → `mercato`).

## 1. Which listings

- **With a link**: `home_known_links` with that link. If it is already in Geenuga you will update that home
  (`home_update` with its id); otherwise you will create it at the end with `home_save`.
- **Without a link**: `home_overview` → `daFare.linkDaAnalizzare`: links the family pasted in the app (status «da
  analizzare»). Analyse them one by one; each one already has an id.
- **Pasted text, no link**: analyse the text; save it only if the family wants.

## 2. Read the listing

Open the link in the browser ([web-rules.md](../../references/web-rules.md)) and collect all the fields of the
`home_save` schema. If the link is from an aggregator (Trovit, Mitula), open and record the **original source**. If the
listing no longer exists, say so (for a home already in Geenuga: `home_listing_checked` with `online: false`).

If it is an auction, bare ownership, a share or a rent-to-buy, say it clearly at once
([listing-terms.md](../../references/listing-terms.md)).

**For rent?** If the listing is a rental (monthly rent), save it with `contract: "affitto"`, `price` = monthly rent
and the rental fields (lease type, furnished, deposit, commission, fees included, available from). Use the rental part
of [scams.md](../../references/scams.md) and the area rent in €/m² per month.

## 3. Comparisons and checks

1. **€/m² against the area** ([area-prices.md](../../references/area-prices.md)) → `areaPricePerSqm`.
2. **Your anti-scam judgement** ([scams.md](../../references/scams.md)) → `scamRisk`, `redFlags`. Search the web
   for a distinctive phrase of the description to find copies in other cities or with other prices. Geenuga adds its
   own check and keeps the higher risk.
3. **The same property elsewhere?** `home_list` with the municipality (`town`), a price cap close to this one
   (`priceMax`) and the `contract`; if it is the same home on another portal, link it with `home_listing_checked`
   (`otherListing`) and point out differences in price or advertiser.
4. **Does it fit them?** `home_searches` (criteria) and `home_budget` → within the criteria? within the budget?

## 4. Save

- New home: `home_save` with all the data, the photos and the `analysis`. If Geenuga answers `scartata_truffa`, it
  was not saved: tell the family why, and save it anyway (`includeSuspicious`) only if they explicitly ask.
- Home already in Geenuga or «da analizzare»: `home_update` with its id, the fields found (`fields`) and the
  `analysis` (the status moves from «da analizzare» to «da valutare» by itself).
- The analysis follows the structure in [listing-terms.md](../../references/listing-terms.md): concrete and honest,
  with the **questions to ask** the agency or the owner and what to ask to see (energy certificate, floor plan,
  condominium fees and minutes, title of provenance).
- Photos: `home_save_photos` with the id.
- Then `home_details` with the id: it gives the real costs computed by Geenuga (cash needed, monthly payment and costs;
  for rent the money to move in and the monthly spend), the comparison with the area, the match and the driving times.

## 5. Answer

In the chat, a few lines: the verdict (worth a viewing? yes / no / maybe, and why), the real cost (from `home_details`),
the 2–3 main doubts, the risk level and the `nellApp` link. If the risk is high: never send money or documents, and
which checks to do. For rent, always: no payment before having seen the home and read the contract.

If the link came from a request of the family (`home_requests`), close it with `home_answer_request`, summarising
the verdict.

Never contact the advertiser: at most prepare the message, and they send it.
