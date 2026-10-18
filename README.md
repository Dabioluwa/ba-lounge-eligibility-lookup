# British Airways Lounge Eligibility Lookup — Heathrow T3

A data-calibrated model that estimates **how many passengers will be eligible for each
British Airways lounge tier** at London Heathrow **Terminal 3**, using nothing more than a
flight's **route group** and **time of day**. Built as part of the
**British Airways Data Science Job Simulation on [Forage](https://www.theforage.com/)** (Task 1).

---

## Background

British Airways operates three lounge tiers at Heathrow T3:

| Tier | Lounge | Who is eligible |
|------|--------|-----------------|
| **Tier 1** | Concorde Room *(hypothetical at T3 — none exists there today)* | First Class, premier cardholders, Gold Guest List |
| **Tier 2** | First Lounge | British Airways Gold members |
| **Tier 3** | Club Lounge | Club World / Club Europe (business class) + British Airways Silver |

The task: produce a reusable lookup table of **Tier 1 / Tier 2 / Tier 3 eligibility as a
percentage of boarded passengers**, so lounge demand can be forecast for any schedule
without flight-number-level data.

## The model

Flights are grouped on two dimensions, giving **15 cells (5 route groups × 3 time bands)**:

- **Route group** — Europe & Domestic, North America, Asia & Oceania, Middle East & Africa, South America
- **Time band** — Early morning (05:00–09:59), Midday (10:00–15:59), Evening (16:00–22:59)

Grouping this way captures the two factors that actually drive eligibility: the **premium
and loyalty mix** of a route (long-haul carries First cabins and more top-tier frequent
flyers) and the **business-travel intensity** of the departure time.

Each cell's percentage is **calibrated on real data** from the Forage summer-2025 schedule
(10,000 British Airways departures, Apr–Oct 2025) and expressed as a share of boarded
passengers (seats × 85% load factor).

## Validation

The model was back-tested on a representative day — **18 June 2025** (44 LHR departures,
8,901 passengers):

| Model | Eligible passengers | vs actual |
|-------|--------------------|-----------|
| Baseline (assumption-only) | 2,281 | +46% |
| **Recalibrated lookup** | **1,713** | **+10%** |
| Actual (schedule dataset) | 1,557 | benchmark |

Europe & Domestic — the largest group — landed within **1%** of actual.

## Repository contents

| File | Description |
|------|-------------|
| `British Airways Lounge Eligibility Lookup T3.xlsx` | Main deliverable — 9 sheets: Read Me, Dictionary, Data Preparation, Recommended Lookup, Lookup Table (baseline), Schedule Sample (validated day), Assumptions, Worked Example, Justification |
| `Lounge Eligibility Lookup Template - Task 1.xlsx` | Completed Forage Task 1 template — percentages by group + the four reflection questions |
| `British Airways Lounge Demand Defence.pptx` | One-slide summary defending the result |
| `British Airways Summer Schedule Dataset - Forage Data Science Task 1.xlsx` | Source dataset (Forage) |
| `slide_preview.png` | Exported preview of the slide |

## How to use the lookup

1. Take any future flight and classify it into its **route group** and **time band**.
2. Read the three tier percentages from the `Recommended Lookup` sheet.
3. Multiply by that flight's expected passengers:
   `Tier N passengers = total passengers × Tier N %`.

The table is schedule-agnostic — new routes slot into an existing group, and the
percentages can be refreshed each season by re-running the same calibration.

## Notes & limitations

- The **85% load factor** is an industry assumption (the dataset provides seats, not passengers).
- **South America** has no flights in the source dataset, so its rates are judgement-based,
  re-based on the verified long-haul rates.
- **Tier 1** is modelled as a hypothetical future Concorde-style space — there is no Concorde
  Room at T3 today.
- Long-haul groups are noisier per flight; the model is most accurate at the **day total** and
  **larger category** level.

---

*This project was completed as part of the **British Airways Data Science Job Simulation on
Forage** — a virtual work-experience programme. The dataset and template were provided by
Forage; the model, analysis and write-up are my own.*
