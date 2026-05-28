<div align="center">

<img src="https://img.shields.io/badge/Built%20on-Swiggy%20MCP-FC8019?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0id2hpdGUiIGQ9Ik0xMiAyQzYuNDggMiAyIDYuNDggMiAxMnM0LjQ4IDEwIDEwIDEwIDEwLTQuNDggMTAtMTBTMTcuNTIgMiAxMiAyem0tMSAxNXYtNEg3bDUtOXY0aDRsLTUgOXoiLz48L3N2Zz4=&logoColor=white" alt="Built on Swiggy MCP"/>
<img src="https://img.shields.io/badge/Status-Swiggy%20Builders%20Club%20Submission-1D9E75?style=for-the-badge" alt="Status"/>
<img src="https://img.shields.io/badge/Market-India%20🇮🇳-138808?style=for-the-badge" alt="India"/>

<br/><br/>

```
 ███╗   ██╗ █████╗ ███████╗ █████╗ ██████╗ 
 ████╗  ██║██╔══██╗╚══███╔╝██╔══██╗██╔══██╗
 ██╔██╗ ██║███████║  ███╔╝ ███████║██████╔╝
 ██║╚██╗██║██╔══██║ ███╔╝  ██╔══██║██╔══██╗
 ██║ ╚████║██║  ██║███████╗██║  ██║██║  ██║
 ╚═╝  ╚═══╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝╚═╝  ╚═╝
```

### **नज़र** &nbsp;·&nbsp; *The eye that never misses a deal*

**India's first consumer intelligence layer on Swiggy — price history, drop alerts, and hyperlocal food trends. Powered by Swiggy's MCP APIs.**

<br/>

[![Swiggy Food API](https://img.shields.io/badge/Swiggy-Food%20API-FC8019?style=flat-square)](https://mcp.swiggy.com)
[![Swiggy Instamart](https://img.shields.io/badge/Swiggy-Instamart%20API-1D9E75?style=flat-square)](https://mcp.swiggy.com)
[![Swiggy Dineout](https://img.shields.io/badge/Swiggy-Dineout%20API-534AB7?style=flat-square)](https://mcp.swiggy.com)
[![Claude AI](https://img.shields.io/badge/AI-Claude%20API-CC785C?style=flat-square)](https://anthropic.com)
[![Node.js](https://img.shields.io/badge/Backend-Node.js-339933?style=flat-square)](https://nodejs.org)
[![React](https://img.shields.io/badge/Frontend-React-61DAFB?style=flat-square)](https://react.dev)

</div>

---

## 🎯 The Problem

Every Indian household buys the same 10–15 grocery items on Instamart every week — atta, dal, chawal, tel, sugar. These prices change constantly. Swiggy runs a **Mega Savings Festival** (up to 60% off) in the first 3 days of every month. Most people miss it entirely.

> **The current workaround? A notebook.**
> 
> *"Swiggy does not have a price drop alert function — users have to keep track of prices manually, open the app, write down the price in a notebook."* — NewsBytesApp, Jan 2025

On the food side: Swiggy publishes **"How India Swiggy'd"** — a national annual report. Biryani is #1 (for 10 years running). That's interesting. But it tells you nothing about what's trending **in your neighbourhood, this week.**

**Nazar fixes both.**

---

## 👁️ What Nazar Does

### 1 · Instamart Price Watcher

| Feature | What it does |
|---|---|
| **Price History** | Tracks prices of your past-ordered Instamart items over 30 / 60 / 90 days — visualised as a chart |
| **Drop Alerts** | WhatsApp + push notification the moment a tracked item falls below your set price |
| **Smart Buy Timing** | Learns Instamart's Mega Sale pattern (Day 1–3 every month) and reminds you 24hrs before it starts |
| **My Watchlist** | Save your 8–15 regular items (atta, dal, oil, sugar) — track all in one dashboard |
| **Cross-Platform Compare** | Price history for the same item on Instamart vs Blinkit vs Zepto — best deal over time, not just today |
| **One-Tap Order** | Alert fires → user taps → Instamart cart opens with that item pre-added |

### 2 · Hyperlocal Trend Finder

| Feature | What it does |
|---|---|
| **Neighbourhood Trends** | "In your area this week: Pav Bhaji orders ↑34%, new cloud kitchen trending" |
| **Rising Dishes** | Dishes gaining order volume fast in your pin code — before everyone else discovers them |
| **New Restaurant Radar** | Restaurants that opened recently in your area AND are already gaining traction |
| **Weekly Digest** | Every Monday morning: a 5-line WhatsApp summary of what's hot near you this week |
| **AI Trend Summaries** | Claude reads raw order signals and writes human-readable trend cards — not just numbers |

---

## 🗺️ Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                          NAZAR — SYSTEM OVERVIEW                    │
└─────────────────────────────────────────────────────────────────────┘

  USER TOUCHPOINTS
  ┌──────────────┐   ┌──────────────┐   ┌──────────────┐
  │  React Web   │   │  WhatsApp    │   │ Push Notif.  │
  │  Dashboard   │   │  Alerts Bot  │   │  (Mobile)    │
  └──────┬───────┘   └──────┬───────┘   └──────┬───────┘
         │                  │                  │
         └──────────────────▼──────────────────┘
                            │
                    ┌───────▼────────┐
                    │   Node.js API  │
                    │   (Express)    │
                    └───────┬────────┘
                            │
          ┌─────────────────┼──────────────────┐
          │                 │                  │
   ┌──────▼──────┐  ┌───────▼──────┐  ┌────────▼──────┐
   │   PRICE     │  │    TREND     │  │     ALERT     │
   │   ENGINE    │  │  AGGREGATOR  │  │   SCHEDULER   │
   │             │  │              │  │               │
   │ Polls item  │  │ Aggregates   │  │ Redis queue   │
   │ prices on   │  │ order volume │  │ Fires WhatsApp│
   │ schedule    │  │ by pin code  │  │ when price    │
   │ Stores diff │  │ weekly       │  │ threshold hit │
   └──────┬──────┘  └───────┬──────┘  └────────┬──────┘
          │                 │                  │
          └─────────────────▼──────────────────┘
                            │
          ┌─────────────────┼──────────────────┐
          │                 │                  │
   ┌──────▼──────┐  ┌───────▼──────┐  ┌────────▼──────┐
   │  INSTAMART  │  │  FOOD API    │  │  DINEOUT API  │
   │  MCP API    │  │  MCP         │  │  MCP          │
   │             │  │              │  │               │
   │ Product     │  │ Restaurant   │  │ Venue trends  │
   │ price fetch │  │ order volume │  │ by area       │
   │ Item search │  │ Dish signals │  │               │
   └─────────────┘  └──────────────┘  └───────────────┘
                            │
                    ┌───────▼────────┐
                    │  CLAUDE API    │
                    │ (Anthropic)    │
                    │                │
                    │ • Writes smart │
                    │   alert copy   │
                    │ • Trend cards  │
                    │ • Buy timing   │
                    │   advice       │
                    └───────┬────────┘
                            │
          ┌─────────────────┼──────────────────┐
          │                                    │
   ┌──────▼──────┐                    ┌────────▼──────┐
   │ PostgreSQL  │                    │    Redis      │
   │             │                    │               │
   │ Price hist. │                    │ Alert queue   │
   │ User watch- │                    │ Rate limits   │
   │ lists       │                    │ Session cache │
   │ Item master │                    └───────────────┘
   └─────────────┘
```

---

## 🔌 Swiggy MCP Integration

| MCP Server | Tool Used | Powers |
|---|---|---|
| **Swiggy Instamart** | `search_products` | Find user's tracked items by name + variant |
| **Swiggy Instamart** | `get_product_details` | Fetch current price, MRP, discount % |
| **Swiggy Food** | `search_restaurants` | Neighbourhood restaurant discovery |
| **Swiggy Food** | `get_restaurant_menu` | Dish-level data for trend tracking |
| **Swiggy Dineout** | `search_restaurants` | Dineout venue trends by area |

**Auth flow:** OAuth 2.0 via Swiggy MCP — user connects their Swiggy account once, Nazar accesses their order history to auto-populate the watchlist. No manual item entry needed.

---

## 💡 Why This Doesn't Exist Yet

| Existing Tool | What it does | Why it's not Nazar |
|---|---|---|
| **MetricsCart / 42Signals** | Tracks Instamart prices for FMCG brands | B2B only — for Amul, HUL tracking their own SKUs. Costs ₹lakhs/month. Not for consumers. |
| **Savvio / Quick Compare** | Instamart vs Blinkit price today | Point-in-time only. No history. No alerts. No buy timing. |
| **Keepa / Camelizer** | Amazon India price history + alerts | Excellent product — but zero Instamart support. Groceries aren't on Amazon. |
| **Swiggy app itself** | Everything | Zero price tracking, zero watchlist, zero alerts. Confirmed missing feature. |
| **Zomato Trends** | Restaurant supply-demand data | For restaurant owners making menu decisions. Not consumer-facing. National scale only. |
| **Swiggy annual report** | "93M biryanis ordered in India" | National, annual. Not your neighbourhood, not this week. |

**The gap:** No product tracks MY grocery prices over time on Instamart, alerts me when they drop, and shows me what's trending near me this week. Nazar is the first.

---

## 🇮🇳 Why India, Why Now

- **19.8M** monthly transacting Swiggy users (FY 2024–25)
- **100+ cities** with active Instamart service
- **Mega Savings Festival** runs Day 1–3 every month — most users miss it every single month
- **₹X saved per household per month** with smart buy timing — a real, tangible saving that drives daily app opens
- WhatsApp penetration in India means price drop alerts reach users where they already live — no new app install needed for notifications
- Hindi-first naming (नज़र) — built for India, not adapted from a US product

---

## 🗓️ Roadmap

**v1 — MVP (Month 1–2)**
- [ ] Instamart price history for user's top 10 past-ordered items
- [ ] Manual watchlist + price threshold setting
- [ ] WhatsApp alert when price drops
- [ ] Mega Sale Festival reminder (monthly)

**v2 — Intelligence (Month 3–4)**
- [ ] Auto-watchlist from order history (no manual setup)
- [ ] Hyperlocal trend finder — weekly digest by pin code
- [ ] Cross-platform price comparison (Instamart vs Blinkit vs Zepto)
- [ ] Claude-generated buy timing advice ("prices historically lowest mid-month for this item")

**v3 — Platform (Month 5–6)**
- [ ] Shareable trend reports ("What Rajkot ordered this week")
- [ ] Food blogger / journalist API access
- [ ] Price alert sharing — "atta dropped, tell your family group"
- [ ] Mobile app (React Native)

---

## 🏗️ Tech Stack

```
Frontend    →  React + TypeScript + Recharts (price history graphs)
Backend     →  Node.js + Express + BullMQ (job queue for polling)
Database    →  PostgreSQL (price history, watchlists) + Redis (alerts, cache)
AI          →  Claude API via Anthropic (alert copy, trend summaries)
Alerts      →  WhatsApp Business API + Web Push API
Auth        →  Swiggy OAuth 2.0 (MCP)
Hosting     →  Railway / Render (backend) + Vercel (frontend)
```

---


---

<div align="center">

**नज़र रखो। पैसे बचाओ।**  
*Keep watching. Keep saving.*

<br/>

*Built for Swiggy Builders Club · Powered by Swiggy MCP APIs · Made in India 🇮🇳*

</div>
