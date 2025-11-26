# Consumer Finance Complaints
Link to Report : https://app.powerbi.com/reportEmbed?reportId=0ce5f813-bcef-4b36-bc6d-83067141bc91&autoAuth=true&ctid=b065f3d5-e27a-4caf-9216-790f5cdfe5ee

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
The following tasks were performed:
1. Checked for duplicates, correct formatting. 

## Exploratory Analysis
1. When do complaints rise or fall over time?
2. Which states and regions are hotspots?
3. Which products and sub-products drive the most complaints?
4. Which issues and sub-issues are most common or most severe?
5. How fast, and how timely are company responses to complaints?
6. Which companies show the highest complaint rates relative to market share?
7. Do company traits (size tier, reputation, enforcement history) correlate with outcomes?
8. Do submission channels differ in response speed or outcomes?

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
