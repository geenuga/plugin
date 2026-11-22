---
name: aggiorna-annunci
description: Checks again the homes saved in Geenuga Casa on the portals — are they still for sale or for rent? did the price (or the rent) go down? — records the new prices in the history, marks removed listings and tells the family about price drops. Use it when the user asks to update the listings, check the prices, or "any news on the homes we follow?", and in the automatic round after the search (`automatica`).
argument-hint: "[automatica]"
---

# Check the saved listings again

You work only through the Geenuga tools (`home_…`) and the browser. Reply in the user's language.

References: [web-rules.md](../../references/web-rules.md); the portals of the family's country
([portals-italy.md](../../references/portals-italy.md) or [other-countries.md](../../references/other-countries.md)).

## 1. What to check

`home_overview` → `toDo.toRecheck` is the list of the homes to check again **now**, already in the right order:
`count` how many are due, `items` the ones for this round (at most 40), each with its `reason`. The family decides it
in «How the assistant works» (`assistantPreferences.priceChecks`):

- `all` (default): the homes in play, i.e. the ones they like (`liked`: favourites, interesting, with a viewing or an
  offer) and those for rent (`rent`, they disappear in a few days) **every day**, the others (`other`) **every 3 days**;
- `liked`: only the ones they like, every day;
- `off`: none on your own; check them only when the family asks (then start from `home_list`, favourites and rentals
  first).

Discarded and bought homes, links to analyse and listings already removed never enter. If the family asks you to
check something more (a home, all of them), do it even if it is not due. They change the preference in the app, or
you with `home_set_assistant_preferences` only when they ask.

### Automatic mode (`automatica`)

It starts from the automatic round, after the searches (`cerca-casa` skill, automatic mode): nobody at the screen, no
questions.

- Do the homes in `toDo.toRecheck.items`, in order; if `count` is bigger, the others wait for the next round.
- On Immobiliare.it you don't need to open one listing at a time: from a listing page already open, read the other
  listings of the same portal one after the other (at least 2 seconds apart) and take from `__NEXT_DATA__` only the
  price, "not available" and how many photos ([portals-italy.md](../../references/portals-italy.md)); print only what
  changed. Other portals, one page at a time.
- At the first block (CAPTCHA, check, 401/403/429) stop on that portal and go on with the others.
- No chat report: price drops and removed listings go in the same message as the automatic search (`home_message`),
  only if there are any. Unchanged homes are not reported.

## 2. For each listing

1. Open the link (and, if useful, the other linked listings from `home_details`). One page at a time, human pace.
2. **Removed?** (404, "listing no longer available", redirect to the search, "sold", "rented"):
   `home_listing_checked` with `online: false`.
3. **Still online**: read the price and record the check with `home_listing_checked` (`online: true` and the
   `price` you see now: if it changed, the history keeps the previous one), even when nothing changed: that is how
   the home leaves the list until its next check. Note the change for the report.
   **Missing photos?** If the listing has more photos than the home in Geenuga (up to 100 are kept), send with
   `home_update` in `photos` the photos already saved (Geenuga's links from `home_details`, as they are) followed by
   the missing ones, then `home_save_photos`: it copies only the new ones.
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

Write the message in the family's language, with the `appLink` links Geenuga returns.
