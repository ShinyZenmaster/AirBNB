[Airbnb](https://apify.com/gooyer.co/airbnb?fpr=data)

# Airbnb Scraper

Pull structured data from any Airbnb URL — rooms, searches, experiences, services, reviews, and availability calendars. Built for short-term-rental market research, property-manager competitive intelligence, travel-tech product teams, and anyone who needs Airbnb data at volume without babysitting a browser.

This is the only scraper on the Apify Store with both **Airbnb Experiences** and **Airbnb Services** coverage. Everyone else stops at stays.

## What you can scrape

Paste any of these URL types into **Start URLs** — the scraper classifies each one and runs the right handler:

| URL shape | What you get |
| --- | --- |
| `airbnb.com/rooms/<id>` | One rich record per stay — host, amenities, pricing breakdown, house rules, safety, 10+ review tags with counts, 5-star rating distribution, per-category ratings, sleeping arrangement with room photos, 40+ hero images, guest-favorite flag |
| `airbnb.com/s/<city>/homes` | One record per listing in the search (up to ~18 per page), with canonical URL, rating + review count, price quote, badges, payment messages ("free cancellation"), map coordinates |
| `airbnb.com/s/<city>/experiences` | One record per experience — title, duration, theme, price breakdown, cover + gallery images, rating + review count |
| `airbnb.com/experiences/<id>` | Full experience PDP — name, byline, cover image, offerings, duration, raw activity listing metadata |
| `airbnb.com/s/<city>/services?service_type_tag=Tag:<id>` | One record per service (photographers, chefs, trainers, massage, etc.) — name, byline, rating + review count, price, display location, cover + gallery |
| `airbnb.com/services/<id>` | Full service PDP — name, byline, cover image, offerings, `productType: SERVICE`, raw activity listing metadata |

Dates, guest counts, price filters, property types, service types — any filters you'd set via the Airbnb UI just need to be baked into the URL. Paste a search URL with `?adults=2&checkin=2026-06-01` or `?service_type_tag=Tag%3A8949` and the scraper respects it automatically.

## Optional enrichments

Turn these on per-run to pull extra data the base URL doesn't ship with. Each enrichment is billed separately — so a run without enrichments stays at the dirt-cheap commodity rate.

- **`reviews`** — paginated reviews on ANY PDP type (stays, experiences, services). One switch. Stays return the first 24 sorted BEST_QUALITY; experiences & services return the full list sorted by recency. Each includes reviewer name, location, profile photo, rating, written comment, and the auto-translated version.
- **`scrapeCalendar`** — 12-month booking calendar per stay PDP showing available / unavailable / min-stay per day
- **`scrapeSimilarListings`** — the ~8 similar listings Airbnb recommends on each stay PDP with their IDs, names, prices, images
- **`pdpPage`** — for stays-search URLs, enqueue a full PDP scrape for every listing the search returns
- **`experiencePage`** — same, but for experiences-search URLs
- **`servicePage`** — same, but for services-search URLs

Nothing is on by default. Check only what you need.

**One note on the enqueue toggles**: when `pdpPage` / `experiencePage` / `servicePage` is on, each listing produces **two dataset records** — one slim search-card record (`pageType: "*_search"`) and one rich PDP record (`pageType: "*_pdp"`) — both sharing the same `id`. This is intentional: the card lands in your dataset within seconds so you can start working immediately, and the full PDP follows a few seconds later. To "flatten" them into one row per listing, join on `id`:

```
SELECT pdp.*, card.metadata__rank AS search_rank
FROM dataset pdp
LEFT JOIN dataset card ON pdp.id = card.id AND card.pageType LIKE '%_search'
WHERE pdp.pageType LIKE '%_pdp'
```

## Sample output — real record, trimmed

From a fresh scrape of `airbnb.com/rooms/32124652` with `reviews: true`, `scrapeCalendar: true`, `scrapeSimilarListings: true`:

```
{
    "pageType": "stays_pdp",
    "id": "32124652",
    "url": "https://www.airbnb.com/rooms/32124652",
    "title": "Pavilion with private pool – AL MAADEN Marrakech",
    "description": {
        "htmlDescription": { "htmlText": "Beautiful contemporary studio in the middle of Golf Al Maaden..." }
    },
    "highlights": [
        { "title": "Dive right in", "subtitle": "This is one of the few places in the area with a pool.", "icon": "SYSTEM_POOL" },
        { "title": "Self check-in", "subtitle": "Check yourself in with the lockbox.", "icon": "SYSTEM_CHECK_IN" }
    ],
    "images": [ /* 42 hero photos with baseUrl + accessibilityLabel + aspectRatio */ ],
    "imageCount": 42,
    "sleepingArrangement": [
        { "title": "Living area", "subtitle": "2 couches", "images": [...] }
    ],
    "amenities": {
        "preview": ["Kitchen", "Wifi", "Free parking on premises", "Private pool", "TV"],
        "groups": [ /* 11 groups: Bathroom, Kitchen, Bedroom, etc., each amenity with available: true|false */ ]
    },
    "host": {
        "name": "Karim",
        "userId": "RGVtYW5kVXNlcjoyNDEwMTcxNTk=",
        "isSuperhost": true,
        "isVerified": true,
        "stats": [
            { "label": "Reviews", "value": "355" },
            { "label": "Rating", "value": "4.76" },
            { "label": "Years hosting", "value": "7" }
        ],
        "details": ["Response rate: 100%", "Responds within an hour"]
    },
    "location": {
        "latitude": 31.59329, "longitude": -7.94488,
        "title": "Where you'll be",
        "subtitle": "Marrakesh, Marrakesh-Safi, Morocco"
    },
    "pricing": {
        "structuredDisplayPrice": {...},
        "canInstantBook": true,
        "petsAllowed": false,
        "cancellationPolicies": [...]
    },
    "rules": {
        "houseRulesSections": [
            { "title": "Checking in and out", "items": [{ "title": "Check-in after 3:00 PM" }] }
        ],
        "propertyLicenseTextList": ["MC3021/2019"]
    },
    "reviews": {
        "overallRating": 4.77,
        "overallCount": 219,
        "categoryRatings": [
            { "type": "CLEANLINESS", "rating": "4.6", "percentage": 0.92 }
            /* 5 more: ACCURACY, CHECKIN, COMMUNICATION, LOCATION, VALUE */
        ],
        "reviewTags": [
            { "name": "POOL", "localizedName": "Pool", "count": 61 },
            { "name": "HOSPITALITY", "localizedName": "Hospitality", "count": 54 }
        ],
        "items": [ /* paginated reviews when reviews=true */ ]
    },
    "flags": { "isGuestFavorite": true },
    "availability": { /* 12-month calendar when scrapeCalendar=true */ },
    "similarListings": [ /* 8 recommendations when scrapeSimilarListings=true */ ],
    "__metadata": {
        "scrapedAt": "2026-04-21T15:32:25.010Z",
        "sourceUrl": "https://www.airbnb.com/rooms/32124652",
        "listingGlobalId": "U3RheUxpc3Rpbmc6MzIxMjQ2NTI="
    }
}
```

Experience & service PDPs follow a similar flat shape, keyed on `pageType` (`experiences_pdp` / `services_pdp`), with `productType: "EXPERIENCE"` or `"SERVICE"` and the full activity-listing payload under `rawActivityListing`. Search results follow the `*_search` pageType and flatten to one record per hit.

## Pricing

Per-feature billing. You pay only for what you extract.

| Event | Price | Fires |
| --- | --- | --- |
| `apify-actor-start` | $0.0200 | Once per run |
| `stays-search-item` | $0.0010 | Per listing from a stays search — below or on par with every competing full-feature Airbnb scraper |
| `stays-pdp-item` | $0.0060 | Per rooms PDP with 80+ fields |
| `stays-pdp-reviews` | $0.0030 | Per stays PDP when `reviews: true` |
| `stays-pdp-calendar` | $0.0020 | Per stays PDP when `scrapeCalendar: true` — unique |
| `stays-pdp-similar` | $0.0020 | Per stays PDP when `scrapeSimilarListings: true` — unique |
| `experiences-search-item` | $0.0015 | Per experience from a search — exclusive to this scraper |
| `experiences-pdp-item` | $0.0080 | Per experience PDP — exclusive |
| `experiences-pdp-reviews` | $0.0015 | Per experience PDP when `reviews: true` |
| `services-search-item` | $0.0015 | Per service from a search — exclusive to this scraper |
| `services-pdp-item` | $0.0080 | Per service PDP — exclusive |
| `services-pdp-reviews` | $0.0015 | Per service PDP when `reviews: true` |

**Real budgets:**

| Use case | Config | Cost |
| --- | --- | --- |
| Daily check on one competitor's listing | 1 stays PDP + reviews + calendar + similar | **$0.0330** |
| Weekly refresh: all Marrakech listings, search-only | 500 search hits | **$0.5200** |
| Monthly snapshot: full city, with PDPs + reviews | 500 search + 500 PDPs × (base + reviews) | **$5.02** |
| Quarterly experiences inventory | 2,000 experience hits + 200 experience PDPs + reviews | **$4.92** |
| Services lead-gen sweep (NYC photographers) | 100 service hits + 100 service PDPs + reviews | **$1.12** |
| Everything-on stress test | 6 URL types, 110 dataset items, all enrichments | **$0.69** |

Set a `maxTotalChargeUsd` cap on any run and the scraper stops cleanly once the budget lands. No surprise invoices.

## Inputs

| Field | What it does |
| --- | --- |
| **startUrls** (required) | Array of Airbnb URLs. Mix rooms, experiences, services, and searches freely — they're classified automatically. Filters (dates, guests, price, property type, beds, service type tag) are read from the URL itself |
| **pagination** | How many pages per search URL. `1` = first page only, `0` = all pages |
| **pdpPage** | Enqueue every stays-search hit as a full stays PDP scrape |
| **experiencePage** | Enqueue every experiences-search hit as a full experience PDP scrape |
| **servicePage** | Enqueue every services-search hit as a full service PDP scrape |
| **reviews** | Fetch the reviews endpoint for any PDP type (stays, experiences, services). One switch, three billing events |
| **scrapeCalendar** | Fetch the 12-month availability calendar for every stays PDP |
| **scrapeSimilarListings** | Fetch the similar-listings carousel for every stays PDP |
| **proxyUrls** | Proxy configuration. **Residential strongly recommended** — datacenter IPs get challenged by Airbnb's bot defense |

## How it works

- **No browser.** The scraper calls Airbnb's internal GraphQL API directly using persisted-query hashes — the same pipeline Airbnb's own web frontend uses. Roughly 10× faster per listing than browser-based scrapers on the Apify Store, and far cheaper to run.
- **One architecture for three product lines.** Airbnb treats Experiences and Services as two flavors of the same `ActivityListing` GraphQL type — they share endpoints, ID encoding, and field shapes. This scraper inherits that unification: one set of handlers covers both, and if Airbnb launches another activity-flavored product, the plumbing is already there.
- **Residential proxy + sticky sessions.** Airbnb runs DataDome as its bot defense. Residential IPs pass clean; datacenter IPs get challenged. Each Crawlee session holds one sticky IP through the whole source's pagination, so DataDome never sees the fingerprint switch that trips up naive scrapers.
- **Self-healing session rotation.** On any 4xx or challenge page, the session retires and the next request picks up a fresh IP automatically. You won't see failed requests in normal operation.
- **Resilient to Airbnb's degraded-shell responses.** Under heavy load Airbnb sometimes ships a generic HTML shell with the site-wide title but full hydrated state in the embedded script tag. Other scrapers hit that edge case and emit `title: "Airbnb: Vacation Rentals…"` for every listing. This one reads the title from the hydrated JSON directly, so the output is correct regardless.

## Why this scraper beats the alternatives

| Alternative | Where it falls short |
| --- | --- |
| **Airbnb Partner API** | Invite-only, limited endpoints, requires business relationship with Airbnb |
| **Browser-based scrapers on Apify** | 3–10× slower per listing, 2–3× more expensive, more fragile against DOM changes |
| **Other GraphQL-style scrapers on Apify** | None support Experiences. None support Services. None support availability calendar or similar listings. None offer this granular per-feature billing — the closest competitor has 3 tiers; this scraper has 12, so search-only runs stay at the commodity rate and enrichment runs pay only for what they enable |
| **DIY from scratch** | You'd need to capture Airbnb's persisted-query hashes, handle DataDome rotation, manage sticky sessions, track schema drift, and rebuild when Airbnb ships a new build. This scraper does all of that |

## FAQ

**Why do I get two records per listing when `pdpPage` is on?**
By design. The search handler emits one card record per hit as soon as it parses the search page, and the PDP handler emits a separate rich record once it's fetched the full PDP (plus any enrichments you enabled). Both share the same `id`, so you can `JOIN ... ON id` to get one combined row per listing in your BI tool, or filter `pageType LIKE '%_pdp'` to see only the full records. This streams results faster and keeps search-only runs priced at the commodity rate.

**What's the pagination limit?**
The `pagination` input caps how many pages you follow per search URL — `1` (default) for the first page, `0` for every page Airbnb will give you, or any number up to 40. Airbnb itself caps inventory at around 15 pages per search (~270 stays, fewer experiences/services); when `nextPageCursor` comes back null the scraper stops regardless of your setting. To go deeper into a city, chunk the search by date range, price band, or map bounds — same technique Airbnb's own UI uses.

**Does this work from a free Apify plan?**
Yes. The default residential proxy is Apify's shared pool, no extra cost beyond the per-event prices above. Free-tier accounts get $5 in monthly credit — enough for ~10,000 search hits or ~1,500 full PDPs.

**Can I run this continuously?**
Yes. Schedule the actor from the Apify Console (cron / hourly / daily). Combined with a `maxTotalChargeUsd` cap per run, you get predictable monthly spend with per-listing granularity.

**Is the data live?**
Yes. Each run hits Airbnb's live GraphQL endpoint — no caching, no intermediate storage. A listing published five seconds before your run appears if Airbnb has indexed it.

**What's Services and why isn't anyone else scraping it?**
Services is Airbnb's 2025-launched product line covering vetted local professionals — photographers, chefs, personal trainers, massage therapists, and more. It uses the same internal `ActivityListing` GraphQL type as Experiences, so once you've built the experiences pipeline correctly, services is essentially free. Most existing scrapers were written before Services launched and nobody has gone back to add it.

**What if Airbnb rotates their internal build?**
The scraper fails fast on a `PersistedQuery not found` response, retires the session, and surfaces the rotation. In practice Airbnb rotates every few months; we track and ship updates within 24 hours.

**Privacy / compliance?**
Only public listing data and public review text. No private host contact info, no guest PII, no booking-level data. Residential proxies obey Airbnb's IP-level rate limits — sessions retire on throttle signals, not mask them.

**What about rate limits?**
Handled automatically. Each sticky session paginates conservatively; on any 4xx or challenge the session is retired and the work picks up on a fresh IP.

**How do I export?**
Dataset → **Export** in the Apify Console. CSV, JSON, JSONL, or XLSX with one click. Programmatically, fetch `/v2/datasets/{id}/items` to stream records, or push to BigQuery / Snowflake / S3 via Apify's built-in integrations.

**Can I request a custom field or enrichment?**
Yes. Open an issue via the **Issues** tab in the Apify Console or contact the actor owner. Simple field additions usually ship within a week.

## Support

Open an issue via the **Issues** tab in the Apify Console, or message the actor owner directly. We triage the same business day.

## Keywords

Airbnb scraper, Airbnb API, Airbnb data export, short-term rental data, STR market research, vacation rental scraper, Airbnb listings scraper, Airbnb reviews scraper, Airbnb experiences scraper, Airbnb services scraper, Airbnb host data, property management intelligence, vacation rental pricing data, Airbnb availability calendar, Airbnb JSON export, Airbnb GraphQL scraper.