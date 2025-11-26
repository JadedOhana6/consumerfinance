# Consumer Finance Complaints (incomplete).
Link to Dashboard : https://app.powerbi.com/reportEmbed?reportId=0ce5f813-bcef-4b36-bc6d-83067141bc91&autoAuth=true&ctid=b065f3d5-e27a-4caf-9216-790f5cdfe5ee


## Table of Contents
-	[Project Overview](#project-overview)
- [Tools Used](#tools-used)
- [Data_Source & Tables](#data-source-and-tables)
- [Data Cleaning & Preparation](#data-cleaning-and-preparation)
- [Exploratory Analysis](#exploratory-analysis)
- [Interesting codes](#interesting-codes)
- [Limitations](#limitations)

### Project Overview
This analysis aims to identify patterns in complaints, evaluates company responses, and highlights areas of concern for regulators and consumers.

The CFPB collects complaints from consumers across the United States regarding financial products and services. Each complaint is tracked from submission through company response. The dataset contains consumer complaints spanning multiple financial categories, locations, and companies. 

### Tools Used
1. Excel for cleaning
2. PowerBI for visualization & analysis

### Data Source and Tables
[ZOOMCHARTS October challenge](https://zoomcharts.com/en/microsoft-power-bi-custom-visuals/challenges/onyx-data-october-2025) 
- Company List
- Complaints List

## Data Cleaning and Preparation
The following tasks were performed: Checked for duplicates, correct formatting. 

## Exploratory Analysis & Findings
1. When do complaints rise or fall over time?
-- There are more complaints in the first half of the year than in the next half. 
-- Companies also receive more complaints on weekdays than weekends.

2. Which states and regions are hotspots?
-- The South Region has the most number of complaints (37%), followed by the West Region (32%). But the state with the most number of complaints is CA in the West (21%) & FL in the South (10%). 

3. Which products and sub-products drive the most complaints?
-- Product (all time): Banking (39.69%) ; Sub-product (all time): Checking Account (33.22%)

4. Which issues and sub-issues are most common or most severe?
-- Issue (all time): Managing an Account (24.17%); Sub-issue (all time): Deposit & Withdrawals (8.95%)

5. How fast, and how timely are company responses to complaints?
-- Each company has an average timely response rate of 93.77%
   
6. Which companies show the highest complaint rates relative to market share?
-- Small Companies with No Enforcement history have the highest avg complaints per 1% share (2.25K).

7. Do company traits (size tier, reputation, enforcement history) correlate with outcomes?
-- The smaller the size, the larger the avg complaints per 1% share. Companies in the small size tier have the highest avg complaints per 1% share (2.24K), followed by the medium tier (400.21) and the large tier (203.61).
-- Companies with No Enforcement History score have higher (diff 0.54%) reputation scores than those with enforcement history.

8. Do submission channels differ in response speed or outcomes?
-- Yes. Complaints received via email and fax have the highest timely response rate (100%, 99.57%). The days between submitted and received is the lowest of all the submission channels (0.5 & 0.48)

9. Others:
-- Rate of timely response is the lowest in 2023 (77.92% vs 95.62% in 2022). 50.66% in July, 18.13% in June. 

## Interesting Features
1. Created a button to display all filters. DAX formula below:

SELECTED FILTER CATEGORY = 

VAR Maxfilters = 3

RETURN (
    
    IF( //Date, Month
        OR(ISFILTERED('Date Table'[Year]), ISFILTERED('Date Table'[Mo_Short])),
        VAR __x = "Year/Month Submitted"
        RETURN __x & UNICHAR(13) & UNICHAR(10)
    )&
    IF( //Day
        ISFILTERED('Date Table'[Day]), 
        VAR __x = "Day"
        RETURN __x & UNICHAR(13) & UNICHAR(10)
    )& ...
    )
    
#SELECTED FILTERS = 

VAR Maxfilters = 3

RETURN (
    
    IF( //YEAR & MONTH
        OR(ISFILTERED('Date Table'[Year]), ISFILTERED('Date Table'[Mo_Short])),
        VAR __y = SELECTEDVALUE('Date Table'[Year])
        VAR __m = SELECTEDVALUE('Date Table'[Mo_Short])
        RETURN
        __y & " / " &
        IF(
            NOT ISBLANK(__m), __m, "-" // Otherwise, add nothing
        ) & UNICHAR(13) & UNICHAR(10) 
    )&
    IF( // DAY
        ISFILTERED('Date Table'[Day]), 
        VAR __f = FILTERS('Date Table'[Day])
        VAR __r = COUNTROWS(__f)
        VAR __t = TOPN( Maxfilters, __f, 'Date Table'[Day])
        Var __d = CONCATENATEX(__t, 'Date Table'[Day], ", ")
        VAR __x = __d & IF( __r > Maxfilters, ", ... [+" & __r - Maxfilters & " items")
        RETURN __x & UNICHAR(13) & UNICHAR(10)
    )&...
    )
2. Added a 'Clear filters' & User guide button

## Limitations
Data updated until August 2023. Hence average is used when analyzing complaints and response trends. 
