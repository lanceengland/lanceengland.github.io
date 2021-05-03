---
date: 2021-05-03s
tags: analysis
title:  Aggregate Changes with T-SQL Window Functions
---
# Aggregate Changes with T-SQL Window Functions

Recently I was presented an interesting data challenge. A database would import data feeds at the detail level of one row per patient, per month, per amount type. The output of the database would be a summary that captures the changes. A change would either be a change in amount (in chronological order) or a skipped period (or periods).

For example, in the table below

1. PatientId '1234' no changes for January 2020 - December 2020
2. PatientId '2345' has a new Amount starting in June 2020
3. PatientId '3456' has a gap for June - July 2020

The table below shows the input detail data.

| PatientId | PeriodStartDate | PeriodEndDate | AmountType | Amount |
|-----------|-----------------|---------------|------------|--------|
| 1234      | 1/1/2020        | 1/31/2020     | EW         | 100    |
| 1234      | 2/1/2020        | 2/29/2020     | EW         | 100    |
| 1234      | 3/1/2020        | 3/31/2020     | EW         | 100    |
| 1234      | 4/1/2020        | 4/30/2020     | EW         | 100    |
| 1234      | 5/1/2020        | 5/31/2020     | EW         | 100    |
| 1234      | 6/1/2020        | 6/30/2020     | EW         | 100    |
| 1234      | 7/1/2020        | 7/31/2020     | EW         | 100    |
| 1234      | 8/1/2020        | 8/31/2020     | EW         | 100    |
| 1234      | 9/1/2020        | 9/30/2020     | EW         | 100    |
| 1234      | 10/1/2020       | 10/31/2020    | EW         | 100    |
| 1234      | 11/1/2020       | 11/30/2020    | EW         | 100    |
| 1234      | 12/1/2020       | 12/31/2020    | EW         | 100    |
| 2345      | 1/1/2020        | 1/31/2020     | EW         | 100    |
| 2345      | 2/1/2020        | 2/29/2020     | EW         | 100    |
| 2345      | 3/1/2020        | 3/31/2020     | EW         | 100    |
| 2345      | 4/1/2020        | 4/30/2020     | EW         | 100    |
| 2345      | 5/1/2020        | 5/31/2020     | EW         | 100    |
| 2345      | 6/1/2020        | 6/30/2020     | EW         | 125    |
| 2345      | 7/1/2020        | 7/31/2020     | EW         | 125    |
| 2345      | 8/1/2020        | 8/31/2020     | EW         | 125    |
| 2345      | 9/1/2020        | 9/30/2020     | EW         | 125    |
| 2345      | 10/1/2020       | 10/31/2020    | EW         | 125    |
| 2345      | 11/1/2020       | 11/30/2020    | EW         | 125    |
| 2345      | 12/1/2020       | 12/31/2020    | EW         | 125    |
| 2345      | 1/1/2020        | 12/31/2020    | C5         | 5      |
| 3456      | 1/1/2020        | 1/31/2020     | EW         | 100    |
| 3456      | 2/1/2020        | 2/29/2020     | EW         | 100    |
| 3456      | 3/1/2020        | 3/31/2020     | EW         | 100    |
| 3456      | 4/1/2020        | 4/30/2020     | EW         | 100    |
| 3456      | 5/1/2020        | 5/31/2020     | EW         | 100    |
| 3456      | 8/1/2020        | 8/31/2020     | EW         | 100    |
| 3456      | 9/1/2020        | 9/30/2020     | EW         | 100    |
| 3456      | 10/1/2020       | 10/31/2020    | EW         | 100    |
| 3456      | 11/1/2020       | 11/30/2020    | EW         | 100    |
| 3456      | 12/1/2020       | 12/31/2020    | EW         | 100    |

The table below shows the output summary with changes data.

| PatientId | PeriodStartDate | PeriodEndDate | AmountType | Amount |
|-----------|-----------------|---------------|------------|--------|
| 1234      | 1/1/2020        | 12/31/2020    | EW         | 100    |
| 2345      | 1/1/2020        | 12/31/2020    | C5         | 5      |
| 2345      | 1/1/2020        | 5/31/2020     | EW         | 100    |
| 2345      | 6/1/2020        | 12/31/2020    | EW         | 125    |
| 3456      | 1/1/2020        | 5/31/2020     | EW         | 100    |
| 3456      | 8/1/2020        | 12/31/2020    | EW         | 100    |

The solution involved a few [common table expressions](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql?view=sql-server-ver15) and a few T-SQL [window functions](https://docs.microsoft.com/sql/t-sql/queries/select-over-clause-transact-sql?view=sql-server-ver15). Let's walk through each step/CTE.

## Step 1: Detect Changes

The important bits are on lines 7 and 8. Line 7 uses the [LAG function](https://docs.microsoft.com/sql/t-sql/functions/lag-transact-sql?view=sql-server-ver15) to calculate the amount change between the previous row and current row. The first row would return NULL, so the ISNULL function replaces NULL with $0 (no change).

Line 8 is similar in that it calculates the date difference in months between the previous row and current row. Each row is expected to represent a month, so any value greater than 1 represents a gap.

<pre data-enlighter-language="sql">
SELECT
       PatientId,
       PeriodStartDate,
       PeriodEndDate,
       AmountType,
       Amount,
       ISNULL(Amount - LAG(Amount, 1) OVER (PARTITION BY PatientId, AmountType ORDER BY PeriodStartDate), -1) AS AmtDiff,
       ISNULL(DATEDIFF(month, LAG(PeriodStartDate, 1) OVER (PARTITION BY PatientId, AmountType ORDER BY PeriodStartDate), PeriodStartDate), -1) AS PeriodDiff
FROM
       dbo.PatientSubsidy
WHERE
       PeriodStartDate BETWEEN @PeriodStartDate AND @PeriodStartDate
</pre>

## Step 2: Assign a Group Number

The query from step 1 is referenced in step 2 as 'cte_detectChanges'. The important bit in step 2 is on line 7-9. Line 7 conditionally sums a '1' if any change in amount or period was detected from step 1, or a '0' if no change. This effectively assigns a group number for each sequential block with no change. 

<pre data-enlighter-language="sql">
SELECT
       PatientId,
       PeriodStartDate,
       PeriodEndDate,
       AmountType,
       Amount,
       SUM(
              CASE WHEN AmtDiff <> 0 or PeriodDiff <> 1 THEN 1 ELSE 0 END
       ) OVER (ORDER BY PatientId, AmountType, PeriodStartDate) AS GroupNumber
FROM
       cte_detectChanges
</pre>

The un-aggregated results of step 2 would look like the following table:

| PatientId | PeriodStartDate | PeriodEndDate | AmountType | Amount | GroupNumber |
|-----------|-----------------|---------------|------------|--------|-------------|
| 1234      | 1/1/2020        | 1/31/2020     | EW         | 100    | 1           |
| 1234      | 2/1/2020        | 2/29/2020     | EW         | 100    | 1           |
| 1234      | 3/1/2020        | 3/31/2020     | EW         | 100    | 1           |
| 1234      | 4/1/2020        | 4/30/2020     | EW         | 100    | 1           |
| 1234      | 5/1/2020        | 5/31/2020     | EW         | 100    | 1           |
| 1234      | 6/1/2020        | 6/30/2020     | EW         | 100    | 1           |
| 1234      | 7/1/2020        | 7/31/2020     | EW         | 100    | 1           |
| 1234      | 8/1/2020        | 8/31/2020     | EW         | 100    | 1           |
| 1234      | 9/1/2020        | 9/30/2020     | EW         | 100    | 1           |
| 1234      | 10/1/2020       | 10/31/2020    | EW         | 100    | 1           |
| 1234      | 11/1/2020       | 11/30/2020    | EW         | 100    | 1           |
| 1234      | 12/1/2020       | 12/31/2020    | EW         | 100    | 1           |
| 2345      | 1/1/2020        | 12/31/2020    | C5         | 5      | 2           |
| 2345      | 1/1/2020        | 1/31/2020     | EW         | 100    | 3           |
| 2345      | 2/1/2020        | 2/29/2020     | EW         | 100    | 3           |
| 2345      | 3/1/2020        | 3/31/2020     | EW         | 100    | 3           |
| 2345      | 4/1/2020        | 4/30/2020     | EW         | 100    | 3           |
| 2345      | 5/1/2020        | 5/31/2020     | EW         | 100    | 3           |
| 2345      | 6/1/2020        | 6/30/2020     | EW         | 125    | 4           |
| 2345      | 7/1/2020        | 7/31/2020     | EW         | 125    | 4           |
| 2345      | 8/1/2020        | 8/31/2020     | EW         | 125    | 4           |
| 2345      | 9/1/2020        | 9/30/2020     | EW         | 125    | 4           |
| 2345      | 10/1/2020       | 10/31/2020    | EW         | 125    | 4           |
| 2345      | 11/1/2020       | 11/30/2020    | EW         | 125    | 4           |
| 2345      | 12/1/2020       | 12/31/2020    | EW         | 125    | 4           |
| 3456      | 1/1/2020        | 1/31/2020     | EW         | 100    | 5           |
| 3456      | 2/1/2020        | 2/29/2020     | EW         | 100    | 5           |
| 3456      | 3/1/2020        | 3/31/2020     | EW         | 100    | 5           |
| 3456      | 4/1/2020        | 4/30/2020     | EW         | 100    | 5           |
| 3456      | 5/1/2020        | 5/31/2020     | EW         | 100    | 5           |
| 3456      | 8/1/2020        | 8/31/2020     | EW         | 100    | 6           |
| 3456      | 9/1/2020        | 9/30/2020     | EW         | 100    | 6           |
| 3456      | 10/1/2020       | 10/31/2020    | EW         | 100    | 6           |
| 3456      | 11/1/2020       | 11/30/2020    | EW         | 100    | 6           |
| 3456      | 12/1/2020       | 12/31/2020    | EW         | 100    | 6           |

## Step 3: Aggregate the Data

The query from step 2 is referenced in the final step as 'cte_AssignGroupNumber'. The important bits here are grouping on PatientId, AmountType, Amount, _and_ the new GroupNumber created in step 2. We get the minimum start period and maximum end period for each group.

<pre data-enlighter-language="sql">
SELECT
       PatientId,
       AmountType,
       MIN(PeriodStartDate) AS PeriodStartDate,
       MAX(PeriodEndDate) AS PeriodEndDate,
       Amount
FROM
       cte_AssignGroupNumber
GROUP BY
       PatientId,
       AmountType,
       Amount,
       GroupNumber
ORDER BY
       PatientId,
       AmountType,
       PeriodStartDate
;
</pre>

## Complete Query

The complete query is below. Volume and usage patterns will dictate how to organize the clustered index. It is anticipated that this will be a batch load/extract process with no additional non-clustered indexes, so the initial clustered index will probably be on PeriodStartDate.

<pre data-enlighter-language="sql">
DECLARE
       @PeriodStartDate DATE = '2020-01-01',
       @PeriodStartDate DATE = '2020-12-31'
;

WITH cte_detectChanges AS (
SELECT
       PatientId,
       PeriodStartDate,
       PeriodEndDate,
       AmountType,
       Amount,
       ISNULL(Amount - LAG(Amount, 1) OVER (PARTITION BY PatientId, AmountType ORDER BY PeriodStartDate), -1) AS AmtDiff,
       ISNULL(DATEDIFF(month, LAG(PeriodStartDate, 1) OVER (PARTITION BY PatientId, AmountType ORDER BY PeriodStartDate), PeriodStartDate), -1) AS PeriodDiff
FROM
       dbo.PatientSubsidy
WHERE
       PeriodStartDate BETWEEN @PeriodStartDate AND @PeriodStartDate
),
cte_AssignGroupNumber as (
       SELECT
              PatientId,
              PeriodStartDate,
              PeriodEndDate,
              AmountType,
              Amount,
              SUM(
                     CASE WHEN AmtDiff <> 0 or PeriodDiff <> 1 THEN 1 ELSE 0 END
              ) OVER (ORDER BY PatientId, AmountType, PeriodStartDate) AS GroupNumber
       FROM
              cte_detectChanges
)
SELECT
       PatientId,
       AmountType,
       MIN(PeriodStartDate) AS PeriodStartDate,
       MAX(PeriodEndDate) AS PeriodEndDate,
       Amount
FROM
       cte_AssignGroupNumber
GROUP BY
       PatientId,
       AmountType,
       Amount,
       GroupNumber
ORDER BY
       PatientId,
       AmountType,
       PeriodStartDate
;
</pre>
