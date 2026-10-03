## Data Quality Audit

### Negative Discount Values

- Found: Discount column contained negative values (e.g., -0.1) instead of positive percentages.
- Why a problem: A negative discount mathematically inflates revenue instead of reducing it, producing incorrect profit calculations.
- Effect on analysis: Any revenue/profit measure using Discount would be inaccurate for affected rows.
- Handling: Converted negative values to positive using ABS(), treating it as a data entry sign error.
- Assumption: Negative discount values were unintentional entry errors, not valid business logic.

### Discount Values Exceeding 100% (e.g., 1.20)

- Found: Some Discount values were greater than 1 (representing more than 100% off).
- Why a problem: A discount over 100% is not a valid business scenario.
- Effect: Would make calculated revenue negative or nonsensical for those rows.
- Handling: Treated as misplaced decimal entry errors (e.g., 1.20 → 0.12) and corrected.
- Assumption: Intended value was 10x smaller than recorded.

### Revenue Recorded Blank Despite Payment Status = "Paid"

- Found: Multiple rows had Payment Status = "Paid" but Revenue Recorded was blank.
- Why a problem: Confirmed payment should correspond to recorded revenue; a blank value understates total revenue.
- Effect: Total Revenue was understated before correction.
- Handling: Recalculated missing values using Unit Selling Price × Units Sold × (1 − Discount) for affected rows only.
- Assumption: These were data entry omissions, not cancelled/void transactions.

### Duplicate / Non-Unique Order IDs

- Found: The fact table contains 276 total rows, but only 255 distinct Order IDs — meaning some Order IDs repeat across multiple rows.
- Why a problem: Order ID is expected to be a unique transaction identifier; duplication suggests either genuine duplicate records (double-entry errors) or that Order ID cannot be reliably used as a primary key for counting distinct transactions.
- Effect on analysis: Measures using DISTINCTCOUNT(Order ID) for "Total Orders" may undercount actual rows, while COUNTROWS would overcount if duplicates are true errors rather than legitimate repeat entries (e.g., amended orders).
- Handling: Total Orders was calculated using DISTINCTCOUNT(Order ID) rather than COUNTROWS, to avoid inflating order counts from any duplicate ID entries.
- Assumption: Repeated Order IDs were treated as potential data entry duplicates rather than legitimate multi-line orders, since no line-item or sub-order field exists to justify repetition.

### Residual Revenue Discrepancy

- Found: After cleaning, a gap of approximately KES -468.51M remains between Revenue Recorded and independently calculated revenue (~36% of total).
- Why a problem: Indicates Revenue Recorded may still not be fully reliable for a subset of transactions.
- Effect: Numbers may be understated in some areas; cannot be fully resolved with available evidence.
- Handling: Documented as a known limitation; Revenue Recorded (as corrected) retained as the primary reported figure since it reflects actual recorded transactions.
- Assumption: Remaining gap requires further investigation beyond this assessment's scope.

### Invalid Customer Rating Values

- Found: Customer Rating is expected to fall within a 1–5 scale, but at least one row showed a value of -1.
- Why a problem: A rating outside the valid scale cannot be meaningfully included in average rating calculations without distorting results.
- Effect: Avg Customer Rating measure could be understated if invalid values are included as-is.
- Handling: Documented as invalid; excluded from rating-based analysis rather than guessed/replaced, since there's no reliable way to infer the intended value.
- Assumption: Values outside the 1–5 range are treated as data entry errors, not valid ratings.
