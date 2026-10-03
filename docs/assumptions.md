## Assumptions and Business Rules

### Currency

Monetary values without an explicit currency indicator were assumed to be in Kenya Shillings (KES), per assessment instructions. Due to a data cleaning limitation, original currency identifiers (USD, EUR, ZAR) were removed before conversion rates could be applied, and all monetary values are therefore treated as KES in this analysis. This is documented as a known limitation.

### Discounts

Negative discount values were assumed to be data entry sign errors and converted to positive using absolute value. Discount values exceeding 1 (100%) were assumed to be decimal placement errors (e.g., 1.20 interpreted as 0.12) and corrected accordingly.

### Revenue

Where Payment Status was "Paid" but Revenue Recorded was blank, revenue was recalculated using Unit Selling Price × Units Sold × (1 − Discount), on the assumption that these were data entry omissions rather than legitimate zero-revenue transactions.
Revenue Recorded (post-correction) was used as the primary revenue figure throughout the analysis, as it reflects actual recorded transaction values.

### Gross Profit

Gross Profit was defined as Total Revenue − (Unit Cost + Logistic Cost). Delivery Fee was treated as a separate operational cost, not included in Gross Profit, since it may be passed on to or collected from customers rather than absorbed by the business.

### Dates

Order Date was used as the primary date for time-based analysis and trend reporting. Delivery Date was related to the date table via an inactive relationship, used only in delivery-specific measures via USERELATIONSHIP.

### Returns and Cancellations

Returned orders and Cancelled orders were treated as distinct categories and analyzed separately, since a return implies a completed transaction later reversed, while a cancellation implies the transaction did not complete.

### Customer Names

Customer Name values were standardized for formatting (trimmed whitespace, consistent capitalization) during cleaning, but no assumption was made about customer identity beyond what was explicitly recorded.
