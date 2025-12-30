# Loan Default Dashboard


### Live Dashboard Link
[View Power BI Dashboard](https://app.powerbi.com/view?r=eyJrIjoiZjlhYTRlYjgtNTBkZi00NzM3LWI1MWUtMWFkOWQxMTI1YzUwIiwidCI6IjEzOGE5ZGI3LWVmNjItNDZkOC1iMTRiLTI4MWM1NWM1N2QxNCJ9)

### Dashboard Overview
This Power BI dashboard analyzes loan default behavior to help identify high-risk and low-risk borrowers using demographic, income, employment, and credit metrics.

### Dashboard Pages
- **Loan Default & Overview** – Overall defaults, loan purpose, income and employment insights  
- **Applicant Demographics** – Age group and applicant distribution analysis  
- **Financial Risk Metrics** – Year-wise default trends and risk indicators

## Problem Statement

This dashboard helps banks and financial institutions understand loan default behavior among borrowers. It enables decision-makers to identify high-risk customers, analyze default patterns, and understand which factors contribute most to loan defaults.

By analyzing borrower demographics, financial details, credit behavior, and repayment status, this dashboard supports:

* Better loan approval decisions
* Reduced default risk
* Improved profitability
* Data-driven credit policy formulation

The dashboard highlights key risk indicators such as credit score, income, employment type, age group, debt-to-income ratio, and loan purpose.

---
## Steps Followed

# First Report Page: Loan Default & Overview

### Step 1: Downloading, Installing & Configuring Standard Mode Gateway

**Purpose**
To connect Power BI Service with a local SQL Server (on-premises data source).

**Steps:**

  1.  Download the On-premises Data Gateway (Standard mode) from Microsoft.
  
  2. Install the gateway on the machine where SQL Server is available.

  3. During setup, select **Register a new gateway**.
  
  4. Enter a Gateway Name (example: DF1).
  
  5. Set and confirm the Recovery Key.
  
  6. Complete the configuration.

✅ **Status:** SUCCESS

---

### Step 2: Downloading & Installing Microsoft SQL Server

1. Download Microsoft SQL Server.
2. Install Database Engine Services.
3. Install SQL Server Management Studio (SSMS).
4. Verify SQL Server services are running successfully.

---

### Step 3: Importing Data into SQL Server

1. Open SSMS and connect to SQL Server.
2. Open database **loan**.
3. Right-click database → Tasks → Import Flat File.
4. Select Loan_Default dataset file.
5. Review column names and data types.
6. Import data into table **loan_default**.

---

### Step 4: Creating a Dataflow Using Power BI Service

1. Sign in to Power BI Service.
2. Open or create a workspace named **Loan_Default**.
3. Click **New Item** → **Dataflow Gen1**.
4. Add new tables → SQL Server.
5. Enter SQL Server and database details.
6. Connect using On-premises Data Gateway.
7. Select **loan_default** table.
8. Save and refresh the Dataflow.


---

### Step 5: Importing Data into Power BI Desktop from Dataflow

* Connected Power BI Desktop to Power BI Service
* Selected **Dataflow Gen1**
* Loaded cleaned data from SQL Server via gateway
* Used Windows Authentication for secure access
* Centralized data preparation
* Avoid duplicate data cleaning
* Consistency across multiple reports
* Time-saving for teams

---

### Step 6: Column Definitions & Dataset Description

| Column Name    | Description                     |
| -------------- | ------------------------------- |
| LoanID         | Unique identifier for each loan |
| Age            | Borrower's age                  |
| Income         | Annual income                   |
| LoanAmount     | Approved loan amount            |
| CreditScore    | Creditworthiness score          |
| MonthsEmployed | Employment duration             |
| NumCreditLines | Active credit lines             |
| InterestRate   | Loan interest rate              |
| LoanTerm       | Loan duration in months         |
| DTIRatio       | Debt-to-Income ratio            |
| Education      | Education level                 |
| EmploymentType | Employment category             |
| MaritalStatus  | Marital status                  |
| HasMortgage    | Existing mortgage indicator     |
| HasDependents  | Dependents indicator            |
| LoanPurpose    | Reason for loan                 |
| HasCoSigner    | Co-signer availability          |
| Default        | Loan default status             |
| Loan Date      | Loan issue date                 |

---

### Step 7: Data Types & Profiling (Power Query Editor)

* Enabled Column Quality, Distribution & Profile
* Profiling based on entire dataset
* Total records: **2,55,347**
* No nulls, errors, or invalid values
* LoanID verified as unique primary key
* All data types correctly detected

---

### Step 8: Report Design Setup

* Renamed page to **Loan Default – Overview**
* Added header, shapes, and layout design
* Applied consistent theme and formatting

---

### Step 9: Loan Amount by Purpose (Line Chart)


In this step, we focus on creating KPIs using **DAX measures** and **calculated columns**, and representing them using a **line chart** to analyze loan data effectively.
We start by changing the color and formatting of the report 

---

## Calculated Column & DAX Measure – Year and Loan Amount by Purpose

To enable **year-wise analysis** and calculate **loan amount by purpose**, we create:

###  Calculated Column – Year

```DAX
Year = YEAR('Loan_default'[Loan_Date_DD_MM_YYYY])

```
![Year](https://github.com/vinaysaivutukuri/Loan_Default/blob/main/images/Year.png?raw=true)



### DAX Measure – Loan Amount by Purpose
```DAX
Loan Amount by Purpose =
SUMX(
    FILTER('Loan_default', NOT(ISBLANK('Loan_default'[LoanAmount]))),
    'Loan_default'[LoanAmount]
)

```

Purpose of this measure:

* Identify how loan distribution varies by purpose

* Spot high-risk loan purposes


Finally in step-9 we validate all calculations by checking values in Power BI Table view and cross-verifying with Excel data.
This ensures all numbers are accurate and reliable for analysis.

---

### Step 10: Average Income by Employment Type
I created a DAX measure to calculate the average income for each employment type and visualized it using a line chart in Power BI.

**DAX Measure:**

```DAX
Average Income by Employment Type =
CALCULATE(
    AVERAGE('Loan_default'[Income]),
    ALLEXCEPT('Loan_default', Loan_default[EmploymentType])
)
```

**Purpose:**

* Compares income levels across employment types
* Ignores unrelated filters


**Implementation:**

* Created and executed the measure in Power BI to calculate average income by employment type.
* Added it to a line chart, dragging Employment Type to the axis and the measure to values, with proper formatting to show trends.
* Verified results using Table view and cross-checked with Excel data to ensure accuracy.
---


### Step 11: Default Rate by Employment Type

I created a DAX measure to calculate the default rate (%) for each employment type and visualized it using a line chart in Power BI.

**DAX Measure:**

```DAX
Default Rate by EmploymentType =
VAR totalrecords = COUNTROWS(ALL('Loan_default'))
VAR DefaultCases = COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE()))
RETURN
CALCULATE(DIVIDE(DefaultCases,totalrecords),ALLEXCEPT('Loan_default','Loan_default'[EmploymentType]))*100
```

**Purpose:**

* Identifies risky employment categories

* Shows the percentage of loans that became default for each Employment Type

* Ignores unrelated filters, allowing only Employment Type to filter the results

**Implementation:**

* Added the measure to a line chart, dragging Employment Type to the axis and the measure to values, and applied formatting to show trends.

* Verified results using Table view and cross-checked with Excel data to ensure accuracy.

---

### Step 12: Average Loan Amount by Age Group

I created Age Groups using a calculated column and calculated the average loan amount for each group, then visualized it using a line chart in Power BI.

### Calculated Column: Age Groups

```
Age Groups =
IF('Loan_default'[Age] <= 19, "Teen",
    IF('Loan_default'[Age] <= 39, "Adults",
        IF('Loan_default'[Age] <= 59, "Middle Age Adults",
            "Senior Citizens"
        )
    )
)

```
![Age Groups](https://github.com/vinaysaivutukuri/Loan_Default/blob/main/images/Age%20Groups.png?raw=true)


### DAX Measure: Average Loan by Age Group

```
Average Loan by Age Group =
AVERAGEX(
    VALUES('Loan_default'[Age Groups]),
    AVERAGE('Loan_default'[LoanAmount])
)

```

**Purpose:**

* Analyzes average loan amounts for different age categories

* Helps identify trends in borrowing behavior across age groups

* Provides a clear comparison between Teen, Adult, Middle Age Adults, and Senior Citizens

**Implementation:**

* Added the measure to a line chart, with Age Groups on the axis and the measure on values

* Applied formatting like colors, smooth lines, and proper chart title

* Performed data validation by comparing results with a Table visual in Power BI and cross-checking with Excel data to ensure accuracy

---

### Step 13: Default Rate by Year

I created a DAX measure to calculate the default rate (%) for each year and visualized it using a line chart in Power BI.

```
Default Rate by Year =
VAR totalloans =
    CALCULATE(
        COUNTROWS('Loan_default'),
        ALLEXCEPT('Loan_default', 'Loan_default'[Year])
    )

VAR default =
    CALCULATE(
        COUNTROWS(
            FILTER('Loan_default', 'Loan_default'[Default] = TRUE())
        ),
        ALLEXCEPT('Loan_default', 'Loan_default'[Year])
    )

RETURN
    DIVIDE(default, totalloans) * 100
```

**Purpose:**

* Shows the trend of loan defaults over different years

* Ensures only the Year filter impacts the calculation

* Helps identify periods with higher default rates for analysis

**Implementation:**

* Added the measure to a line chart, with Year on the axis and the measure on values

* Applied formatting for clear visualization (colors, smooth lines, proper title)

* Performed data validation by checking results in Power BI Table visual and cross-checked with Excel Pivot Tables to ensure accuracy

---

# Second Report Page: Applicant Demographics & Financial Profile

### Step 14: Median Loan Amount by Credit Score Category



On this page, I calculated the **Median Loan Amount** and grouped applicants into **Credit Score Bins**, then visualized the data using a **line chart** in Power BI.  


### Calculated Column - Credit Score Bins
```
Credit Score Bins = 
IF( Loan_default[CreditScore] <= 400, "Very Low",
    IF( Loan_default[CreditScore] <= 450, "Low",
        IF( Loan_default[CreditScore] <= 650, "Medium",
            "High"
        )
    )
)
```
![Credit Score](https://github.com/vinaysaivutukuri/Loan_Default/blob/main/images/credit%20score.png?raw=true)


### Measure: Median Loan Amount by Credit Score Bins
```
Median by Credit Score bins = 
MEDIANX('Loan_default', 'Loan_default'[LoanAmount])
```

**Purpose:**

* Calculates the median loan amount for each credit score category

* Groups applicants into Very Low, Low, Medium, and High credit score bins

* Helps identify trends in loan amounts across different credit risk segments

**Implementation:**

* Added the measure to a line chart, with Credit Score Bins on the axis and Median Loan Amount on values

* Applied formatting: colors, line styles, markers, titles, and smoothing for clear visualization

* Performed data validation by manually verifying median values in Power Query Editor and cross-checking with Excel data to ensure accuracy

---

### Step 15: Average Loan Amount (High Credit Score)

In this step, I analyzed the Average Loan Amount for customers with High Credit Score and visualized the results using a Donut Chart in Power BI.
```
Average Loan Amt (High Credit) = 
AVERAGEX(
    FILTER(
        'Loan_default',
        'Loan_default'[Credit Score Bins] = "High"
    ),
    'Loan_default'[LoanAmount]
)

```
**Purpose:**

* Calculates the average loan amount only for High Credit Score customers

* Helps understand loan distribution based on Age Group and Marital Status

* Focuses analysis on financially reliable (high credit) applicants

**Donut Chart:**

* Values: Average Loan Amt (High Credit)

* Details: Marital Status

* Legend: Age Groups

This visualization shows how average loan amounts vary across different age groups and marital statuses, specifically for high credit score customers.

**Implementation:**

* Applied the measure to a Donut Chart and configured Age Groups and Marital Status for detailed analysis

* Formatted the visual with colors, borders, titles, and data labels for clarity

* Performed data validation by verifying values using Power BI Table Visual and Excel Pivot Table, ensuring all results matched accurately

---

### Step 16: Total Loan (Adults) by Credit Score

```
I created a DAX measure to calculate total loan amount for Adult customers and visualized it using a Line Chart.

Total Loan (Credit Bins) =
CALCULATE(
    SUM('Loan_default'[LoanAmount]),
    'Loan_default'[Age Groups] = "Adults",
    ALLEXCEPT(
        'Loan_default',
        'Loan_default'[Age],
        'Loan_default'[Age Groups],
        'Loan_default'[CreditScore],
        'Loan_default'[Credit Score Bins]
    )
)

```
**Purpose:**

* Shows total loan distribution for Adults across Credit Score Bins

* Filters only on Age, Age Groups, Credit Score, and Credit Score Bins

**Implementation:**

* Added the measure to a Line Chart with proper formatting (title, line color, markers)

* Validated data using Power BI Table Visual and Excel Pivot Table, ensuring totals matched the chart

**Outcome:**

* Enabled accurate analysis of Adult loans by credit score

* Ensured reliable insights using DAX and proper validation
---

### Step 17: Total Loan (Middle Age Adults) by Mortgage & Dependents – Clustered Column Chart

I created a DAX measure to calculate the total loan amount for Middle Age Adults and visualized it using a Clustered Column Chart, broken down by Mortgage and Dependents.
```
Total Loan (Middle Age Adults) =
SUMX(
    FILTER(
        'Loan_default',
        'Loan_default'[Age Groups] = "Middle Age Adults"
    ),
    'Loan_default'[LoanAmount]
)

```
**Purpose:**

* Calculates total loan amount specifically for Middle Age Adults

* Allows analysis based on Mortgage (Yes/No) and Dependents (Yes/No)

* Ensures the calculation is strictly limited to the target age group

**Clustered Column Chart Configuration:**

* Values: Total Loan (Middle Age Adults)

* X-Axis: Has Mortgage (Yes/No)

* Legend: Has Dependents (Yes/No)

**Implementation:**

Added the measure to a Clustered Column Chart and applied formatting (colors, column spacing, title)

#### Performed data validation:

* Power BI Table Visual: Checked totals by Age Group, Mortgage, and Dependents; matched chart values

* Excel Pivot Table: Created Age Group identifier, applied filters, calculated totals; confirmed exact match with Power BI

**Outcome:**

* Enabled clear analysis of total loan amounts for middle age adults based on mortgage and dependent status

* Ensured accurate, validated insights using proper DAX calculations and cross-checks



---


### Step 18: Loans by Education Type

In this step, I created a **DAX measure** to count total loans by **Education Type**, excluding blank LoanIDs, and visualized it using a **Line Chart**.

```DAX
Loans by Education type =
COUNTROWS(
    FILTER(
        'Loan_default',
        NOT(ISBLANK('Loan_default'[LoanID]))
    )
)
```
**Purpose:**

* Counts total loans for each Education Type (Bachelor, Master, PhD, etc.)

* Excludes blank LoanID values to ensure accurate counts

* Works dynamically with visuals like line charts and tables

**Implementation:**

* Added the measure to a Line Chart, using Education Type on the X-axis and the measure on values

* Validated results using Power BI Table Visual and Excel, confirming counts matched

**Outcome:**

* Provides reliable, education-wise loan distribution

* Ensures accurate and clean data for analysis and reporting
---

# Third Report Page: Financial Risk Metrics

### Step 19: Year-on-Year (YoY) Loan Amount – Line Chart

I created a DAX measure to calculate the Year-on-Year percentage change in total loan amount and visualized it using a Line Chart.
```
YOY Loan Amount Change =
DIVIDE(
    /* Numerator: Current Year - Previous Year */
    CALCULATE(
        SUM('Loan_default'[LoanAmount]),
        'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY]))
    )
    -
    CALCULATE(
        SUM('Loan_default'[LoanAmount]),
        'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY])) - 1
    ),

    /* Denominator: Previous Year */
    CALCULATE(
        SUM('Loan_default'[LoanAmount]),
        'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY])) - 1
    ),

    0
) * 100
```

**Purpose:**

* Calculates the YoY change (%) in total loan amount

* Compares current year vs previous year dynamically

* Safely handles zero or missing previous-year values

**Implementation:**

* Added the measure to a Line Chart to show yearly loan trends

* Applied formatting for clarity (line color, markers, title)

* Validated results using Power BI Table Visual and Excel Pivot Table, confirming calculations matched

**Outcome:**

* Provides insights into loan growth or decline over time

* Useful for financial risk analysis and executive dashboards

* Supports trend analysis and KPI comparisons

---
### Step 20: Year-on-Year (YoY) Default Loans – Line Chart

Created a DAX measure to calculate Year-on-Year % change in defaulted loans and visualized it using a Line Chart.
```
YOY Default Loans Change =
DIVIDE(
    CALCULATE(COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE())),
        'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY]))
    )
    -
    CALCULATE(COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE())),
        'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY])) - 1
    ),
    CALCULATE(COUNTROWS(FILTER('Loan_default','Loan_default'[Default]=TRUE())),
        'Loan_default'[Year] = YEAR(MAX('Loan_default'[Loan_Date_DD_MM_YYYY])) - 1
    ),
    0
) * 100

```

**Purpose:**

* Shows YoY % change in defaulted loans

* Counts only Default = TRUE loans

* Handles missing/zero previous-year values safely

**Implementation:**

* Added to a Line Chart with proper formatting

* Validated using Power BI Table Visual and Excel Pivot Table

**Outcome:**

* Tracks default trends year-on-year for risk analysis and dashboards

### Step 21: Year Till Date (YTD) Loan Amount by Credit Score & Marital Status

Created a YTD Loan Amount DAX measure using DATESYTD and visualized it with a Ribbon Chart to analyze year-to-date loan trends by Credit Score Bins and Marital Status.
```

YTD Loan Amount =
CALCULATE(
    SUM('Loan_default'[LoanAmount]),
    DATESYTD('Loan_default'[Loan_Date_DD_MM_YYYY].[Date]),
    ALLEXCEPT(
        'Loan_default',
        'Loan_default'[Credit Score Bins],
        'Loan_default'[MaritalStatus]
    )
)

```

**Purpose:**

* Calculates total loan amount from Jan 1 to latest date in the year

* Compares YTD loan distribution across credit score categories

* Shows ranking changes by marital status using Ribbon Chart

**Implementation:**

* Used DATESYTD for time-based calculation

* Controlled slicers using ALLEXCEPT

* Built and formatted a Ribbon Chart for trend comparison

**Validation:**

* Cross-checked values using Power BI Table Visual

* Verified totals with Excel Pivot Table

**Outcome:**

* Provides clear YTD loan trends for financial and risk analysis

* Ensures accurate, consistent, and business-ready insights

---

### Step 22: Income Bracket (Decomposition Tree)


Created an Income Bracket calculated column using SWITCH(TRUE()) and used it in a Decomposition Tree to analyze loan amounts by income category.

### Calculated Column:  Income Bracket 

```

Income Bracket =
SWITCH(
    TRUE(),
    'Loan_default'[Income] < 30000, "Low Income",
    'Loan_default'[Income] >= 30000 && 'Loan_default'[Income] < 60000, "Medium Income",
    'Loan_default'[Income] >= 60000, "High Income"
)

```
![Income Bracket](https://github.com/vinaysaivutukuri/Loan_Default/blob/main/images/income%20bracket.png?raw=true)


**Purpose:**

* Converts raw income values into meaningful categories

* Groups customers into Low, Medium, and High Income

* Makes income-based analysis simple and readable


**Income Categories:**

* Income < 30,000 → Low Income

- 30,000 – 59,999 → Medium Income

- ≥ 60,000 → High Income

**Implementation:**

* Created a calculated column (row-level logic)

* Used in Decomposition Tree, filters, and slicers


* Helps break down loan amount by income level


**Outcome:**

* Clear income-based insights

* Better visualization and drill-down in Decomposition Tree

---

# Step 23 : The report was then published to Power BI Service.

![Publishing to Power BI](https://github.com/vinaysaivutukuri/Loan_Default/blob/main/images/Publishing%20to%20Power%20BI.png?raw=true)



# Snapshot of Dashboard (Power BI Service)

![Power BI Service](https://github.com/vinaysaivutukuri/Loan_Default/blob/main/images/POWER%20BI%20SERVICE.png?raw=true)


 

# 📊 Report Snapshots (Power BI Desktop)

## Report 1: Loan Overview & Default Analysis

![1st Dashboard](https://github.com/vinaysaivutukuri/Loan_Default/blob/main/images/1st%20dashboard%20.png?raw=true)

## Report 2: Applicant Demographics & Financial Profile

![2nd Dashboard](https://github.com/vinaysaivutukuri/Loan_Default/blob/main/images/2nd%20dashboard.png?raw=true)


## Report 3: Risk Metrics, Trends & Advanced Analysis

![3rd Dashboard](https://github.com/vinaysaivutukuri/Loan_Default/blob/main/images/3rd%20dashboard.png?raw=true)

---


# 📊 Insights

A three-page interactive Power BI report was developed and published to Power BI Service to analyze loan default behavior using borrower demographics, financial attributes, credit metrics, and time-based risk indicators.

The following insights were derived from the dashboard:

 ## [1] Loan Dataset Summary

* A total of 2,55,347 loan records were analyzed.

* The dataset includes both defaulted and non-defaulted loans, enabling unbiased risk comparison.

* Loan exposure is concentrated among specific age groups, income levels, and credit segments.

➡️ This provides a strong foundation for credit risk evaluation.

## [2] Credit Score – Primary Risk Driver

* Borrowers in Low and Very Low credit score categories show a significantly higher default rate.

* High credit score customers:

* Have the lowest default probability

* Receive higher average and median loan amounts

* Credit score strongly correlates with repayment behavior across all visuals.

➡️ Credit Score is the most influential factor in predicting loan defaults.

## [3] Income & Employment Risk Analysis

* High-income borrowers consistently show:

* Lower default rates

* More stable repayment behavior

* Medium and low-income groups contribute a larger share of defaults.

* Employment type analysis reveals:

* Certain employment categories carry elevated default risk

* Average income varies widely across employment types

➡️ Combining Income + Employment Type improves borrower risk segmentation.

## [4] Age Group & Loan Exposure Analysis

* Middle Age Adults account for the highest total loan amount.

* Adult and Middle Age segments dominate overall lending activity.

* Average loan size changes across age groups, reflecting different borrowing needs.

➡️ Age-based analysis highlights exposure concentration risk.

## [5] Loan Purpose Risk Patterns

* Business-related loan purposes contribute the largest share of total loan amount.

* Certain loan purposes exhibit:

* Higher loan exposure

* Increased sensitivity to defaults

➡️ Loan purpose plays a critical role in risk diversification.

## [6] Time-Based Risk Trends (YoY & YTD)

* Year-on-Year (YoY) analysis shows:

* Fluctuations in total loan amount

* Changes in defaulted loans across years

* Year-to-Date (YTD) analysis reveals:

* Credit score–wise and marital status–wise loan movement

* Ranking shifts clearly visualized using Ribbon Charts

➡️ Time intelligence enables early risk detection and trend monitoring.

## [7] Mortgage, Dependents & Education Impact

* Middle Age Adults with:

* Mortgages and dependents show distinct loan distribution patterns

* Education-wise loan analysis shows:

   * Clear variation in loan participation by education level

➡️ Personal and social attributes influence borrowing behavior and risk.

## [8] Income Bracket Decomposition

Borrowers were grouped into:

* Low Income

* Medium Income

* High Income

#### Decomposition Tree analysis explains:

* How total loan amount breaks down by income category

* Contribution of each income bracket to overall loan exposure

➡️ Enhances explainability and drill-down analysis.
