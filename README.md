# اوقات · Owqat — prayer times & qibla

<div align="right">

**اوقات** — اوقات شرعی و جهت قبله، محاسبه‌شده روی همین دستگاه. بدون اینترنت، بدون API، و موقعیتت هیچ‌جا فرستاده نمی‌شود.

</div>

Prayer times and the qibla bearing, computed **on your device** from astronomy —
no API, no network, and your coordinates never leave the page. One
self-contained HTML file.

## What it does

- The **next prayer** with a live countdown
- A **bar of the whole day** — every time marked in place, with a "now" marker
- All of today's times, the current period highlighted
- **Qibla dial** with the exact bearing from true north, and the distance to the Kaaba
- Four calculation methods (Tehran Geophysics, Jafari, MWL, ISNA) and both Asr schools
- 14 preset cities, browser geolocation, or type coordinates directly
- Today's Solar Hijri date
- Bilingual (فارسی / English), full RTL↔LTR

## Why there's no weather-app-style API here

This replaced a planned weather app. An API call is a `fetch` and a spinner;
this is actual astronomy, it works with no connection at all, and — in a country
where a third-party endpoint may simply be unreachable — offline is the feature.

**Solar position** uses the U.S. Naval Observatory's low-precision algorithm:
mean anomaly and mean longitude → ecliptic longitude → declination and the
equation of time. Times then come from the hour angle for each twilight
depression angle.

**Qibla** is the great-circle initial bearing to the Kaaba (21.4225°N, 39.8262°E) —
*not* the naive flat-map angle, which is wrong by tens of degrees at this distance.

## Verified against reference values

| check | result |
|---|---|
| Julian day, 3 epochs incl. Meeus's worked example | exact |
| Declination at March equinox | −0.24° (≈0 ✓) |
| Declination at both solstices | ±23.43° (vs ±23.44) |
| Equation of time, mid-Feb / early-Nov extremes | −14.20 / +16.45 min |
| Qibla — Cairo / Istanbul / London | 136.14 / 151.62 / 118.99° |
| Solar noon, day length | match analytic formulas to 0.002 h |
| Sunrise/sunset symmetry about noon | exact |
| Svalbard at midsummer | returns `null`, not `NaN` |

Three of my own initial expectations were wrong and the tests caught it: Tehran's
qibla is **218.4°**, not the ~199° I first assumed; Tehran→Mecca is 1944 km; and
solar noon sits just after 12:00 because **Iran abolished DST in 2022** — my
first guess assumed it still applied. Each was re-derived independently before
the expectation was changed.

## ⚠️ Scope

Times are computed to the minute with a standard low-precision solar model —
fine for planning, but this is **not** an authority. Different institutions use
different twilight angles, which is exactly why the method is selectable. For
anything you consider binding, check against your local convention.

## Design note

The page background follows **where you are inside the day** — indigo at night,
violet at dawn, blue through the day, amber at dusk. The prayer times *are* the
structure of the day, so this is information rather than decoration.

## Stack

`HTML` · `CSS` · `Vanilla JavaScript` · SVG

## Running locally

Because Owqat is a single self-contained HTML file, you can run it without a
build step or package installation. Open the HTML file directly in a browser,
or serve the folder with any simple static HTTP server when testing browser
features such as geolocation.
