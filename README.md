<div align="center">
<br/>

# 👁️ &nbsp;N A Z A R&nbsp; &nbsp;नज़र
### *The consumer intelligence layer Swiggy never built — but every Indian needs.*

<br/>

> **Instamart grocery prices change every day.**  
> **Swiggy's Mega Sale runs the first 3 days of every month.**  
> **What's trending in your neighbourhood changes every week.**  
>
> **Swiggy shows you none of this.**  
> **Nazar does.**

<br/>

![Swiggy Instamart](https://img.shields.io/badge/Swiggy-Instamart%20MCP-FC8019?style=for-the-badge&logoColor=white)
![Swiggy Food](https://img.shields.io/badge/Swiggy-Food%20MCP-1D9E75?style=for-the-badge&logoColor=white)
![Swiggy Dineout](https://img.shields.io/badge/Swiggy-Dineout%20MCP-534AB7?style=for-the-badge&logoColor=white)
![Claude AI](https://img.shields.io/badge/AI-Claude%20API-CC785C?style=for-the-badge&logoColor=white)

<br/>
</div>

---

## The problem is embarrassingly simple

An Indian household buys the same 12 items on Instamart every single week.  
Atta. Dal. Chawal. Tel. Doodh. Cheeni. The list doesn't change.

The prices do — sometimes daily. Swiggy runs a **Mega Savings Festival** with up to 60% off, every month, on the first 3 days. Most people miss it every single month because Swiggy sends no reminder, no alert, nothing.

The current state-of-the-art solution for tracking Instamart prices?

```
A notebook.
```

> *"Swiggy does not have a price drop alert function. Users have to keep track  
> of prices manually — open the app, search for the item, write down the price."*  
> — NewsBytesApp, January 2025

This is the gap. A product that 19.8 million monthly Swiggy users need, that no one has built.

---

## What Nazar is

Nazar is a **consumer AI agent** built on Swiggy's MCP APIs. It does two things:

### 🛒 &nbsp;Part 1 — Instamart Price Watcher

Nazar connects to a user's Swiggy account, reads their Instamart order history, and automatically starts tracking prices for everything they regularly buy — no manual setup, no typing item names.

From that moment, it builds a price history graph for each item and watches for the right moment to buy.

```
┌─────────────────────────────────────────────────────────────┐
│  📲  WhatsApp alert · 7:42 AM                               │
│                                                             │
│  👁️ Nazar spotted a deal                                    │
│                                                             │
│  Fortune Sunflower Oil 1L                                   │
│  ₹152  →  ₹134   ↓ ₹18 (12% drop)                         │
│                                                             │
│  This is the lowest price in 6 weeks.                       │
│  Mega Sale ends in 14 hours.                                │
│                                                             │
│  [Order on Instamart →]                                     │
└─────────────────────────────────────────────────────────────┘
```

**What it tracks:**

| Feature | Description |
|---|---|
| **Auto Watchlist** | Built from your Instamart order history — no manual entry |
| **Price History Graph** | 30 / 60 / 90 day price chart per item |
| **Drop Alerts** | WhatsApp + push the moment price falls below your threshold |
| **Mega Sale Reminder** | Pre-scheduled alert every month, 24hrs before Day 1-3 sale starts |
| **Smart Buy Timing** | AI learns each item's price pattern — tells you when it's historically lowest |
| **Cross-Platform Check** | Same item's current price on Instamart vs Blinkit vs Zepto |
| **One-Tap Order** | Alert → tap → Instamart cart pre-loaded. Zero friction. |

---

### 📍 &nbsp;Part 2 — Hyperlocal Trend Finder

Swiggy publishes one national report per year. Biryani is #1 — has been for a decade. We know.

What Swiggy has never shown anyone: **what your specific neighbourhood is ordering right now, this week.**

Nazar reads order volume signals across Food and Dineout MCP APIs, aggregates by pin code, and surfaces what's actually trending near you — not based on paid ads or editorial picks, but on real order velocity.

```
┌─────────────────────────────────────────────────────────────┐
│  📍  Your area · Kalawad Road, Rajkot · This week           │
│                                                             │
│  🔥  Trending dishes                                        │
│      Pav Bhaji          ↑ 34%  (3 restaurants, same dish)  │
│      Paneer Tikka       ↑ 28%  (new cloud kitchen)         │
│      Cold Coffee        ↑ 41%  (post-3pm orders only)      │
│                                                             │
│  🆕  New & already trending                                 │
│      Grill House Rajkot · Opened 12 days ago · ↑ Fast      │
│                                                             │
│  📬  Weekly digest sent every Monday 8 AM                   │
└─────────────────────────────────────────────────────────────┘
```

**What it surfaces:**

| Feature | Description |
|---|---|
| **Neighbourhood Trends** | Dishes + restaurants gaining order velocity in your pin code this week |
| **Early Mover Signal** | Spots new restaurants already trending before they go mainstream |
| **Dish-Level Insight** | Not just "restaurant is popular" — specifically which dishes are surging |
| **Weekly Digest** | Every Monday: a clean WhatsApp summary of what's hot near you |
| **AI Trend Cards** | Claude writes human-readable summaries — not raw numbers, actual insight |

---

## Why this doesn't exist yet

We searched every Indian app store, product hunt, and known price tracker. Here is what we found:

| Existing product | What it does | Why it is not Nazar |
|---|---|---|
| **MetricsCart / 42Signals** | Tracks Instamart prices for FMCG brands | Enterprise B2B. For Amul tracking their own shelf. Costs ₹lakhs/month. Not consumer. |
| **Savvio / Quick Compare** | Instamart vs Blinkit price comparison | Point-in-time snapshot only. No history, no alerts, no timing intelligence. |
| **Keepa / CamelCamelCamel** | Amazon India price history + alerts | Outstanding product — but not a single Instamart item in their database. |
| **Swiggy app** | Everything | Zero price history. Zero watchlist. Zero alerts. Confirmed missing feature as of 2025. |
| **Zomato Trends** | Restaurant demand data | Built for restaurant owners adjusting their menus. Not consumer-facing. |
| **Swiggy annual report** | "93 million biryanis ordered" | National. Annual. Tells you nothing about your street, this week. |

**The gap in one line:** Keepa exists for Amazon. Nazar is Keepa for Instamart — plus the hyperlocal food intelligence layer that no one has built anywhere in India.

---

## Architecture

```
╔══════════════════════════════════════════════════════════════════════╗
║                        NAZAR — FULL SYSTEM                          ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║   USER SURFACES                                                      ║
║   ┌──────────────┐    ┌─────────────────┐    ┌───────────────────┐  ║
║   │  React Web   │    │    WhatsApp     │    │   Web Push / PWA  │  ║
║   │  Dashboard   │    │  (Alerts Bot)   │    │  (Mobile Browser) │  ║
║   │  Price charts│    │  Price drops    │    │  Trend digests    │  ║
║   │  Trend map   │    │  Trend digests  │    │  Reminders        │  ║
║   └──────┬───────┘    └────────┬────────┘    └─────────┬─────────┘  ║
║          └────────────────────▼────────────────────────┘            ║
║                                │                                     ║
║                    ┌───────────▼────────────┐                       ║
║                    │    Node.js API Server  │                       ║
║                    │  Express + BullMQ Jobs │                       ║
║                    └─────────┬──────────────┘                       ║
║                              │                                       ║
║          ┌───────────────────┼───────────────────┐                  ║
║          │                   │                   │                  ║
║   ┌──────▼──────┐    ┌───────▼──────┐   ┌────────▼──────┐          ║
║   │   PRICE     │    │    TREND     │   │    ALERT      │          ║
║   │   ENGINE    │    │  AGGREGATOR  │   │   SCHEDULER   │          ║
║   │             │    │              │   │               │          ║
║   │ Cron job    │    │ Reads order  │   │ Redis queue   │          ║
║   │ polls item  │    │ volume by    │   │ Fires on      │          ║
║   │ prices      │    │ pin code     │   │ price drop    │          ║
║   │ Diffs vs    │    │ Ranks dish   │   │ Or scheduled  │          ║
║   │ stored hist │    │ velocity     │   │ (Mega Sale)   │          ║
║   └──────┬──────┘    └───────┬──────┘   └───────────────┘          ║
║          │                   │                                       ║
║          └───────────────────▼───────────────────────               ║
║                              │                                       ║
║     ┌────────────────────────┼─────────────────────────┐            ║
║     │                        │                         │            ║
║  ┌──▼──────────┐    ┌────────▼────────┐    ┌──────────▼──────┐     ║
║  │ INSTAMART   │    │   FOOD MCP      │    │  DINEOUT MCP    │     ║
║  │ MCP         │    │                 │    │                 │     ║
║  │             │    │ search_restau.. │    │ search_restau.. │     ║
║  │ search_     │    │ get_restaurant  │    │ Venue trends    │     ║
║  │ products    │    │ _menu           │    │ by area         │     ║
║  │ get_product │    │ Order signals   │    │                 │     ║
║  │ _details    │    │ by pin code     │    │                 │     ║
║  └──────┬──────┘    └────────┬────────┘    └─────────────────┘     ║
║         │                    │                                       ║
║         └────────────────────▼──────────────                        ║
║                              │                                       ║
║                    ┌─────────▼──────────┐                           ║
║                    │    CLAUDE API      │                           ║
║                    │   (Anthropic)      │                           ║
║                    │                    │                           ║
║                    │ • Alert copy       │                           ║
║                    │   ("lowest in 6wk")│                           ║
║                    │ • Trend summaries  │                           ║
║                    │ • Buy timing       │                           ║
║                    │   reasoning        │                           ║
║                    └────────────────────┘                           ║
║                                                                      ║
║   DATA LAYER                                                         ║
║   ┌─────────────────────────┐    ┌──────────────────────────────┐   ║
║   │      PostgreSQL         │    │           Redis              │   ║
║   │                         │    │                              │   ║
║   │  price_history table    │    │  Alert job queue             │   ║
║   │  user_watchlists        │    │  Rate limit counters         │   ║
║   │  item_master            │    │  Session + response cache    │   ║
║   │  trend_snapshots        │    │                              │   ║
║   └─────────────────────────┘    └──────────────────────────────┘   ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## Swiggy MCP — exact integration

```
User connects Swiggy account (OAuth 2.0)
         │
         ▼
Instamart MCP → get past orders → extract item names + variants
         │
         ▼
Instamart MCP → search_products (item name) → get_product_details (price, MRP, discount)
         │
         ▼
Store in PostgreSQL → price_history { item_id, price, timestamp, discount_pct }
         │
         ▼
BullMQ cron job runs every 6 hours → fetches new price → compares → if drop > threshold:
         │
         ▼
Claude API → writes personalised alert message in plain Hindi/English
         │
         ▼
WhatsApp Business API → sends alert with one-tap Instamart deep link
```

```
Food + Dineout MCP → restaurant search by pin code
         │
         ▼
Aggregate order volume signals week-over-week by dish and restaurant
         │
         ▼
Rank by velocity (what's rising fastest, not just what's most popular)
         │
         ▼
Claude API → writes Monday morning trend digest for each neighbourhood cluster
         │
         ▼
WhatsApp + push → delivered every Monday 8 AM
```

---

## Swiggy MCP tools used

| MCP Server | Tool | Purpose in Nazar |
|---|---|---|
| Instamart | `search_products` | Find user's tracked items by name + variant (1kg / 5kg etc.) |
| Instamart | `get_product_details` | Current price, MRP, active discount, availability |
| Food | `search_restaurants` | Restaurant lookup by pin code for trend aggregation |
| Food | `get_restaurant_menu` | Dish-level data — track which dishes are getting ordered |
| Dineout | `search_restaurants` | Fine dining + café venue trends by neighbourhood |

---

## Tech stack

```
Frontend     →  React + TypeScript  ·  Recharts for price graphs  ·  Tailwind CSS
Backend      →  Node.js + Express  ·  BullMQ job queue for price polling
Database     →  PostgreSQL (price history, watchlists)  ·  Redis (alert queue, cache)
AI           →  Claude API — alert copy, trend summaries, buy timing advice
Notifications →  WhatsApp Business API  ·  Web Push API
Auth         →  Swiggy OAuth 2.0 via MCP
Infra        →  Railway (backend)  ·  Vercel (frontend)
```

---

## Roadmap

```
v1 · MVP ──────────────────────────────────────────────────────── Month 1–2
  ✦ Auto-watchlist from Instamart order history
  ✦ Price history graph (30 days)
  ✦ WhatsApp drop alerts
  ✦ Mega Sale Festival monthly reminder

v2 · Intelligence ──────────────────────────────────────────────── Month 3–4
  ✦ 60 / 90 day price history
  ✦ Smart buy timing (AI-powered pattern detection per item)
  ✦ Cross-platform comparison (Instamart vs Blinkit vs Zepto)
  ✦ Hyperlocal trend finder — weekly digest by pin code
  ✦ New restaurant early signal detection

v3 · Scale ─────────────────────────────────────────────────────── Month 5–6
  ✦ Mobile app (React Native)
  ✦ Shareable trend reports ("What Rajkot ordered this week")
  ✦ Food blogger / journalist API tier
  ✦ Price alert sharing ("atta dropped — forward to family group")
  ✦ Household mode — shared watchlist for family
```

---

## Why this matters to Swiggy

Every price drop alert Nazar sends is a **direct conversion event** — user sees the alert, taps, lands on Instamart, buys. The alert is the top of the funnel and the bottom of the funnel in one notification.

Every Monday trend digest creates a **weekly habit loop** — users open Swiggy to order what Nazar told them is trending near them.

Nazar doesn't compete with Swiggy. It makes Swiggy stickier — for the 19.8 million users who are already there, but need a reason to open the app more than once a week.

---

<div align="center">
<br/>

**नज़र रखो। पैसे बचाओ। ट्रेंड से आगे रहो।**

*Keep watching. Save money. Stay ahead of the trend.*

<br/>

*Built for Swiggy Builders Club &nbsp;·&nbsp; Powered by Swiggy MCP APIs &nbsp;·&nbsp; Made in India 🇮🇳*

</div>
