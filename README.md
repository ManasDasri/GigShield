# GigShield — AI-Powered Parametric Income Protection for India's Gig Workers

> Guidewire DEVTrails 2026 | Phase 1 Submission

---
CHECK OUT THE PROTOPE! REPO: <a href = "https://github.com/ManasDasri/GigSheild_demo">GigShield</a>
## Problem Statement

India's platform-based delivery partners (Zomato, Swiggy, Zepto, Blinkit, Amazon etc.) are the backbone of the digital economy. External disruptions — extreme weather, floods, severe pollution, curfews, local strikes — can wipe out 20-30% of a worker's monthly income. These workers have no safety net. When disruptions hit, they bear the full loss alone.

GigShield fixes that.

---

## Our Solution

GigShield is a **B2B2C parametric income protection platform** where delivery platforms subscribe on behalf of their delivery partners. When a verified external disruption triggers predefined thresholds, affected riders are automatically compensated — no manual claims, no delays, no fraud.

GigShield operates as a **capital-light technology platform**, not a balance sheet insurer. Risk exposure is underwritten by a licensed non-life insurance partner (e.g., ICICI Lombard, Bajaj Allianz). GigShield owns the intelligence layer — the AI engine, trigger logic, rider app, company dashboard, and payout UX. Revenue comes from platform fees, not premium float.

---

## Chosen Persona

**Food Delivery Partners — Zomato & Swiggy**

### Why Food Delivery?

- Highest outdoor exposure among all gig segments — fully on 2-wheelers, rain or shine
- Most directly impacted by weather disruptions (heavy rain = zero orders, extreme heat = heat alerts, flood = area lockdowns)
- Largest segment by volume in India (~5M+ active riders)
- Zomato and Swiggy already have structured partner programs, making B2B contracts feasible
- Both companies face ongoing public scrutiny over worker welfare — this contract gives them a verifiable, concrete response to that criticism at minimal contractual cost

### Persona: Rajan, Zomato Delivery Partner

- Age: 26, operates in Bengaluru (Indiranagar zone)
- Earnings: Rs. 600-900/day on a good day; drops to Rs. 0-200 during heavy rain
- Work Pattern: 10-12 hours/day, 6 days/week
- Pain Point: During monsoon, 2-3 days/week are disrupted. He loses Rs. 1,200-2,700/week with no recourse
- Tech Comfort: Uses Zomato app daily; prefers push notifications; UPI-native

### Persona: Divya, Swiggy Delivery Partner

- Age: 31, operates in Chennai
- Earnings: Rs. 500-750/day; Chennai is especially vulnerable to cyclone-season disruptions
- Pain Point: A single cyclone warning shuts her down for 3-4 days (~Rs. 2,500 loss per event)
- Tech Comfort: Basic Android user; Tamil interface preferred

---

## Application Workflow

### Platform Flow Diagram

```
+------------------+      signs contract       +------------------------+
|  Platform Company| ------------------------> |    GigShield Platform  |
|  (Zomato/Swiggy) |                           |                        |
+------------------+                           |   Prediction Engine    |
         |                                     |   (Weather + Income    |
         | weekly post-event invoice           |    historical data)    |
         | (pays only for actual payouts)      +----------+-------------+
         |                                                |
         v                                               v
+--------+----------+                      +-----------+-------------+
|                   |   Pricing Engine     |                         |
|  ACCUMULATION     | -------------------> |   DISTRIBUTION POOL     |
|  POOL  (70%)      |   (zone risk score,  |        (30%)            |
|                   |    weekly rebalance) |                         |
|  Primary reserve  |                      |   Catastrophe reserve   |
|  Routine events   |                      |   Systemic events only  |
+--------+----------+                      +-----------+-------------+
         ^                                             |
         |    unutilised funds return weekly           |
         +---------------------------------------------+
         |
         |    end of 3-month cycle
         v
+--------+----------+
|  Unused funds     |
|  returned to      |
|  Company          |
+-------------------+


   DISRUPTION MONITOR  (polls every 30 minutes)
   +--------------------------------------------------+
   |   OpenWeatherMap   |   CPCB AQI   |  Civic Feed  |
   +--------------------------------------------------+
                          |
               Threshold crossed?
                          |
          +---------------+----------------+
          |                                |
     Routine event                   Systemic event
     (<55% riders affected)          (>55% riders affected
          |                           in city within 4 hrs)
          |                                |
   Draw from                        Draw from
   ACCUMULATION                     BOTH POOLS
   POOL only                        (70% + 30%)
          |                                |
          v                                v
   Severity-weighted              Pro-rated payout
   zone payout                    within city daily cap
   (hardest-hit zones             (hardest-hit zones
    paid first)                    paid first)
          |                                |
          +----------------+--------------+
                           |
                           v
                +----------+----------+
                |   Affected Riders   |
                |   UPI Instant Pay   |
                +---------------------+
```

### What "Systemic Event" Means

A systemic event is triggered when **more than 55% of enrolled active riders in a city report disruption within a 4-hour window**, corroborated by external API data. At this point the catastrophe reserve (30%) unlocks alongside the primary reserve.

This threshold is objective, API-validated, and not overridable by riders or company staff.

---

### Key Flows

#### 1. Company Onboarding and Policy Setup

1. Zomato/Swiggy signs a 3-month contract with GigShield
2. GigShield's Prediction Engine analyses historical weather per zone, average daily rider income, historical disruption frequency
3. A recommended quarterly coverage scope is generated — companies are invoiced weekly based on actual payouts, not upfront worst-case deposits
4. Company configures optional custom thresholds within allowed bands
5. Company nominates zones and rider cohorts to be covered

#### 2. Rider Onboarding and Weekly Premium

1. Riders are bulk-enrolled by the platform via CSV or API
2. Each rider pays a **weekly premium of Rs. 100** (auto-deducted from weekly settlement or UPI autopay)
3. Rider profile created: zone, average earnings tier, platform, disruption history
4. Rider gets access to GigShield dashboard (PWA, mobile-first)

#### 3. Pool Management Logic

```
Week Start
    |
    v
Pricing Engine scores each zone
(7-day weather forecast + historical loss ratio + rider density)
    |
    v
Moves calculated weekly reserve into Distribution Pool (30% bucket)
Primary reserve stays in Accumulation Pool (70% bucket)
    |
    v
Disruption Monitor runs continuously (every 30 min)
    |
    +------ Routine event --------> Draw from Accumulation (70%)
    |
    +------ Systemic event -------> Draw from both pools
    |
    v
Week End: unutilised Distribution funds return to Accumulation
Quarter End: total unutilised Accumulation returned to Company
```

#### 4. Disruption Detection and Payout Trigger

1. AI Disruption Monitor polls every 30 minutes:
   - Weather: OpenWeatherMap (rainfall intensity, wind speed, temperature)
   - Pollution: CPCB AQI (PM2.5, PM10)
   - Civic: admin-curated curfew and strike feed
2. If threshold is crossed for a zone:
   - Disruption event created with timestamp and zone ID
   - All active policyholders in that zone identified
   - Lost income estimated: `avg_hourly_earnings x disruption_hours`
   - City daily cap and per-rider cap enforced before payout
   - Funds transferred automatically to rider UPI/wallet
3. Rider receives notification: "Heavy rain detected in your zone. Rs. 320 credited."

#### 5. Incident Flagging (Fraud-Safe Human Signal)

- Riders can raise a flag on the app for an undetected disruption
- This does NOT release funds directly — it is a data signal only
- AI cross-references the flag against weather and civic data for that zone and timestamp
- Confidence score 0-1 generated:
  - Below 0.75 — flag dismissed, rider notified
  - 0.75 to 0.90 — routed to human reviewer
  - Above 0.90 — auto-payout triggered within 24 hours
- Riders cannot self-initiate payouts — they can only inform the model

#### 6. End-of-Cycle Settlement

- GigShield generates a settlement report for the company
- Unused funds returned via NEFT/RTGS within 5 business days
- Company receives zone analytics: disruption frequency, utilization rate, loss ratio per zone

---

## Risk Architecture and Exposure Caps

This section defines how GigShield bounds its financial exposure at every layer. These are hard system limits enforced at the database and payout engine level, not soft guidelines.

### Layer 1 — Per-Rider Weekly Cap

Every rider has an individual weekly payout ceiling based on their coverage tier. No rider receives more than their cap regardless of disruption severity.

| Tier | Weekly Premium | Weekly Payout Cap |
|------|---------------|-------------------|
| Basic | Rs. 100 | Rs. 1,500 |
| Standard | Rs. 150 | Rs. 2,500 |
| Pro | Rs. 200 | Rs. 4,000 |

### Layer 2 — City Daily Cap

Each city has a hard daily payout ceiling to prevent a single catastrophic event from exhausting the quarterly reserve in one day.

```
Example: Bengaluru daily cap = Rs. 1 crore

Scenario: 10,000 riders each entitled to Rs. 2,000
Total demand  = Rs. 2 crore
City cap      = Rs. 1 crore

Resolution:
  Riders in the most severely affected micro-zones are paid in full first.
  Remaining budget is distributed to outer zones proportionally.
  Outer/less-affected zones absorb the shortfall, not the hardest-hit riders.
```

Severity-weighted distribution is fairer than a flat pro-rata haircut. It anchors payouts to actual impact rather than spreading a reduction arbitrarily across everyone.

### Layer 3 — Quarterly Aggregate Cap Per Contract

Each company-city contract has a total payout ceiling for the 3-month period.

```
Example: Zomato x Bengaluru quarterly cap = Rs. 10 crore

  - Once this ceiling is reached, payouts pause until contract renewal
  - Companies are notified at 70% and 90% utilization of the quarterly cap
  - This is standard insurance practice — every policy has a sum insured limit
```

### Layer 4 — The 70-30 Pool Structure

```
Total Quarterly Allocation
          |
    +-----+------+
    |             |
   70%           30%
    |             |
ACCUMULATION  DISTRIBUTION
   POOL          POOL
(Primary)    (Catastrophe
              Reserve)
    |             |
Routine       Systemic events
events        only (>55% rider
              affectation in
              city / 4 hours)
    |             |
    +------+------+
           |
    Unutilised funds
    roll back to
    Accumulation
    at week-end
```

**Accumulation Pool (70%):** Handles all routine disruptions — bad rain days, local heat waves, isolated curfews. This is the operational reserve and the one that runs continuously.

**Distribution Pool (30%):** Locked for systemic events only. A single bad monsoon day does not unlock this. A city-wide flood that disables the majority of an active rider fleet does.

The split percentages are configurable per contract. A company operating in a high-catastrophe-risk city like Chennai during cyclone season may negotiate a 60-40 split with higher insurer backing.

### Why This Structure Holds Up

The 70-30 architecture solves the core financial design problem: how do you guarantee fast routine payouts without leaving yourself exposed to a catastrophe depleting everything?

By locking 30% behind a high, objective threshold, the platform pays out confidently on routine events. A systemic event trigger cannot be manufactured by riders or companies — it requires external API corroboration. This makes the reserve logic both safe and auditable.

---

## Financial Model

### Company-Side — Post-Event Invoice Model

Rather than requiring companies to pre-fund worst-case scenarios (which creates large working capital liabilities), GigShield invoices companies weekly based on actual payouts made.

- GigShield's licensed insurer partner carries the float
- Companies pay for what happened, not predictions
- No large upfront deposit — no CFO escalation required for initial sign-on
- GigShield charges a platform fee of 8% on total quarterly payout volume
- Insurer partner charges a risk premium on the underwriting layer separately

### Revenue Sources

| Source | Model | Notes |
|--------|-------|-------|
| Rider weekly premium | Rs. 100-200 per rider per week | Retained by GigShield |
| Platform fee | 8% of quarterly payout volume | Charged to company |
| SaaS dashboard fee | Fixed monthly per company | Analytics and admin access |
| Insurer revenue share | Share of underwriting margin | Negotiated per contract |

### Parametric Triggers

| Disruption Type | Threshold | Source | Payout |
|----------------|-----------|--------|--------|
| Heavy Rain | >= 35mm in 3 hours | OpenWeatherMap / IMD | 60% of avg daily earnings |
| Extreme Heat | >= 42 deg C for 4+ hours | OpenWeatherMap | 40% of avg daily earnings |
| Severe Flooding | IMD flood alert for zone | IMD API | 100% of avg daily earnings |
| High Pollution | AQI >= 300 for 4+ hours | CPCB / OpenAQ | 50% of avg daily earnings |
| Curfew or Strike | Confirmed zone closure | Admin override | 100% of avg daily earnings |

Companies can adjust thresholds within defined bands. All threshold changes are logged and auditable.

---

## AI/ML Integration Plan

### 1. Weekly Premium Prediction Engine

- Model: Gradient Boosted Regression (XGBoost)
- Inputs: 5-year IMD weather data per zone, historical disruption frequency, rider density, seasonal patterns
- Output: Recommended quarterly allocation scope per company per city
- Retraining: Quarterly using actual payout data

### 2. Dynamic Risk Pricing Engine

- Model: Zone-level risk scoring
- Logic: Each week, re-scores zones using 7-day forecast, historical loss ratio, current rider count
- Output: How much to hold in Distribution Pool per zone for the coming week
- Effect: Low-disruption zones get lower per-rider allocations — creates natural efficiency

### 3. Fraud Detection

Payouts are fully automated and parameter-driven, so human-initiated fraud is structurally minimal. Detection targets edge cases:

- Repeated flagging from same rider (>3 flags/month with no weather validation) — rider flagged for review
- Location spoofing: rider GPS zone at flag time vs. actual disruption zone
- Duplicate signals from multiple riders in same event — cross-checked
- Model: Isolation Forest for anomaly detection on flag events
- Rule Engine: Deterministic threshold checks run first, before any ML model — fast and fully auditable

### 4. Incident Flag Validator

- Model: Multi-feature binary classifier
- Inputs: Rider zone, flag timestamp, nearest weather station reading, rider's historical flag accuracy, current zone disruption status
- Output: Confidence score 0-1
- Purpose: Catches ground-reality events that official APIs have not yet registered (e.g., a very localised flash flood)

### 5. Systemic Event Classifier

- Monitors the ratio of active riders signalling disruption per city per time window
- When the 55% threshold is crossed and corroborated by external API data, classifies the event as systemic
- Sends an unlock signal to the pool management engine to draw from both the 70% and 30% reserves
- Generates an automated incident report for the company and insurer within 1 hour of trigger

---

## Tech Stack

### Platform Choice: Web (PWA — Progressive Web App)

- Target users are on mid-range Android phones — no app store install needed
- PWA gives mobile-first UX with offline capability
- Companies integrate via API — no separate native app required
- Faster to build and iterate across all competition phases

### Stack

| Layer | Technology | Reason |
|-------|-----------|--------|
| Frontend | React.js (PWA) | Mobile-first, offline-capable, installable |
| Backend API | Node.js + Express | Fast REST APIs, clean async handling |
| Database | PostgreSQL | Pools, policies, payouts require ACID compliance |
| Cache / Queue | Redis | Real-time disruption event queue |
| ML Models | Python (FastAPI microservice) | XGBoost, scikit-learn, Isolation Forest |
| Weather API | OpenWeatherMap (free tier) | Real-time + 7-day forecast |
| Pollution API | CPCB AQI / OpenAQ | India-specific AQI data |
| Payment | Razorpay test mode | UPI instant payouts, India-native |
| Auth | Firebase Auth | OTP login, mobile-friendly |
| Hosting | Railway / Render (free tier) | Zero-ops deployment |
| Version Control | GitHub | Required by competition |

### System Architecture

```
+---------------+  +--------------------+  +---------------+
|  Rider PWA    |  | Company Dashboard  |  |  Admin Panel  |
+-------+-------+  +---------+----------+  +-------+-------+
        |                    |                     |
        +--------------------+---------------------+
                             |
                    +--------+--------+
                    |   API Gateway   |
                    | Node.js/Express |
                    +--------+--------+
                             |
            +----------------+-------------------+
            |                |                   |
   +--------+------+  +------+--------+  +-------+------+
   |  PostgreSQL   |  |  Python ML    |  | Redis Queue  |
   |  (Policies,   |  |  FastAPI      |  | (Disruption  |
   |   Pools,      |  |  (Prediction, |  |  Events)     |
   |   Payouts)    |  |   Fraud,      |  +-------+------+
   +---------------+  |   Flag Valid, |          |
                      |   Systemic    |          |
                      |   Classifier) |  +-------+----------+
                      +---------------+  | Disruption       |
                                         | Monitor          |
                                         | (cron: 30 min)   |
                                         | OpenWeather      |
                                         | CPCB / AQI       |
                                         | Civic feed       |
                                         +------------------+
```

---

## Development Plan

### Phase 1 (Weeks 1-2) — Foundation, Current

- [x] Problem analysis and persona definition
- [x] Financial model design
- [x] Risk architecture (70-30 pool, exposure caps, systemic event definition)
- [x] System architecture design
- [ ] Repo setup and README
- [ ] UI wireframes for rider dashboard
- [ ] Mock data setup for Bengaluru and Chennai zones
- [ ] Initial DB schema (riders, policies, pools, events, payouts)

### Phase 2 (Weeks 3-4) — Build Core

- [ ] Rider registration and onboarding flow
- [ ] Company contract and pool setup UI
- [ ] Weekly premium deduction simulation (Rs. 100 autopay)
- [ ] Pool management engine (Accumulation and Distribution, 70-30 split)
- [ ] City daily cap and per-rider cap enforcement at payout layer
- [ ] Weather API integration (OpenWeatherMap)
- [ ] 3-5 parametric triggers (rain, heat, pollution, flood, curfew)
- [ ] Automated payout flow (Razorpay test mode)
- [ ] Dynamic premium calculation ML model v1

### Phase 3 (Weeks 5-6) — Polish and Scale

- [ ] Advanced fraud detection (Isolation Forest)
- [ ] Incident flag validator with confidence scoring
- [ ] Systemic event classifier and 30% reserve unlock logic
- [ ] Rider dashboard: earnings protected, active coverage, payout history
- [ ] Admin/insurer dashboard: loss ratios, zone analytics, predictive forecast
- [ ] End-of-cycle settlement engine and unused fund return
- [ ] Final demo video and pitch deck

---

## Scope Boundaries — What GigShield Does Not Cover

Per problem statement constraints, strictly excluded:

- Health insurance or medical reimbursements
- Accident coverage or hospitalisation
- Vehicle repair or maintenance
- Life insurance

Covered exclusively: lost income caused by verified external disruptions (weather, environmental, civic).

---

## Competitive Differentiation

| Aspect | Traditional Insurance | GigShield |
|--------|----------------------|-----------|
| Claim Process | Manual, 3-7 days | Zero-touch, automatic |
| Fraud Risk | High (manual claims) | Minimal (parametric, no self-initiation) |
| Premium Structure | Monthly or annual | Weekly Rs. 100, matches pay cycle |
| Payout Speed | Days to weeks | Minutes via UPI |
| Company Cost | Fixed premiums, no refund | Post-event invoice, unused funds returned |
| Catastrophe Exposure | Unbounded | Hard-capped at rider, city, and quarterly level |
| Trust Signal | Low (claim denials common) | High (objective API-triggered payouts) |

---

## Team

> Manas Dasari
> Sai Anushree Kasturi
> Rohan Anand
> Tejas M
> Vishal KVS 

---


---

*Guidewire DEVTrails 2026 — Unicorn Chase*
