
# SQL Query: Session, Event, and Order Analysis

## Description
This query analyzes user sessions, events, orders, and new account registrations in the context of A/B testing.  
It aggregates data by date, country, device, continent, traffic channel, test, and test group.

## Query Structure

The query uses multiple CTEs (Common Table Expressions) to organize the data:
session_info → base session data joined with A/B test info
├─ session_with_orders → count of sessions with orders
├─ events → count of events per session and event type
├─ session → total session count
└─ account → count of new accounts per session

### CTE Details

1. **session_info**
   - Joins sessions, session parameters, and A/B test tables.
   - Key columns: `date`, `ga_session_id`, `country`, `device`, `continent`, `channel`, `test`, `test_group`.

2. **session_with_orders**
   - Counts sessions with at least one order.
   - Grouped by session and test attributes.

3. **events**
   - Counts events (`event_name`) per session.
   - Grouped by session and test attributes.

4. **session**
   - Counts total sessions.
   - Grouped by session and test attributes.

5. **account**
   - Counts new accounts created in sessions.
   - Grouped by session and test attributes.

## Final Output
- Combines all CTEs using `UNION ALL`.
- Columns in the result:
  - `date`, `country`, `device`, `continent`, `channel`, `test`, `test_group`
  - `event_name` – type of metric (`session with orders`, specific event, `session`, `new account`)
  - `value` – corresponding count

## Notes
- Uses `COUNT(DISTINCT ga_session_id)` to count unique sessions.
- Supports multi-segment analysis.
- Useful for dashboards and evaluating A/B test performance.

## Example Result

| date       | country | device | continent | channel | test | test_group | event_name          | value |
|------------|---------|--------|-----------|---------|------|------------|-------------------|-------|
| 2025-10-01 | US      | mobile | NA        | organic | A    | control    | session            | 1234  |
| 2025-10-01 | US      | mobile | NA        | organic | A    | control    | session with orders| 234   |
| 2025-10-01 | US      | mobile | NA        | organic | A    | control    | new account        | 45    |
| 2025-10-01 | US      | mobile | NA        | organic | A    | control    | purchase_click     | 567   |

# 📈 Conversion Funnel A/B Testing Analysis

This repository contains a Python script designed to analyze A/B test results. It calculates the statistical significance (Z-test for proportions) for four key conversion funnel metrics and generates structured conclusions and recommendations for each test.

---

## Key Features

* **Statistical Significance Calculation:** Performs a two-sided Z-test for proportions to compare conversion rates between the control and test groups.
* **Metric Analysis:** Analyzes four primary, session-based metrics.
* **Structured Output:** Generates summarized results including **P-Value**, **Z-Stat**, and clear implementation recommendations.
* **Overall Test Significance:** Significance is calculated across the total observed volume for each specific test.

---

## 📊 Metrics Analyzed

The script analyzes the following conversion metrics, using the total number of **sessions** as the denominator (base):

| Metric Name | Numerator (Event) | Denominator (Base) | Description |
| :--- | :--- | :--- | :--- |
| `add_payment_info/session` | `add_payment_info` | `session` | Conversion to adding payment information per session |
| `add_shipping_info/session` | `add_shipping_info` | `session` | Conversion to adding shipping information per session |
| `begin_checkout/session` | `begin_checkout` | `session` | Conversion to beginning the checkout process per session |
| `new_account/session` | `new account` | `session` | Conversion to new account creation per session |
