# Segment Profiles

Derived from K-Means clustering (k = 3, selected via the Elbow method and confirmed with
the highest silhouette score) on `customer_churn.csv` (500 customers). Full derivation
is in `customer_segmentation.ipynb`.

## Summary Table

| Segment | Customers | % of Base | Avg Tenure (mo) | Avg Monthly Charges | Avg Total Charges | Senior Citizen % | Churn Rate |
|---|---|---|---|---|---|---|---|
| Premium Loyalists | 232 | 46.4% | 37.5 | $122.87 | $4,731.25 | 47.0% | 10.0% |
| Budget Regulars | 124 | 24.8% | 37.0 | $107.28 | $4,167.58 | 52.0% | 15.0% |
| Standard Newcomers | 144 | 28.8% | 34.6 | $103.56 | $3,563.60 | 52.0% | 7.0% |

*(Exact figures also available in `segmentation_data.csv`, grouped by `Segment_Name`.)*

---

## 1. Premium Loyalists

**Who they are:** The largest segment (46.4% of the base). Long-tenured, highest
average spend of the three segments, and the highest average total revenue per
customer.

- **Contract mix:** 55% One year, 45% Month-to-month — no Two-year customers in this segment
- **Payment method:** Split evenly between Electronic Check (50%) and Bank Transfer (50%)
- **Paperless billing:** Evenly split (50/50)
- **Churn rate:** 10.0% — roughly matches the overall base rate

**Defining trait:** Highest spenders, but nearly half are still on flexible
month-to-month terms rather than long-term contracts — a mismatch between their value
and their commitment level.

## 2. Budget Regulars

**Who they are:** The smallest segment (24.8%). Comparable tenure to the Premium
Loyalists but noticeably lower average spend, and this segment has the **highest churn
rate** of the three.

- **Contract mix:** 52% Month-to-month, 48% One year — also no Two-year customers
- **Payment method:** 100% Credit Card (a defining feature the clustering picked up on)
- **Paperless billing:** 57% No, 43% Yes
- **Churn rate:** 15.0% — the highest of the three segments, and above the 10.6% base rate

**Defining trait:** Uniformly pays by Credit Card, lower spend, and the segment most at
risk of churning.

## 3. Standard Newcomers

**Who they are:** Mid-sized segment (28.8%), the shortest average tenure of the three,
and the **lowest churn rate**.

- **Contract mix:** 100% Two-year contracts — every customer in this segment is on a
  long-term commitment
- **Payment method:** Mixed — Credit Card (38%), Electronic Check (33%), Bank Transfer (30%)
- **Paperless billing:** 51% Yes, 49% No
- **Churn rate:** 7.0% — the lowest of the three segments

**Defining trait:** Despite having the shortest tenure, the universal Two-year
contract commitment correlates with the best retention of any segment.

---

## Cross-Segment Observations

- **Contract length is the strongest visible churn signal.** The segment locked into
  Two-year contracts (Standard Newcomers) churns least, even with the shortest
  tenure; the two segments with no Two-year customers (Premium Loyalists, Budget
  Regulars) churn more.
- **Payment method concentrates by segment.** Budget Regulars are 100% Credit Card —
  worth validating whether that's a data-collection artifact or a genuine behavioral
  pattern before acting on it.
- **Spend and loyalty don't move together.** The highest spenders (Premium Loyalists)
  are not the most contractually committed group, which is the opportunity called out
  in the business recommendations.
