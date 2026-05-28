# Nazar &nbsp;·&nbsp; नज़र

**Price intelligence for Instamart. Food trends for your neighbourhood.**

---

Swiggy has the data. None of it reaches the user.

Instamart runs a Mega Savings Festival — up to 60% off — on the first 3 days of every month. Prices on everyday groceries shift daily. New restaurants in your area open and trend before anyone notices. None of this is surfaced. There is no alert, no watchlist, no trend feed, no history graph anywhere in the Swiggy ecosystem today.

> *"Swiggy does not have a price drop alert function. Users have to track prices manually — open the app, search for the item, write down the price in a notebook."*  
> — NewsBytesApp, January 2025

Nazar is the layer that fixes this. Built on Swiggy's MCP APIs.

---

## What it does

### 1. Instamart Price Watcher

Nazar reads a user's Instamart order history and automatically begins tracking prices for everything they buy regularly — atta, dal, tel, sugar, doodh. No manual setup. The moment a price drops, or the Mega Sale is 24 hours away, the user gets a WhatsApp alert.

```
┌─────────────────────────────────────────────────────┐
│  Nazar · Price Alert · 7:42 AM                      │
│                                                     │
│  Fortune Sunflower Oil 1L                           │
│  ₹152  →  ₹134  ↓ ₹18                              │
│                                                     │
│  Lowest price in 6 weeks.                           │
│  Mega Sale ends in 14 hours.                        │
│                                                     │
│  Open Instamart →                                   │
└─────────────────────────────────────────────────────┘
```

| Feature | Description |
|---|---|
| Auto watchlist | Built from order history — nothing to configure |
| Price history | 30 / 60 / 90 day chart per item |
| Drop alerts | WhatsApp + push when price falls below threshold |
| Mega Sale reminder | Automatic alert before Day 1–3 of every month |
| Buy timing | AI detects each item's price pattern — tells you when it's historically cheapest |
| Cross-platform | Same item's price history on Instamart vs Blinkit vs Zepto |
| One-tap order | Alert → tap → Instamart cart pre-loaded |

---

### 2. Hyperlocal Trend Finder

Swiggy publishes one national data report per year. What's missing is neighbourhood-level, week-by-week signal — what your specific area is actually ordering right now, which new restaurants are gaining traction before they go mainstream, which dishes are quietly surging.

Nazar reads order volume signals from the Food and Dineout MCP APIs, aggregates by pin code, ranks by velocity, and delivers a weekly digest every Monday morning.

```
┌─────────────────────────────────────────────────────┐
│  Nazar · Your Area This Week · Kalawad Rd, Rajkot   │
│                                                     │
│  Trending dishes                                    │
│  Pav Bhaji          ↑ 34%                           │
│  Cold Coffee        ↑ 41%  (post-3pm orders)        │
│  Paneer Tikka       ↑ 28%  (new cloud kitchen)      │
│                                                     │
│  New & already rising                               │
│  Grill House · Opened 12 days ago · Growing fast    │
└─────────────────────────────────────────────────────┘
```

| Feature | Description |
|---|---|
| Neighbourhood trends | Dishes and restaurants gaining velocity in your pin code this week |
| Early signal | New restaurants trending before they're widely known |
| Dish-level | Not just which restaurant is popular — which specific dish is surging |
| Weekly digest | Clean WhatsApp summary every Monday, 8 AM |

---

## Why this gap exists

| Product | What it does | Why it falls short |
|---|---|---|
| MetricsCart / 42Signals | Tracks Instamart prices | B2B tool for brands like Amul monitoring their own SKUs. Not for consumers. |
| Savvio / Quick Compare | Instamart vs Blinkit price today | Point-in-time only. No history, no alerts, no pattern detection. |
| Keepa / CamelCamelCamel | Amazon India price history | Best-in-class for Amazon. Zero Instamart coverage. Groceries aren't on Amazon. |
| Swiggy app | — | No price history. No watchlist. No alerts. Confirmed absent feature. |
| Zomato Trends | Restaurant demand data | Built for restaurant operators. Not consumer-facing. City-level, not hyperlocal. |

Keepa for Amazon is a beloved, widely-used product. Nazar is Keepa for Instamart — with hyperlocal food intelligence built on top. This specific combination does not exist anywhere.

---

## Architecture

```
                        ┌─────────────────────┐
                        │     React Web App    │
                        │  Price charts        │
                        │  Trend feed          │
                        │  Watchlist dashboard │
                        └──────────┬──────────┘
                                   │
              ┌────────────────────┼────────────────────┐
              │                    │                    │
     WhatsApp Alert           Push Notification      Web Dashboard
     (price drops,            (mobile browser)      (full history)
      weekly digest)
              │                    │                    │
              └────────────────────▼────────────────────┘
                                   │
                        ┌──────────▼──────────┐
                        │   Node.js API        │
                        │   Express + BullMQ   │
                        └──────────┬──────────┘
                                   │
              ┌────────────────────┼────────────────────┐
              │                    │                    │
    ┌─────────▼────────┐  ┌────────▼───────┐  ┌────────▼───────┐
    │  Price Engine     │  │ Trend Engine   │  │ Alert Queue    │
    │                   │  │                │  │                │
    │ Polls item prices │  │ Aggregates     │  │ Redis-backed   │
    │ every 6 hours     │  │ order volume   │  │ Fires on drop  │
    │ Diffs vs history  │  │ by pin code    │  │ or schedule    │
    │ Triggers alerts   │  │ weekly         │  │                │
    └─────────┬─────────┘  └────────┬───────┘  └────────────────┘
              │                     │
              └─────────────────────▼
                                    │
         ┌──────────────────────────┼──────────────────────────┐
         │                          │                          │
┌────────▼────────┐      ┌──────────▼────────┐      ┌─────────▼──────────┐
│  Instamart MCP  │      │    Food MCP        │      │   Dineout MCP      │
│                 │      │                    │      │                    │
│ search_products │      │ search_restaurants │      │ search_restaurants │
│ get_product_    │      │ get_restaurant_    │      │ Venue trends       │
│ details         │      │ menu               │      │ by area            │
└────────┬────────┘      └──────────┬─────────┘      └────────────────────┘
         │                          │
         └──────────────────────────▼
                                    │
                         ┌──────────▼────────┐
                         │    Claude API      │
                         │                   │
                         │ Writes alert copy  │
                         │ Trend summaries    │
                         │ Buy timing advice  │
                         └──────────┬─────────┘
                                    │
              ┌─────────────────────┴────────────────────┐
              │                                          │
   ┌──────────▼──────────┐                  ┌───────────▼──────────┐
   │     PostgreSQL       │                  │        Redis          │
   │                      │                  │                       │
   │ price_history        │                  │ Alert job queue       │
   │ user_watchlists      │                  │ Rate limit counters   │
   │ item_master          │                  │ API response cache    │
   │ trend_snapshots      │                  │                       │
   └─────────────────────┘                  └──────────────────────┘
```

---

## MCP integration detail

| Server | Tool | Used for |
|---|---|---|
| Instamart | `search_products` | Find user's tracked items by name and variant (1kg, 5kg, etc.) |
| Instamart | `get_product_details` | Current price, MRP, discount %, availability |
| Food | `search_restaurants` | Restaurant discovery by pin code for trend aggregation |
| Food | `get_restaurant_menu` | Dish-level order signal tracking |
| Dineout | `search_restaurants` | Venue trends by neighbourhood |

Auth flow: Swiggy OAuth 2.0. User connects their account once. Nazar reads order history to auto-populate the watchlist. No manual item entry required.

---

## Stack

| Layer | Technology |
|---|---|
| Frontend | React, TypeScript, Recharts |
| Backend | Node.js, Express, BullMQ |
| Database | PostgreSQL (price history, watchlists), Redis (alert queue, cache) |
| AI | Claude API — alert copy, trend cards, buy timing |
| Alerts | WhatsApp Business API, Web Push |
| Auth | Swiggy OAuth 2.0 via MCP |
| Infra | Railway (backend), Vercel (frontend) |

---

## Roadmap

**v1 — Core**
- Auto-watchlist from Instamart order history
- Price history graph (30 days)
- WhatsApp drop alerts
- Mega Sale Festival monthly reminder

**v2 — Intelligence**
- 60 / 90 day history, buy timing advice
- Cross-platform price comparison
- Hyperlocal trend finder — weekly digest by pin code
- New restaurant early signal detection

**v3 — Reach**
- React Native mobile app
- Shareable neighbourhood trend reports
- Household mode — shared watchlist for a family
- Price alert forwarding to WhatsApp groups

---

*Nazar is being submitted to the Swiggy Builders Club program.*  
*Built on Swiggy Food, Instamart, and Dineout MCP APIs.*
