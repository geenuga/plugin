---
name: aggiorna-annunci
description: Checks again the homes saved in Geenuga Casa on the portals — are they still for sale or for rent? did the price (or the rent) go down? — records the new prices in the history, marks removed listings and tells the family about price drops. Use it when the user asks to update the listings, check the prices, or "any news on the homes we follow?".
---

# Check the saved listings again

You work only through the Geenuga tools (`home_…`) and the browser. Reply in the user's language.

References: [web-rules.md](../../references/web-rules.md); the portals of the family's country
([portals-italy.md](../../references/portals-italy.md) or [other-countries.md](../../references/other-countries.md)).

## 1. What to check

`home_list` (the homes still in the running, with link, price and status). Consider the ones with a link, not
discarded or bought. Priority: favourites and those with a viewing or an offer, then the others, oldest check first
(`home_overview` → `daFare.daRicontrollare` says how many have not been checked for a week). Homes for rent first:
rental listings disappear in a few days. At most ~30 listings per session: if there are more, do the most important
ones and say so.

## 2. For each listing

1. Open the link (and, if useful, the other linked listings from `home_details`). One page at a time, human pace.
2. **Removed?** (404, "listing no longer available", redirect to the search, "sold", "rented"):
   `home_listing_checked` with `online: false`.
3. **Still online**: read the price and record the check with `home_listing_checked` (`online: true` and the
   `price` you see now: if it changed, the history keeps the previous one). Note the change for the report.
4. Relevant changes (description, photos, "negotiation in progress", "under offer"): read the current analysis with
   `home_details`, then rewrite it with `home_update` (`analysis`) adding an "Update of <date>" section.

Blocks (CAPTCHA, checks, login, 401/403/429): stop on that site, tell the family and go on with the others.

## 3. Report

In the chat:
- **price drops** first (home, price before → now, −€ and −%), especially on the favourites;
- **removed** listings (maybe sold: if one was a favourite, say it gently and suggest similar alternatives);
- how many are unchanged.

If there are important drops on the favourites, also leave a message in the app with `home_message`, for example:

```markdown
- **Trilocale in Bolognina**: 299.000 € → 285.000 € (−4,7%)
```

Write the message in the family's language, with the `nellApp` links Geenuga returns.
