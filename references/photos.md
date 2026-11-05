# The photos, one by one (floor plans included)

A listing's photos tell more than its text: how many rooms there really are, what state they are in, whether the
terrace exists, whether the home is the one advertised. Your job is to **look at all of them** and tell the family
what adds up and what doesn't. Geenuga saves what you see, runs its own checks on the numbers and shows everything on
the home's page, under «Le foto» (the photos: the `appLink` of the home plus `/foto`).

## 0. The family's preferences

First read how they want you to work: `home_overview` → `assistantPreferences`. They choose it in Geenuga Casa →
«Search with the assistant» → «How it works».

- `photos`: `all` = look at the photos of **every** home you save or analyse; `best` = only for the homes you propose
  as the best and for their favourites (or the ones they are moving forward with); `off` = don't look at them one by
  one (still copy them with `home_save_photos` and pass the portal's floor plans in `floorPlans`).
- `photoDetail`: `full` = original photos (default); `saver` = whole but reduced to 800 px, about half of the usage of
  their plan. `home_photos` applies it by itself; floor plans are always original.
- Change them only when they ask you: `home_set_assistant_preferences`.

## 1. Floor plans first

- **When you save the home** (`home_save`, `home_update`): if the portal lists the floor plans apart, put their links
  in `floorPlans` (not in `photos`): Geenuga adds them after the photos and marks them as floor plans «from the
  portal». Immobiliare.it: `multimedia.floorplans[]`; for the other portals see
  [portals-italy.md](portals-italy.md).
- If the portal mixes them with the photos, you recognise them by looking (`kind: "floor_plan"`).
- What the family said («it's a floor plan» in the app) or the portal said **stays**: you don't change it.

## 2. Looking at them

1. The photos must already be in Geenuga: `home_save_photos`.
2. `home_photos` with the id shows you the photos as images, 8 at a time (`from`, `count` up to 12), always whole,
   never cropped (originals, or reduced when `photoDetail` is `saver`), with the listing data the photos must
   confirm (`listing`), what is already known about each photo, Geenuga's current check (`check`) and the next batch
   (`next`, `remaining`). **Call it again with `next.from` until `next` is null: every photo, none skipped.**
   `only: "floor_plans"` shows only the floor plans, `only: "not_checked"` only the photos nobody has described yet.
3. **Look at every photo**. Don't skip: problems often hide in the last ones (bathroom, cellar, façade).

## 3. What to look at

For **each photo**:

- `kind`: `floor_plan`, `interior`, `outdoor` (the home's balcony, terrace, garden), `building` (façade, stairs,
  entrance, shared parts), `view`, `surroundings` (street, neighbourhood, maps), `document` (energy certificate…),
  `other`.
- `room` (for `interior` and `outdoor`): `living_room`, `kitchen`, `dining_room`, `bedroom`, `bathroom`, `hallway`,
  `study`, `laundry`, `storage`, `balcony`, `terrace`, `garden`, `garage`, `cellar`, `attic`, `other`.
- `caption`: one line for the family, in their language, concrete («Cucina abitabile con finestra»).
- `condition`: `as_new`, `good`, `dated` (1970s–90s, to freshen up), `to_renovate`.
- `flags`, when they apply: `virtual_staging` (furniture added by computer), `ai_edited` (the photo says it was
  modified with AI: a label such as «AI-modified», «Modificata con IA», «Edited with AI», usually in a corner), `render`, `watermark` (another agency's
  or website's logo), `wide_angle`, `heavy_editing` (fake skies, blown lights, retouching), `low_quality`,
  `duplicate` (a room already seen), `other_home` (not this home: stock images, another listing, another city, an
  impossible season).
- `issues`: what you see and what it costs — damp, mould, cracks, leaks, old wiring (sockets, fuse box, boiler, cast
  iron radiators), single glazing, possible asbestos (corrugated roofing), barriers to access — with `severity`
  `info`, `warn` or `serious`.

For each **floor plan** (`floorPlan`): `rooms` (Italian count, *locali*: bathrooms and kitchen excluded),
`bedrooms`, `bathrooms`, `sqm` (written on the plan or added up from the measurements: say how in `notes`), `levels`,
`outdoor` (balconies or terraces drawn), `notes` (windowless rooms, walk-through bedrooms, a 7 m² «bedroom» that
isn't one, doors that don't match the photos).

For the **whole home**:

- `seen`: how many **different** bedrooms and bathrooms you recognised (two photos of the same bathroom = one).
- `findings`, your coherence findings (`topic`, `severity`, `text`, `photos` with the numbers):
  - photos against the description: «ristrutturato» (renovated), «bagno nuovo», «luminoso», «vista aperta»;
  - photos against the floor plan: rooms more or less, windows, layout, terrace;
  - photos against the data: floor (a view from the third floor or the ground floor?), lift, heating, air
    conditioning, orientation (light and shadows), season and trees in a «recent» photo;
  - what is missing: no photo of the bathroom, the kitchen, the façade, the bedrooms;
  - authenticity: too perfect, repeated, other people's logos, another city (see [scams.md](scams.md), also a
    reverse image search when in doubt);
  - and what adds up (`severity: "ok"`): «the terrace is real and usable» reassures as much as a problem worries.
- `summary`: two or three honest sentences for the family, in their language.

**Labels in the corners.** Always check the four corners and the edges of every photo: agencies and portals write
«AI-modified», «Virtual staging», «Immagine indicativa», «Rendering» or their logos there. If there is an AI label, set
`ai_edited` and say in the caption what looks changed (lawn, sky, furniture, light). Even without a label, be wary of a
lawn that is all one green, postcard skies and edges that are too clean (`heavy_editing`).

**Never make things up.** If a photo is too small or dark to tell, say so; if you can't read the measurements, leave
`sqm` empty. Better a finding «to ask» than a wrong certainty.

## 4. Saving

`home_save_photo_analysis` with the id, `photos` (each with its number `n` from `home_photos`), `seen`, `findings`
and `summary`. The photos you don't mention keep what they had: you can save in parts, a page at a time, and send
`findings` and `summary` with the last part. `checked: false` if you only labelled some photos (e.g. the floor plans)
without looking at all of them.

If the answer has `notDescribedYet`, some photos still have no description: look at them and save them.

The answer gives the updated check: Geenuga's own findings (floor plan against m², bedrooms and bathrooms, outdoor
spaces listed but never photographed, condition, photos of another home, watermarks, visible defects…) and yours.
Photos of another home and other people's watermarks also count in the anti-scam check.

## 5. Telling the family

In the analysis and in the chat, a few lines: what the photos confirm, what doesn't add up (with the photo number),
what to ask or to look at during the viewing («open the bathroom cupboard», «look at the ceiling of the small
bedroom», «ask for the land registry floor plan, the *planimetria catastale*»). The home's page in Geenuga already
shows everything, photo by photo.
