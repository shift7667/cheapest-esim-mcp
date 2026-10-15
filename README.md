# Cheapest eSIM Korea — travel eSIM price MCP server (최저가e심)

Verified **daily prices of travel eSIMs and physical SIMs** sold to Korean travelers, for **54 destination countries** and **1–30 day trips** — as a free, no-auth **remote MCP server** and a public JSON API.

- **MCP endpoint (Streamable HTTP):** `https://pyungyo.com/esim/mcp`
- **Public API:** `https://pyungyo.com/esim/api/v1` · [OpenAPI 3.1](https://pyungyo.com/esim/api/v1/openapi.json)
- **Website:** https://pyungyo.com/esim · **Docs:** https://pyungyo.com/esim/developers
- **Auth:** none · **Cost:** free · **Data license:** CC BY 4.0

Every result ends with a citation line, e.g. `출처: 최저가e심(pyungyo.com/esim), 2026-10-15 기준`, so assistants can attribute the source and as-of date.

## Tools

All tools are read-only (`readOnlyHint: true`).

| Tool | What it does | Inputs |
|---|---|---|
| `find_esim_prices` | Sellers and the cheapest price for a country and trip length | `country` (e.g. `japan` or `일본`), `days` 1–30, `type` `unlimited`/`daily`/`total`, `gb`, `form` `esim`/`usim`, `limit` |
| `compare_countries` | Countries ranked by cheapest unlimited eSIM for the same trip length | `days`, `form`, `limit` |
| `get_price_trend` | Recent daily cheapest-price history and "is today cheap?" judgement (no forecasting) | `country`, `days` (1, 3, 5, 7, 10, 15, 30), `form` |
| `get_seller_profile` | How often a seller changed prices, sold-out ratio, cells where it is cheapest today | `seller` |
| `list_countries` | Supported countries | — |

### Example

> "What's the cheapest 5-day unlimited eSIM for Japan?"

`find_esim_prices {"country": "japan", "days": 5}` →

```
일본 eSIM 5일 무제한: 화면 최저가 10,000원 (나무커머스, 판매처 N곳 비교)
1. 도시락이심 12,560원 · 리뷰 136,166 · …
…
출처: 최저가e심(pyungyo.com/esim), 2026-10-15 기준 · 매일 20:00 KST 수집·검증 · https://pyungyo.com/esim
```

`structuredContent` carries the full JSON (sellers, prices, reviews, options, product URLs, as-of time, license, citation).

## Connect

- **Claude:** Settings → Connectors → Add custom connector → URL `https://pyungyo.com/esim/mcp`
- **ChatGPT:** Settings → Apps & Connectors → Advanced (developer mode) → Create → MCP server URL `https://pyungyo.com/esim/mcp`, no authentication
- **Any MCP client (remote/Streamable HTTP):**

```json
{
  "mcpServers": {
    "cheapest-esim": { "type": "http", "url": "https://pyungyo.com/esim/mcp" }
  }
}
```

## How the data is made

- Every day at 20:00 KST, option-level prices of travel eSIM/SIM products on **Naver SmartStore** are collected from each seller's product API.
- Only data that passes automatic verification (live re-check of a sample, integrity checks, screen-text audit) is published; failed runs keep the previous day's data.
- Unlimited lists include only data-only, no-throttling options; call plans, slow-after-cap, 1+1 bundles and sold-out options are excluded. SIM prices include shipping.
- "Cheapest" (`pagePrice`) follows the same rule as the website: sellers with ≥1,000 reviews (filled up to 5 sellers by review count when fewer), bundles excluded, the cheapest single option per seller.
- No recommendation scores and no paid placement: sellers are ordered by review count.

## Limits

120 requests/min per IP for the API, 300/min for MCP. For bulk use, download the daily CSV: https://pyungyo.com/esim/data/esim-price-trends.csv

## License & citation

Data: **CC BY 4.0** — cite as `최저가e심(pyungyo.com/esim), <as-of date>`. Prices change daily; always include the date. Purchases happen on the seller's store; we are not affiliated with any seller.

---

## 한국어 안내

해외여행 eSIM·실물 유심 판매처(네이버 스마트스토어) 가격을 **매일 20:00(KST)에 수집·검증**해 54개국·1~30일로 비교한 데이터를 MCP 서버와 공개 API로 무료 제공합니다.

- MCP 주소: `https://pyungyo.com/esim/mcp` (인증 없음, 읽기 전용 도구 5개)
- 공개 API: `https://pyungyo.com/esim/api/v1`
- 안내: https://pyungyo.com/esim/developers
- 인용: "최저가e심(pyungyo.com/esim), 기준일" (CC BY 4.0)

운영: 케이코인(KCOIN) · 문의 wmask@naver.com
