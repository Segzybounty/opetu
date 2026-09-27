# 💶 Smart Spend Analytics: A Behavioral Finance Dashboard

This project is an end-to-end PowerBI project that i downloaded from my bank.An end-to-end Power BI project that transforms raw, unfiltered personal bank statement export into a fully interactive financial intelligence dashboard — surfacing spend concentration, recurring costs, and behavioral spending patterns from real transaction data.
[link to project](/personal_bank.pbix)

## Background

Bank statement exports are messy by design. A raw export typically mixes credit and debit amounts in a single signed column, marks pending transactions with placeholder text instead of a real date, and represents the same merchant a dozen different ways depending on branch location or casing. Anyone trying to actually understand their own spending from this kind of export hits a wall before any analysis can even begin.

This project starts from exactly that unfiltered export — no pre-cleaned sample dataset — and builds the full pipeline needed to turn it into something analyzable: a governed data model, a normalized merchant dimension, and a dashboard that answers real financial questions rather than just displaying raw numbers back.
## Introduction
The dashboard is built around four core questions:
1. What's my overall financial position — spending, income, net cash flow, and current balance?
2. Which merchants make up 80% of my spending (Pareto analysis)?
3. What are my recurring/subscription costs, and what's their true annual cost?
4. How does my spending differ between weekdays and weekends, and across the days of the month?

It's delivered as a 5-page interactive Power BI report (Home, Executive Overview, Merchant Spend & Pareto Analysis, Recurring Expenses/Subscriptions, and Spending Behavior Patterns), with synced slicers (Merchant, Transaction Type, Month) applying report-wide, and a consistent visual theme carried across every page.
## Tools Utilised
- Power BI Desktop — dashboard development, data modeling, and report design
- Power Query (M) — ETL: text cleaning, conditional columns, custom-column keyword matching for merchant normalization
- DAX — KPI measures, time intelligence, ranking, running totals, and behavioral segmentation
- Visual studio code : source control and project share

Data Model Structure:
- Fact table: bank_statement_fact
- Dimension table: Date_dim — a continuous calendar table generated with CALENDAR(), marked as an official Date Table to support reliable time-intelligence DAX (Power BI's auto-generated date hierarchy can't support this on its own)

## The Analysis
Data Cleaning Process

The raw export required several non-trivial cleaning steps before any analysis was possible.

1. Booking Date The Booking date column mixed real dates with the literal text "Reserved" for pending transactions. Fixed by replacing "Reserved" with null (after first capturing it in a Transaction Status flag column), then casting the column to a proper Date type only once the text was removed.

2. Amount → Credit / Spending Split A single Amount column mixed positive (income) and negative (spending) values. Split into two columns using Conditional Columns:

Credit   = if [Amount] > 0 then [Amount] else 0
Spending = if [Amount] < 0 then -[Amount] else 0

3. Merchant / Title Normalization The Title column was the hardest to clean — the same merchant appeared dozens of ways due to branch suffixes and inconsistent casing (e.g. "Prisma Iso Omena", "PRISMA ISO OMENA", "Prisma Olari"). Solved with a two-tier approach:

4. Keyword matching: a custom column checks each title (case-insensitive) against a maintained list of known brand keywords (neste, lidl, prisma, k-market, st1, mobilepay, netflix, etc.) and returns the canonical brand name regardless of branch/location suffix
Word-overlap grouping: titles that don't match a known brand (mostly personal name transfers) were grouped by shared word tokens to catch reordered variants (e.g. "Segun Olufade" vs. "Olufade Segun Sunday")
Reduced 150 unique raw titles down to ~77 canonical merchant groups

5. Row-Order Index Added an Index column (Add Column → Index Column → From 0) to reliably identify the most recent transaction row for the Current Balance measure, since multiple transactions can share the same date — indexing by original row order (the export's newest-first ordering) was more robust than relying on date alone.

*Business Question 1*: What's my overall financial position?

![Excecutive overview image](/images/project3_image1.png)

The Executive Overview shows a Total Spending of €52K against Income of €52K, resulting in a Net Cashflow of -€77.50 — essentially break-even, with spending edging out income by a small margin. Current Balance sits at €81.19.

The top-merchants breakdown shows spending is heavily concentrated at the top: Lumo leads at €15.4K, more than double the next merchant, followed by NALA Payments (€6.9K), Neste (€6.3K), Prestige Finland Oy (€5.1K), and favour (€4.9K) — the remaining merchants (Lidl, St1, Prisma, MobilePay, Ilmarinen) each account for under €2K.

The weekday/weekend donut shows an overwhelming 93.65% of spending happens on weekdays, versus just 6.2% on weekends — a pattern explored further in the Spending Behavior Patterns page. The Net Cashflow by Month chart shows a volatile year: several deep monthly deficits (a low near -€230 in one month) offset by surplus months, with a strong recovery to roughly +€200 by December.

*Business Question 2*: Which merchants make up 80% of my spending?

![merchant making 80% of spending](/images/project3_image2.png)

The Pareto analysis confirms just how concentrated the spending really is: only 7 merchants make up 80% of total spend (out of dozens in the dataset), out of a total spend of €52,454. Ranked by spend, the Top 80% group is: Lumo (€15,357), NALA Payments (€6,881), Neste (€6,319), Prestige Finland Oy (€5,123), favour (€4,926), Lidl (€1,664), and St1 (€1,298). Everything from Prisma (€1,016, rank 8) downward — including MobilePay, Ilmarinen, and Motonet — falls into the "Remaining 20%" segment, each contributing well under €1K individually.

The combo chart makes this visual: the cumulative % line shoots up almost vertically across the first 5-6 merchants, crosses the 80% reference line right around rank 7, then flattens out almost completely — visually confirming that the long tail of merchants after that point barely moves the needle.

Core DAX:

Merchant Rank by Spend = RANKX(ALLSELECTED(bank_statement_fact[Merchant]), [Total Spending], , DESC)

Running Total Spend = 
VAR CurrentRank = [Merchant Rank by Spend]
RETURN
    CALCULATE([Total Spending], FILTER(ALLSELECTED(bank_statement_fact[Merchant]), [Merchant Rank by Spend] <= CurrentRank))

% Cumulative Spend = DIVIDE([Running Total Spend], CALCULATE([Total Spending], ALLSELECTED(bank_statement_fact[Merchant])))

*Business Question 3*: What are my recurring/subscription costs?

![recuring/subscription spend](/images/project3_image3.png)

Merchant Active Months = DISTINCTCOUNT('Date_dim'[Year-Month])
Is Recurring Merchant  = IF([Merchant Active Months] >= 3, "Recurring", "One-off")
Estimated Annual Recurring Cost = 
    IF([Merchant Active Months] >= 3, [Merchant Avg Monthly Spend] * 12, BLANK())

*Business Question 4*: How does my spending behavior differ by day?

![spending behaviour by day](/images/project3_image4.png)

The Avg Daily Spend chart confirms the weekday/weekend split from the Executive Overview at a daily-average level: weekday spend averages roughly €175-180/day, compared to about €110-120/day on weekends.

The Spending Behaviour Pattern matrix (day name × week-of-month) shows Monday is by far the highest-spend day overall (€18,519.03 total) — more than double the next-highest day — with one particular week-4 Monday spiking to €5,873.93 alone. Spend drops off sharply toward the weekend: Saturday (€2,135.10) and Sunday (€1,116.79) are the lowest-spend days of the week by a wide margin.

The Spend by Day of Month trend line shows the month typically opens with the highest single spend spike on day 1 (€3,225.20), dips to the lowest point mid-month around day 10-11 (as low as €618), then climbs again with secondary peaks near day 20-22 (€2,598-€2,650) — a pattern consistent with a post-payday spending spike followed by a mid-month lull.

## What I Learned & Conclusion

Real-world data is never clean. Handling a Reserved/pending-transaction status, a single mixed-sign amount column, and inconsistent merchant naming required designing a repeatable cleaning pipeline rather than one-off fixes.

DAX context transition is subtle. Debugging errors like CALCULATE being used inside a boolean filter expression, or MAX() rejecting a measure argument, taught me to use VAR to freeze a value in its original row context before FILTER shifts into a new one.

A calculated table isn't a measure, and vice versa. A "not a valid table expression" error taught me to be precise about where a piece of DAX logic belongs — Measure vs. Calculated Table vs. Calculated Column.

A real Date dimension matters. Power BI's auto-generated date hierarchy looks convenient but breaks time-intelligence functions; building Date_dim properly, and marking it as an official Date Table, was necessary rather than optional.

Design consistency is part of the analysis, not decoration. Building a shared theme, synced slicers, and a consistent navigation pattern across pages took as much deliberate effort as the DAX itself, and is what separates a working report from a portfolio-ready one.

This project takes a genuinely messy, real-world dataset and builds a complete financial intelligence dashboard from it: a proper star-schema model, a cleaned and normalized merchant dimension, intermediate-to-advanced DAX (Pareto analysis, recurring-cost detection, behavioral segmentation), and a consistently designed multi-page report. Beyond the finished dashboard, the process surfaced and resolved a series of real debugging challenges — DAX context-transition errors, calculated-table vs. measure confusion, and locale-driven data type issues — the kind of practical problem-solving a clean sample dataset never forces you to confront.

Author

Olufade Segun Sunday Data / Business Analyst | Power BI · SQL · Python 📍 Espoo, Finland