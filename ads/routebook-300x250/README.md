# Simple RouteBook — 300x250 MPU (Telegraph)

Medium Rectangle (MPU) display creative for Telegraph Media Group inventory.

## Files to supply

| File | Use | Size |
|---|---|---|
| `routebook-300x250.jpg` | **Primary static creative** — hand this to Telegraph | 41 KB |
| `routebook-300x250.png` | Lossless static alternative | 31 KB |
| `routebook-300x250@2x.png` | 600x500 retina master (previews, social, press) | 70 KB |
| `routebook-300x250-html5.zip` | HTML5 build for 3rd-party tag delivery (GAM / DV360 / Xandr) | 51 KB |
| `routebook-300x250.svg` | Editable vector master | 57 KB |
| `html5/index.html` | Unzipped HTML5 source | 74 KB |
| `apple-app-store-badge-blk.svg` | Apple's official badge artwork, as embedded | 11 KB |
| `fonts/` | DM Sans + Outfit variable TTFs, licences, and build fontconfig | 351 KB |

## Telegraph spec compliance (300x250 MPU)

- **Dimensions:** 300 x 250 exactly.
- **Formats accepted:** GIF, JPG, 3rd-party tag. JPG supplied; HTML5 zip supplied for tag delivery.
- **Initial file size limit:** 100 KB — JPG is 41 KB, HTML5 initial load is 74 KB (single self-contained file, no external requests).
- **Max HTML5 file size:** 2.2 MB — well under.
- **Animation:** none. Static creative, so no loop/duration limits apply.
- **Audio:** none.
- **Border:** 1px `#DDE4EC` border on all four sides so the light creative separates from the page background.
- **Platforms:** Desktop, tablet, mobile (Direct IO or PG).
- **Third-party requests:** none. Fully self-contained; no CDN, font, or tracker calls.

Source: [Telegraph Specs — Standard IAB Display](https://sites.google.com/telegraph.co.uk/telegraph-specs/standard-iab-display)

## Click-through

    https://apps.apple.com/app/simple-routebook/id6792829304

The HTML5 build declares a `clickTag` variable that the ad server overwrites at serve time,
and `<meta name="ad.size" content="width=300,height=250">` for auto-sizing. For a static JPG
buy, give Telegraph the click URL above alongside the file.

Add campaign tracking parameters to the click URL when trafficking, e.g.
`?pt=…&ct=telegraph_mpu&mt=8` (Apple App Analytics campaign token).

## Copy

> **You remember the name. Not the address.**
> The dentist, the vet, the storage unit — saved by name, launched in Apple Maps with one tap.
> [Download on the App Store]
> Free for up to three routes · Premium unlocks unlimited
>
> *Route card:* **Dentist** / 24 Mill Road, Elmsworth

"Remember", not "know": the friction isn't that you never learned the address, it's that it
won't come to you when you need it. The proposition is the *medium-frequency* destination — somewhere you return to often enough
to be worth saving, but rarely enough that the address never sticks. Deliberately not the
commute or the school run: those are places people already know by heart, which makes them the
weakest possible illustration of the problem. The route card carries the concrete detail (a
name standing in for an address) so the headline can stay short and broad.

### The example address is fictional, by construction

`24 Mill Road, Elmsworth` uses a deliberately generic street in a town that **does not exist** —
checked against Ordnance Survey open place-name data via `api.postcodes.io/places`, which
returns no match for `Elmsworth`. Because the town is invented, no street within it can resolve,
so the address cannot collide with a real one regardless of street name.

There is no postcode, intentionally: it keeps the creative free of any country-specific address
format, so the same artwork can run in non-UK placements without a re-cut. If you ever swap the
example, re-check the town name the same way — plausible-sounding English town names very often
turn out to be real (`Ashcombe`, an early candidate, is an existing place).

## Apple badge compliance

The CTA is Apple's **official, unmodified** "Download on the App Store" badge — the US-UK
English black variant (`Download_on_the_App_Store_Badge_US-UK_RGB_blk_4SVG_092917`), pulled
from the [App Store Marketing Tools](https://toolbox.marketingtools.apple.com/en-gb/app-store/)
asset endpoint and saved here as `apple-app-store-badge-blk.svg`. It is embedded in the
creative as vector, at its native 2.9916:1 aspect ratio (137.6 x 46 px).

Per Apple's Identity Guidelines:

- Artwork is unaltered — no recolouring, restyling, or redrawn type.
- The black variant is used, which is the correct choice on a light background.
- Clear space around the badge exceeds 1/10th of badge height on all four sides
  (12 px to the route graphic, 17 px above, 6.5 px below).
- The badge is the only download call-to-action in the creative.

If the campaign extends to non-English Telegraph territories, swap in the matching localised
badge from the same tool.

## Brand

Colours are taken from the site stylesheet (`/styles.css`): navy `#2B3D5B`, teal `#1FA585`,
muted `#5A6E85` / `#8B9DB3` / `#9AAABB`. The pin on the route card uses the app-icon red
(`#EA4335`).

The creative deliberately carries **no background texture and no top accent bar**. Earlier
versions had a faint map grid (echoing the app icon) and a blue gradient rule along the top
(echoing `.app-card::after` on the site). Both were removed: at low opacity the grid read as a
printing flaw rather than a map, and the accent bar read as page furniture rather than part of
the ad. The white field, the 1px border, and the two colour accents — teal headline, red pin —
carry the design on their own. Don't reintroduce either without looking at it at 1:1 first.

**Type:** the same faces as the site — **Outfit** for the app name (700) and headline (800),
**DM Sans** for the strapline, body copy, and footer (400/500). Headline letter-spacing is
`-0.65px`, matching the site's `-.03em` on `.hero h1`.

The fonts are vendored in `fonts/` as Google's variable TTFs, under the SIL Open Font Licence
(`fonts/OFL-DMSans.txt`, `fonts/OFL-Outfit.txt`). They are **not** installed system-wide —
`fonts/fonts.conf` is a self-contained fontconfig that adds this directory alongside the system
font dirs, so builds are reproducible on any machine without touching the user font library.
Note that the fonts are only needed to *render* the SVG; the shipped JPG/PNG/HTML5 files have
the type baked in as pixels and carry no font dependency.

## Rebuilding

Export the build fontconfig first, or the type will silently fall back to a system face:

    export FONTCONFIG_FILE="$PWD/fonts/fonts.conf"

    rsvg-convert -w 300 -h 250 routebook-300x250.svg -o routebook-300x250.png
    rsvg-convert -w 600 -h 500 routebook-300x250.svg -o routebook-300x250@2x.png
    magick routebook-300x250.png -background white -flatten -quality 88 -sampling-factor 4:4:4 -strip routebook-300x250.jpg
    zip -j -9 routebook-300x250-html5.zip html5/index.html

**Two compression settings, deliberately different.** The static JPG ships at **quality 97**:
it is viewed at 1:1, so JPEG noise in the large flat near-white areas is directly visible, and
at 41 KB there is no reason to economise. The 2x asset embedded in the HTML5 build ships at
**quality 84**: it is downscaled to 300x250 on display, which hides the artefacts, and base64
inflates whatever it weighs by a third. Raising it to 92 once pushed `index.html` to 100,252 B
— over the initial-load cap. Keep the embedded asset at or below ~56 KB.

The app icon and the Apple badge are both inlined in the SVG, so it is standalone.
The HTML5 build embeds a 600x500 JPG as a base64 data URI.
