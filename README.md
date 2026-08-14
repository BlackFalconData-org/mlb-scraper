# MLB Stats

Extract structured data from [statsapi.mlb.com](https://statsapi.mlb.com) — statsapi.mlb.com for every MLB player in a season: full roster with batting, pitching and fielding stats, plus club, league, division, venue, position, birth city and country, and debut year. One row per player as structured JSON or CSV.

**[MLB Stats on Apify →](https://apify.com/blackfalcondata/mlb-scraper?fpr=1h3gvi)**

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

**Search with filters** — Search by keyword. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch structured metadata for each player.

**Incremental mode** — Only get new or changed players since your last run. Content hash per player — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how players evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N players (up to 50.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every player returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured players from statsapi.mlb.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor players, track trends, and analyze market dynamics with structured, deduplicated data from statsapi.mlb.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per player is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "Ohtani",
  "maxResults": 5,
  "includeDetails": true,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "emitUnchanged": false,
  "emitExpired": false,
  "skipReposts": false,
  "notificationLimit": 5,
  "notifyOnlyChanges": false,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Name to match against the season roster — a full or partial player name (e.g. "Ohtani", "Aaron Judge", "Rodriguez"). Matching is case-insensitive and substring-based. LEAVE EMPTY to get the complete roster for the season. |
| `season` | integer | — | Season year to pull, e.g. 2026. Defaults to the current year. Historical seasons are served the same way. |
| `sportId` | integer | — | 1 = Major League Baseball (default). 11 = Triple-A, 12 = Double-A, 13 = High-A, 14 = Single-A. |
| `startUrls` | array | `[]` | Paste raw search URLs from the target site. Each URL becomes its own search task; results are merged and deduped by record ID across all URLs. When provided AND parseable, startUrls REPLACE the query field (each URL becomes one task). Implement parseStartUrl() in src/searchTasks.ts for your target site — without it, all URLs are skipped and the actor falls back to query. |
| `maxResults` | integer | `25` | Maximum total records (0 = unlimited — the whole roster). |
| `includeDetails` | boolean | `true` | Fetch full records — season stats, biography, position, handedness and current club. Turn off for a faster, name-only run. |
| `includeRosterStatus` | boolean | `false` | Add each player's current roster status — Active, Injured 10-Day, Injured 60-Day, Reassigned to Minors, Released — plus the date it took effect. Roughly doubles the amount of data fetched per player. Requires 📋 Include Full Details; without it this option is skipped and not charged. |
| `includeAwards` | boolean | `false` | Add All-Star and Futures Game selections and similar honours, with the season each was won. Adds about 60% to the data fetched per player. Needs 📋 Include Full Details switched on — otherwise it is skipped, and not charged. |
| `includeTransactions` | boolean | `false` | Add signings, trades and injured-list placements with their dates and the league's own wording. This is where injury dates come from. Substantially increases the data fetched per player — leave off unless you need the history. Only runs when 📋 Include Full Details is on; skipped and not charged otherwise. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, startUrls) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `emitUnchanged` | boolean | `false` | When incremental mode is on, also emit records whose content has not changed since the last run. |
| `emitExpired` | boolean | `false` | When incremental mode is on, also emit records that were seen before but are no longer found. |
| `skipReposts` | boolean | `false` | When incremental, skip records whose content matches an expired record from a prior run (cross-run duplicate detection). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `notificationLimit` | integer | `5` | Maximum number of records included in each notification message (1–20). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED records. Has no effect outside incremental mode. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results. Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |

---

## Output fields

Every player returns the same 96-field schema. Missing values are `null` — never omitted.

- `listingId`
- `statsSeason`
- `hittingStats`
- `pitchingStats`
- `fieldingStats`
- `gamesPlayed`
- `atBats`
- `hits`
- `doubles`
- `triples`
- `homeRuns`
- `rbi`
- `runs`
- `strikeOuts`
- `baseOnBalls`
- `stolenBases`
- `battingAverage`
- `onBasePercentage`
- `sluggingPct`
- `ops`
- `era`
- `inningsPitched`
- `wins`
- `losses`
- `saves`
- `whip`
- `college`
- `highSchool`
- `rosterStatus`
- `rosterStatusCode`
- `rosterStatusDate`
- `isActiveRoster`
- `isOn40Man`
- `awards`
- `transactions`
- `recordType`
- `teamId`
- `name`
- `teamName`
- `clubName`
- `franchiseName`
- `abbreviation`
- `teamCode`
- `locationName`
- `league`
- `leagueId`
- `division`
- `divisionId`
- `venueId`
- `venueName`
- `springVenueId`
- `fileCode`
- `springLeagueId`
- `springLeague`
- `firstYearOfPlay`
- `season`
- `active`
- `url`
- `portalUrl`
- `playerId`
- `fullName`
- `firstName`
- `middleName`
- `lastName`
- `useName`
- `useLastName`
- `boxscoreName`
- `nameSlug`
- `primaryNumber`
- `primaryPosition`
- `batSide`
- `pitchHand`
- `currentTeam`
- `birthDate`
- `currentAge`
- `birthCity`
- `birthStateProvince`
- `birthCountry`
- `height`
- `weight`
- `gender`
- `isPlayer`
- `isVerified`
- `draftYear`
- `mlbDebutDate`
- `strikeZoneTop`
- `strikeZoneBottom`
- `link`
- `headshotUrl`
- `searchQuery`
- `contentHash`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `changeType`

---

## Sample output

One object per player. Here is a real example from a production run:

```json
{
  "listingId": "0fac9d9cfbdd954979ad050d3e051c6c67ad493ad02fc9a0331b5393c940824d",
  "statsSeason": 2026,
  "hittingStats": null,
  "pitchingStats": {
    "age": 27,
    "gamesPlayed": 24,
    "gamesStarted": 24,
    "groundOuts": 118,
    "airOuts": 157,
    "runs": 60,
    "doubles": 15,
    "triples": 1,
    "homeRuns": 17,
    "strikeOuts": 99,
    "baseOnBalls": 61,
    "intentionalWalks": 0,
    "hits": 118,
    "hitByPitch": 0,
    "avg": ".242",
    "atBats": 487,
    "obp": ".324",
    "slg": ".382",
    "ops": ".706",
    "caughtStealing": 4,
    "stolenBases": 21,
    "stolenBasePercentage": ".840",
    "caughtStealingPercentage": ".160",
    "groundIntoDoublePlay": 6,
    "numberOfPitches": 2223,
    "era": "3.92",
    "inningsPitched": "128.2",
    "wins": 6,
    "losses": 7,
    "saves": 0,
    "saveOpportunities": 0,
    "holds": 0,
    "blownSaves": 0,
    "earnedRuns": 56,
    "whip": "1.39",
    "battersFaced": 555,
    "outs": 386,
    "gamesPitched": 24,
    "completeGames": 0,
    "shutouts": 0,
    "strikes": 1388,
    "strikePercentage": ".620",
    "hitBatsmen": 0,
    "balks": 0,
    "wildPitches": 0,
    "pickoffs": 2,
    "totalBases": 186,
    "groundOutsToAirouts": "0.75",
    "winPercentage": ".462",
    "pitchesPerInning": "17.28",
    "gamesFinished": 0,
    "strikeoutWalkRatio": "1.62",
    "strikeoutsPer9Inn": "6.92",
    "walksPer9Inn": "4.27",
    "hitsPer9Inn": "8.25",
    "runsScoredPer9": "4.20",
    "homeRunsPer9": "1.19",
    "inheritedRunners": 0,
    "inheritedRunnersScored": 0,
    "catchersInterference": 2,
    "sacBunts": 0,
    "sacFlies": 5
  },
  "fieldingStats": {
    "age": 27,
    "gamesPlayed": 24,
    "gamesStarted": 24,
    "assists": 9,
    "putOuts": 1,
    "errors": 1,
    "chances": 11,
    "fielding": ".909",
    "position": {
      "code": "1",
      "name": "Pitcher",
      "type": "Pitcher",
      "abbreviation": "P"
    },
    "rangeFactorPerGame": "0.42",
    "rangeFactorPer9Inn": "0.70",
    "innings": "128.2",
    "games": 24,
    "doublePlays": 0,
    "triplePlays": 0,
    "throwingErrors": 1
  },
  "gamesPlayed": 24,
  "atBats": null,
  "hits": null,
  "doubles": null,
  "triples": null,
  "homeRuns": null,
  "rbi": null
}
```

*Truncated — full records contain 96 fields. See Output fields for the complete schema.*

**[Try MLB Stats now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/mlb-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Record returned | $0.002 |
| Roster & injury status | $0.0015 |
| Awards | $0.0015 |
| Transaction history | $0.0025 |

See the [actor on Apify](https://apify.com/blackfalcondata/mlb-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape statsapi.mlb.com?**
Use this actor on Apify to extract structured data from statsapi.mlb.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get statsapi.mlb.com data as JSON, CSV, or Excel?**
The actor writes each player to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape statsapi.mlb.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check statsapi.mlb.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each player gets a content hash. On subsequent runs, only new or changed players are emitted — saving time, compute, and storage. Expired players can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [MLB Stats](https://apify.com/blackfalcondata/mlb-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 08*
