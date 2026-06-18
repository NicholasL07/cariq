# CarIQ — Project Roadmap

## Overview
**CarIQ** is a free, transparent car research tool that helps buyers and owners make smarter decisions. Users enter a vehicle's year, make, model, trim, and mileage alongside their zip code to receive a fair market price estimate, a reliability deep-dive with sourced data, and a full maintenance roadmap tailored to that exact vehicle.

---

## Problem We're Solving
Most car research sites (CarGurus, Edmunds, KBB) are monetized through dealer leads. Their pricing and ratings are biased toward pushing transactions. CarIQ is the anti-dealer-portal: no ads, no lead gen, just raw data and honest assessments.

---

## Core Features (MVP)

### 1. Vehicle Input Form
- Year (dropdown, 1990–current)
- Make (dropdown, populated from NHTSA API)
- Model (dropdown, filtered by make)
- Trim (text input or dropdown)
- Mileage (number input)
- Zip Code (5-digit, used for regional pricing)

**API:** NHTSA vPIC API (free) — `https://vpic.nhtsa.dot.gov/api/`

---

### 2. Fair Market Pricing by Zip
Show a price range (low / fair / high) for the exact vehicle in the user's region.

**APIs to integrate:**
- **Marketcheck API** — real dealer listing data, filterable by zip radius
- **CarMD API** — vehicle value estimates
- **Backup/fallback:** Scrape or cache median listing prices from Cars.com / AutoTrader weekly

**What we display:**
- Private party value
- Dealer asking price range
- Regional adjustment note (e.g., "Trucks sell 12% higher in Texas")
- Price trend: Is this car appreciating or depreciating?

---

### 3. Reliability Report with Sources
Aggregate reliability data from multiple public and semi-public sources. Be transparent about where every score comes from.

**Sources to pull from:**
- **Consumer Reports** reliability history (link out; paywalled but cite the score)
- **NHTSA Complaints Database** — `https://api.nhtsa.dot.gov/complaints/complaintsByVehicle` (free)
- **NHTSA Recalls** — `https://api.nhtsa.dot.gov/recalls/recallsByVehicle` (free)
- **JD Power IQS ratings** (link out)
- **RepairPal reliability rating** (link out + embed score where available)
- **Owner forum sentiment** — curated notes per model (manual curation or GPT-assisted)

**Display format:**
- Overall reliability grade (A–F) with breakdown by category (engine, transmission, electrical, suspension)
- Source badges linking to each data source
- Open recall count with direct NHTSA link

---

### 4. Common Issues — Engine & Powertrain Transparency
For each make/model/engine, surface the most well-known issues owners face. This is the section that makes CarIQ stand out from review aggregators.

**Data approach:**
- Curated database of known issues per engine code (e.g., BMW N63 oil consumption, GM 3.6L timing chain, Ford 5.4L spark plugs)
- Pull NHTSA complaints and cluster by keyword (oil, timing, transmission, etc.)
- Link to TSBs (Technical Service Bulletins) via NHTSA TSB API

**Display format:**
- "Known Issues for [Engine]" card with severity tags (Minor / Moderate / Major)
- Estimated repair cost per issue (sourced from RepairPal or CarMD)
- Links to NHTSA TSBs and owner forum threads (curated)

---

### 5. Mileage-Based Maintenance Schedule
Show a checkpoint-by-checkpoint service list for the exact vehicle.

**Data approach:**
- Base schedule from OEM service manuals (curated per make/model)
- Pull from CarMD maintenance API where available
- Supplement with RepairPal's scheduled maintenance tool

**Checkpoints to cover (examples):**
- 15k / 30k / 45k / 60k / 75k / 90k / 100k / 120k / 150k miles
- Each checkpoint lists: what to inspect, what to replace, estimated cost

**Display format:**
- Timeline/stepper UI — current mileage highlighted
- Color-coded: overdue (red), due soon (yellow), upcoming (green)
- Estimated cost per service

---

### 6. Oil Change Guidance
Go beyond the OEM recommendation. Show:
- OEM recommended interval (e.g., BMW says 10k miles)
- Owner/enthusiast recommended interval (e.g., BMW M owners do 5k)
- Recommended oil type and viscosity (e.g., 0W-40 full synthetic)
- Recommended oil brands for that engine (e.g., Motul 8100, Liqui-Moly)
- Filter recommendation
- Note on why shorter intervals help (sludge, LSPI prevention, turbo oil coking)

**Data approach:** Curated per engine family. Community-sourced, manually verified.

---

## Unique Selling Propositions (USPs) — Beat the Competition

### 1. "Pre-Buy Checklist" Generator
After entering a vehicle, generate a printable PDF checklist of exactly what to inspect before purchasing that car. Tailored by known weak points of that model. No other major site does this at a per-model level.

### 2. "True Cost to Own" vs "Cost to Fix Once"
Most sites show generic TCO. CarIQ shows: "If this car has the known timing chain issue, fixing it costs $X. After that fix, it's one of the most reliable engines you can buy." Honest risk-adjusted ownership cost.

### 3. Recall & TSB Alert Banner
Prominent, real-time NHTSA recall check at the top of every result. If the VIN is entered, check against NHTSA's VIN recall lookup. Other sites bury this.

### 4. "Owner Reality" Section
Pull and curate actual owner quotes from Reddit (r/mechanics, r/whatcarshouldibuy, model-specific subs) and enthusiast forums. Attributed, linked, and labeled as community opinion — not editorial. Gives raw, unfiltered perspective that review sites sanitize.

### 5. Mileage Risk Score
A single 0–100 score that accounts for: mileage relative to known failure points for that model, age of the car, service history gaps (if VIN entered), recall status, and regional climate. No other tool combines these into one honest number.

### 6. "Best Year" Comparison Toggle
Show which model year to target for best reliability (e.g., "Avoid 2011–2013 Ford F-150 EcoBoost; 2015+ has revised turbo system"). Helps buyers know which year is the sweet spot.

### 7. Mechanic Cost Estimator by Zip
After showing what repairs are common, let users enter their zip and get a realistic labor rate range for their area. Integrates RepairPal or labor rate data from CarMD.

### 8. Mobile-First, No Login, No Ads
Zero friction. No account required, no dealer referrals, no sponsored listings. Designed to be shared ("send this to a friend who's buying a used car").

---

## API Summary

| API | Cost | Usage |
|-----|------|-------|
| NHTSA vPIC | Free | Make/model/trim lookup |
| NHTSA Complaints | Free | Complaint data by vehicle |
| NHTSA Recalls | Free | Open recall data |
| NHTSA TSBs | Free | Technical service bulletins |
| Marketcheck | Paid | Listing prices by zip |
| CarMD | Paid | Maintenance schedules, vehicle value |
| RepairPal (unofficial) | Free (scrape) | Repair cost estimates, reliability ratings |

---

## Tech Stack

- **Frontend:** HTML5, CSS3 (custom variables + grid), Vanilla JS (ES6+)
- **No backend required for MVP** — all API calls made client-side
- **APIs with CORS issues:** Proxy via Cloudflare Workers (free tier) or a simple serverless function
- **Hosting:** GitHub Pages, Netlify, or Vercel (all free)

---

## Development Phases

### Phase 1 — MVP (Current)
- [x] Vehicle input form with NHTSA-powered dropdowns
- [x] Pricing display (mock data structure, real API slots ready)
- [x] Reliability section with NHTSA complaints + recalls (live API)
- [x] Common issues database (curated, expandable)
- [x] Maintenance schedule by mileage (curated)
- [x] Oil change guidance (curated)

### Phase 2 — Data Depth
- [ ] Wire Marketcheck API for real zip-based pricing
- [ ] Wire CarMD API for maintenance data
- [ ] Expand common issues database to 200+ engine families
- [ ] Add "Best Year" comparison for top 50 most searched vehicles

### Phase 3 — Features
- [ ] VIN lookup (decode + NHTSA recall check)
- [ ] Pre-buy checklist PDF export
- [ ] Mileage Risk Score algorithm
- [ ] Owner Reality section (curated Reddit/forum quotes)
- [ ] Mechanic cost estimator by zip

### Phase 4 — Growth
- [ ] User-submitted service records (crowdsource maintenance history)
- [ ] Email alerts for new recalls on saved vehicles
- [ ] Mobile app (React Native or PWA)

---

## File Structure
```
cariq/
├── index.html          # Single-file app (MVP)
├── project.md          # This file
└── assets/             # Phase 2+
    ├── data/
    │   ├── issues.json         # Common issues database
    │   ├── maintenance.json    # Maintenance schedules
    │   └── oil.json            # Oil change guidance
    └── icons/
```

---

## Design Principles
1. **Transparency first** — every data point cites its source
2. **No dark patterns** — no upsells, no dealer referrals, no paywalls
3. **Owner-side bias** — written for buyers and owners, not dealers
4. **Fast** — sub-2s load, works on mobile data
5. **Honest uncertainty** — if data is incomplete, say so

---

*Last updated: 2026-06-18*
