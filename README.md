#  Airbnb New York City 2019 — End-to-End Data Analysis

> **Final project of the Data Analysis track — Voltix EG internship (Task 12).**
> From raw listings to a two-page interactive Power BI dashboard, business insights and recommendations.

---

##  Project Overview

New York City is one of the biggest short-term rental markets in the world, and the 2019 Airbnb listings give a detailed picture of how supply, pricing and demand behave across the five boroughs.

This project takes the dataset through the full analytics workflow: **understand → clean → explore → visualize → build a dashboard → turn findings into decisions**.

**Business questions**
- Which listing types are the most common?
- Which areas are the most active?
- How do prices differ across boroughs and room types?
- Is there a relationship between price and reviews?
- Which neighbourhoods and listings perform best?
- What patterns can a host, a guest or the platform act on?

---

##  Dataset Information

| Item | Details |
|---|---|
| Source | Airbnb NYC listings 2019 (`AB_NYC_2019.csv`), provided with the task as the only allowed dataset |
| Size | **48,895 rows × 16 columns** |
| Time span | Last review dates from 2011-03-28 to 2019-07-08 |
| Granularity | One row per listing |

**Key columns**

| Column | Type | Meaning |
|---|---|---|
| `id`, `host_id` | Integer | Listing / host identifiers |
| `name`, `host_name` | Text | Listing title / host name |
| `neighbourhood_group` | Categorical | Borough (Manhattan, Brooklyn, Queens, Bronx, Staten Island) |
| `neighbourhood` | Categorical | Neighbourhood inside the borough |
| `latitude`, `longitude` | Float | Listing location (used for the map) |
| `room_type` | Categorical | Entire home/apt, Private room, Shared room |
| `price` | Integer | Nightly price in USD |
| `minimum_nights` | Integer | Minimum stay required |
| `number_of_reviews` | Integer | Total reviews (used as a demand proxy) |
| `last_review` | Date | Date of the latest review |
| `reviews_per_month` | Float | Average monthly reviews |
| `calculated_host_listings_count` | Integer | Number of listings owned by the host |
| `availability_365` | Integer | Days available in the next 365 days |

---

## 🧹Data Cleaning & Preparation

| Issue found | Finding | Treatment |
|---|---|---|
| Missing `last_review` / `reviews_per_month` | 10,052 rows (~20.6%) — listings with no reviews yet | Kept the rows; missing `reviews_per_month` set to 0 since no reviews means no monthly activity; `last_review` converted to a proper date type |
| Missing `name` / `host_name` | 16 and 21 rows | Non-analytical text fields, filled with "Unknown" / left out of the analysis |
| Duplicates | 0 duplicated rows | No action needed |
| Data types | `last_review` stored as text | Converted to Date |
| Invalid prices | 11 listings with `price = 0` | Excluded from price analysis |
| Outliers | Prices up to $10,000 (239 listings above $1,000) and minimum stays up to 1,250 nights | Reviewed separately and handled so they don't distort averages; medians used alongside means |
| Unnecessary columns | `host_name`, `name` carry no analytical value for the dashboard | Not used in the visuals |

---

##  Exploratory Analysis — Key Findings

### 1. Supply is concentrated in two boroughs
Manhattan (21,661) and Brooklyn (20,104) hold **~85% of all listings**. Queens follows with 5,666, then the Bronx (1,091) and Staten Island (373).

### 2. Entire homes and private rooms dominate
Entire home/apt makes up **~52%** of listings, private rooms **~46%**, and shared rooms only **~2%**.

### 3. Price depends heavily on location and room type
| Borough | Avg price | Median price |
|---|---|---|
| Manhattan | $196.9 | $150 |
| Brooklyn | $124.4 | $90 |
| Staten Island | $114.8 | $75 |
| Queens | $99.5 | $75 |
| Bronx | $87.5 | $65 |

| Room type | Avg price | Median price |
|---|---|---|
| Entire home/apt | $211.8 | $160 |
| Private room | $89.8 | $70 |
| Shared room | $70.1 | $45 |

The most expensive neighbourhoods (with 50+ listings) are **Tribeca (~$491)**, Battery Park City (~$368), Flatiron District (~$342), NoHo (~$296) and SoHo (~$287).

### 4. The most active neighbourhoods
Williamsburg (3,920), Bedford-Stuyvesant (3,714), Harlem (2,658), Bushwick (2,465) and Upper West Side (1,971).

### 5. Price vs. reviews
The relationship is **weak and slightly negative** (Spearman ≈ −0.06): higher-priced listings tend to get slightly fewer reviews, but price alone doesn't explain demand.

### 6. Demand by borough
Brooklyn collected the most reviews overall (~487K) — more than Manhattan (~455K) despite having fewer listings, which signals strong guest demand relative to supply.

### 7. Availability and host behaviour
- ~36% of listings show **0 available days** in the next year (inactive or fully booked).
- ~34% of listings belong to hosts with more than one property.
- 747 listings require a minimum stay longer than 30 nights.

---

## 📊 Dashboard (Power BI)

File: `Airbnb_Listings.pbix` — a two-page interactive dashboard.

**Page 1 — Overview & Geographic Analysis**
- KPI cards: listings count, price, total reviews, minimum nights
- Column chart: price by borough
- Map: listing locations sized by price

**Page 2 — Room Types & Deep Insights**
- Donut chart: distribution of room types
- Bar chart: price by room type
- Bar chart: price by borough
- Scatter chart: price by neighbourhood


---

##  Business Insights & Recommendations

| # | Insight | What it means | Recommendation |
|---|---|---|---|
| 1 | Manhattan + Brooklyn = ~85% of supply | Highly competitive core, under-served outer boroughs | Platform: recruit hosts in Queens, Bronx and Staten Island, where supply is thin. Hosts: expect stronger competition in the core and differentiate on quality |
| 2 | Entire homes cost ~2.4× private rooms | Room type is the strongest pricing driver alongside location | Hosts with spare capacity can compare converting a private room to an entire-unit offer; guests on a budget should target private rooms |
| 3 | Manhattan's average price is ~2.2× the Bronx's | Location premium is large | Use borough/neighbourhood benchmarks when setting prices instead of one citywide number |
| 4 | Brooklyn has more reviews than Manhattan with fewer listings | Brooklyn demand is strong relative to supply | Good expansion area for new hosts; competitive pricing is still viable |
| 5 | Price barely correlates with reviews | Cheap isn't the only way to win bookings | Hosts should invest in listing quality, photos and responsiveness, not just discounts |
| 6 | ~36% of listings have zero availability | Possible inactive listings inflating market size | Platform: review and clean inactive listings so supply metrics reflect reality |
| 7 | Extreme prices and stays up to 1,250 nights | Outliers distort averages | Always report medians next to means and flag suspicious listings |

---

##  Tools

- **Python** (pandas) — exploration and cleaning
- **Power BI** — interactive dashboard
- **Dataset** — Airbnb NYC 2019 (`AB_NYC_2019.csv`)

##  Repository Structure

```
├── README.md
├── Airbnb_Listings.pbix        # Power BI dashboard
├── AB_NYC_2019.csv             # Raw dataset (provided with the task)
├── notebook/                   # Analysis notebook
├── cleaned_data/               # Cleaned dataset
└── images/                     # Dashboard screenshots
```

##  Conclusion

NYC's Airbnb market is defined by **where** a listing is and **what type** it is. Manhattan and Brooklyn dominate supply, entire homes command a big premium, and price alone doesn't predict popularity. These findings give hosts, guests and the platform clear, data-backed starting points for pricing, expansion and quality decisions.

---

## 🎓 About

Final task of the Data Analysis track in the **Voltix EG** internship.

**Hana** — Artificial Intelligence student | Machine Learning · Data Analysis 
