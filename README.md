# AliExpress Israel Skill

Dev / staging repo for AliExpress-related skills before they ship to the **Israel Shopping plugin**.

> **Status**: ✅ Initial integration done. The plugin (`Claude-Israel-Shopping-Plugin`) now ships `search-aliexpress`, `fetch-listing` (no-auth scraper), and `fetch-listing-api` (stub, official Affiliate API). This repo continues to host research and pre-ship validation for new scraper logic, locale handling, and signing flows.

## Role

- **Sandbox for new logic** — validate scraper changes, locale switching, and API signing patterns here before they land in the plugin.
- **Research store** — `research/cookies/`, `research/ui-selectors/` — DOM and cookie observations from live AliExpress sessions.
- **Reference index** — links to alternative scrapers, official-API references, and the upstream packages we depend on.

## Already shipped to the plugin

| Skill | Repo path | Status |
|---|---|---|
| `search-aliexpress` | `Claude-Israel-Shopping-Plugin/skills/search-aliexpress/` | ✅ Playwright, IL/ILS/Hebrew |
| `fetch-listing` | `Claude-Israel-Shopping-Plugin/skills/fetch-listing/` | ✅ No-auth, default en/USD |
| `fetch-listing-api` | `Claude-Israel-Shopping-Plugin/skills/fetch-listing-api/` | 🟡 Stub — awaiting credential validation |
| `compare-to-local` | `Claude-Israel-Shopping-Plugin/skills/compare-to-local/` | ✅ |
| `il-reviews-show` | `Claude-Israel-Shopping-Plugin/skills/il-reviews-show/` | ✅ |
| `free-shipping-only` | `Claude-Israel-Shopping-Plugin/skills/free-shipping-only/` | ✅ |
| `exclude-combo-deals` | `Claude-Israel-Shopping-Plugin/skills/exclude-combo-deals/` | ✅ |

## Two scraping paths

### 1. No-auth (Puppeteer) — current default
- Library: [`sudheer-ranga/aliexpress-product-scraper`](https://github.com/sudheer-ranga/aliexpress-product-scraper)
- Pros: zero setup, works today
- Cons: brittle (DOM rotation, anti-bot), no rate limit guarantees

### 2. Authenticated (Affiliate API) — preferred long-term
- Endpoint: `https://api-sg.aliexpress.com/sync`
- Methods: `aliexpress.affiliate.productdetail.get`, `aliexpress.affiliate.product.query`
- Auth: HMAC-SHA256 signing with App Key + App Secret + Access Token
- Validation target: Daniel's `DSR Holdings Purchasing` app (Affiliates → Individual)
- Reference impl: [AdirCohen333/FindMyDeal `index.js`](https://github.com/AdirCohen333/FindMyDeal) — uses the same signing pattern against the DS endpoints

## Fallback scrapers (in case sudheer-ranga breaks)

- [`omkarcloud/aliexpress-scraper`](https://github.com/omkarcloud/aliexpress-scraper)
- [`oxylabs/aliexpress-scraper`](https://github.com/oxylabs/aliexpress-scraper)
- [`darwiish1337/scraper_md`](https://github.com/darwiish1337/scraper_md)
- [`BrenoFariasdaSilva/E-Commerces-WebScraper`](https://github.com/BrenoFariasdaSilva/E-Commerces-WebScraper)

## Israel-specific concerns (validated)

- **VAT**: 0–$75 USD exempt; $75–$500 → 18% VAT on (item + shipping); >$500 → customs/duty applies. The `fetch-listing` skill computes both bare and VAT-inclusive landed cost so the user picks which to use for comparisons.
- **FX**: Live USD/ILS via [frankfurter.app](https://www.frankfurter.app) (ECB rates, no key), 24h file cache.
- **Locale**: Default `en/USD`. Hebrew/ILS via documented cookies (`c_tp=ILS`, `b_locale=iw_IL`) — see `research/cookies/`.
- **Shipping labels**: AliExpress's own "free shipping" filter often bakes the fee into the unit price, but it produces a reliable flat number — useful for fast comparisons.

## Integration target

`danielrosehill/Claude-Israel-Shopping-Plugin` — sibling to other Israel agent skills under the broader Israel-Agent-Skills ecosystem.
