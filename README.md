# 4zida.rs Scraper

Extract structured data from [4zida.rs](https://4zida.rs) — 4zida.rs property listings for sale or rent across Serbia. Every record carries price, area and rooms. Search by city, a pasted search URL or a radius around any point and get full photos and agency details on each listing. Ready for a spreadsheet or CRM.

**[4zida.rs Scraper on Apify →](https://apify.com/blackfalcondata/4zida-rs-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch full descriptions, structured metadata for each listing.

**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how listings evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N listings (up to 100.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from 4zida.rs on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from 4zida.rs.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "apartments for sale",
  "location": "Beograd",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Property type + deal type, e.g. "apartments for sale", "houses rent", "offices sale", "lots sale", "garages rent" (Serbian words also work: "stanovi prodaja", "kuce izdavanje"). You can also paste a 4zida.rs search URL such as https://www.4zida.rs/prodaja-stanova/beograd. Use a JSON array for several searches in one run, e.g. ["apartments for sale","houses for sale"]. |
| `location` | string | — | City, municipality or neighbourhood as written on 4zida.rs ("Beograd", "Novi Sad", "Novi Beograd", "Vračar"). Leave empty to search all of Serbia. Use a JSON array for several locations, e.g. ["Beograd","Novi Sad"]. |
| `maxResults` | integer | `100` | Maximum total listings (0 = unlimited). |
| `startUrls` | array | `[]` | Paste raw 4zida.rs search URLs (e.g. https://www.4zida.rs/izdavanje-stanova/novi-sad). Each URL becomes its own search task; results are merged and deduped by listing ID. When provided, startUrls REPLACE the query/location fields (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Fetch each listing's detail page for the full description, all photos, coordinates, agency and amenities. Turn off for faster, cheaper runs with search-card fields only. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (re-listed property detection). |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit listings whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit listings that were seen before but are no longer found. |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "381641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |
| `lat` | number | — | Latitude of the centre point for a radius search. Set lat, lon and radiusKm together to get every listing within that distance, nearest first; location is then left out. |
| `lon` | number | — | Longitude of the centre point for a radius search. Use together with lat and radiusKm. |
| `radiusKm` | integer | — | Distance in kilometres around lat/lon. Every listing inside the circle is returned, nearest first. |

---

## Output fields

Every listing returns the same 56-field schema. Missing values are `null` — never omitted.

- `id`
- `url`
- `portalUrl`
- `transaction`
- `propertyType`
- `price`
- `pricePerSqm`
- `priceCurrency`
- `area`
- `rooms`
- `floor`
- `totalFloors`
- `lastFloor`
- `heatingType`
- `state`
- `registered`
- `structureLabel`
- `address`
- `neighborhood`
- `city`
- `placeTitles`
- `agencyUrl`
- `images`
- `imageCount`
- `createdAt`
- `scrapedAt`
- `title`
- `category`
- `location`
- `previousPrice`
- `furnished`
- `elevator`
- `yearBuilt`
- `description`
- `imageUrl`
- `agencyName`
- `agentName`
- `agentPhones`
- `sellerType`
- `latitude`
- `longitude`
- `bathrooms`
- `hasTerrace`
- `hasGarage`
- `features`
- `status`
- `checkedAt`
- `virtual3dTour`
- `virtual3dTourUrl`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "id": "6ad5165f13c7bb15670a9593",
  "url": "https://www.4zida.rs/prodaja-stanova/mali-mokri-lug-zvezdara-opstina-beograd/jednoiposoban-stan/6ad5165f13c7bb15670a9593",
  "portalUrl": "https://www.4zida.rs/prodaja-stanova/mali-mokri-lug-zvezdara-opstina-beograd/jednoiposoban-stan/6ad5165f13c7bb15670a9593",
  "transaction": "sale",
  "propertyType": "apartment",
  "price": 98625,
  "pricePerSqm": 2529,
  "priceCurrency": "EUR",
  "area": 39,
  "rooms": 1.5,
  "floor": 3,
  "totalFloors": 5
}
```

*Truncated — full records contain 56 fields. See Output fields for the complete schema.*

**[Try 4zida.rs Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/4zida-rs-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.000045 |
| Result | $0.00081 |

See the [actor on Apify](https://apify.com/blackfalcondata/4zida-rs-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape 4zida.rs?**
Use this actor on Apify to extract structured data from 4zida.rs. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get 4zida.rs data as JSON, CSV, or Excel?**
The actor writes each listing to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape 4zida.rs?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check 4zida.rs's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Expired listings can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [4zida.rs Scraper](https://apify.com/blackfalcondata/4zida-rs-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
