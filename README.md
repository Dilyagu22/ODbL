# luach-tz-patch

**A high-resolution coordinate → timezone grid for Israel and its neighbours,
derived from OpenStreetMap. Published under the ODbL.**

This is a small derived database extracted from
[timezone-boundary-builder](https://github.com/evansiroky/timezone-boundary-builder)
(OpenStreetMap, ODbL) and shipped inside the Luach iPhone app. It is published
here so that the ODbL's share-alike term is satisfied plainly rather than by
argument, and so anyone who wants the same data can take it.

## What it is

| | |
| --- | --- |
| window | latitude 29.0 – 33.5, longitude 34.0 – 36.0 |
| resolution | 0.0025° — cells of about 275 m, so a border is placed to within ~140 m |
| grid | 1800 × 800 |
| zones | `Africa/Cairo` · `Asia/Riyadh` · `Asia/Amman` · `Asia/Jerusalem` · `Asia/Gaza` · `Asia/Hebron` · `Etc/GMT-2` · `Asia/Damascus` · `Asia/Beirut` |
| size | ~35 KB as JSON |

`tz-patch.json` is the database. `tz-patch.ts` is the same data with a small
lookup function, which is the form the app uses.

## Why it exists

General-purpose packed timezone rasters have little resolution in the Gulf of
Aqaba corner, where they can hand `Asia/Amman` to everything below about
latitude 30. That includes Eilat, Yotvata, Ketura, Samar and Ovda — and Taba,
which is in Egypt.

**Jordan abolished daylight saving in 2022 and Israel did not**, so through the
Israeli winter the two are an hour apart. For an app that prints candle-lighting
times, resolving Eilat to Jordan's clock is not a rounding error:

    Eilat, Friday 2026-01-16, resolved to Asia/Amman  → candle 17:44, sunset 18:04
    Eilat, same day, resolved to Asia/Jerusalem       → candle 16:44, sunset 17:04

The first is forty minutes **after** sunset, on a deadline that cannot be moved.

**No border here was drawn by hand.** Every boundary comes from the
OpenStreetMap-derived polygons, at full resolution, baked into a small window
so that a 73 MB dependency does not have to ship. Spot-checked when it was
adopted: Eilat resolves to `Asia/Jerusalem`, Aqaba to `Asia/Amman`, Taba to
`Africa/Cairo` — all correct, where the general raster gives Amman for all
three.

**This is a technical statement about timezone data and about nothing else.**
The zone names are those published by the IANA time zone database and carried
by OpenStreetMap; where a boundary falls is a question for the sources, not a
position taken here.

## Regenerating it

```
npm install
npm run generate
```

`gen-tz-patch.mjs` reads the polygons through `geo-tz` and writes the grid.
The output is deterministic: the same inputs give the same bytes.

## Licence

**ODbL 1.0** — see `LICENSE`. Contains information from OpenStreetMap,
© OpenStreetMap contributors.

If you build a derivative of this database and use it publicly, ODbL §4.4 asks
you to offer that database under the same terms.
