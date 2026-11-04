# Infrastructure Project Cost Intelligence & Analytics
### Power BI | Power Query | DAX | Data Modelling | BCIS | Cost Normalization | Currency Standardization

## 1. Project Overview

This project focuses on developing an infrastructure project cost intelligence solution by consolidating, transforming and analysing historical and live project cost data.

The objective is to create a structured analytical framework that enables consistent project cost evaluation across different projects, reporting periods, currencies and work breakdown structures.

Using Microsoft Power BI, Power Query and DAX, the project explores the complete data analytics lifecycle, from raw data preparation and dimensional modelling to advanced cost analysis, historical cost normalization, currency standardization, benchmarking methodology and cost variance decomposition.

The analysis incorporates three historical infrastructure projects (A, B and C) and one LIVE project containing actual and forecast expenditure records.

A key aspect of the project is addressing the challenges associated with comparing infrastructure project costs across different time periods, currencies, project types and measurement units.

---

## 2. Business Objectives

The project addresses several challenges commonly encountered in infrastructure project cost management:

- Consolidating cost information from multiple project datasets.
- Improving data quality and consistency across historical and live project records.
- Establishing a structured data model for analytical reporting.
- Standardizing cost information across different currencies.
- Adjusting historical expenditure to a common price reference period.
- Developing a methodology for comparing costs across projects.
- Understanding the impact of unit-rate and quantity changes on total expenditure.
- Analysing actual versus forecast expenditure to identify potential cost variations.

The overall objective is to transform fragmented project cost data into a structured and reusable cost intelligence framework.

---

## 3. Data Sources and Dataset Structure

The project uses five Excel workbooks containing project cost information and supporting reference data.

### Historical Project Datasets

**Project A, Project B and Project C**

The historical datasets contain item-level cost information, including:

- Project_Code
- Item
- WBS_Code
- Quantity
- Rate
- Currency
- Item_Date
- Total_Cost

These datasets provide the basis for historical project cost analysis and cross-project comparisons.

Project B includes multiple currencies (GBP, USD and EUR), introducing an additional currency standardization requirement.

### LIVE Project Dataset

The LIVE dataset contains current project expenditure and forecast information.

Key fields include:

- Project_Code
- Item
- WBS_L1
- WBS_L2
- WBS_L3
- Quantity
- Rate
- Currency
- Item_Date
- Total_Cost
- Status

The Status field distinguishes Actual and Forecast records, enabling cost performance and variance analysis.

### Ancillary Reference Data

Three supporting datasets are incorporated:

**1. BCIS Indices**

Contains the BCIS All-in Tender Price Index (Series 101), providing quarterly historical construction price index values and associated forecast information.

**2. Work Breakdown Structure (WBS)**

Contains a structured WBS hierarchy with 48 mapping records, linking WBS codes to their corresponding Level 1, Level 2 and Level 3 descriptions.

**3. Project Information**

Contains project-level metadata, including:

- Project_Code
- Overall_Qty
- Overall_Unit
- Project Type
- Location

This information provides additional context for project-level analysis and comparison.

---

## 4. Data Cleaning and Transformation Using Power Query

Power Query was used as the primary data preparation and transformation layer.

The objective was to standardize the structure of the source datasets and prepare them for integration into a consistent analytical model.

### 4.1 Data Import and Consolidation

- Imported multiple Excel workbooks into Power BI.
- Organized historical project datasets and LIVE project data into separate queries.
- Standardized common field structures across project datasets.
- Prepared the datasets for consolidation into a unified cost fact table.
- Imported ancillary reference datasets separately for dimensional modelling.

### 4.2 Data Cleaning

Performed data preparation activities to improve data consistency and reliability, including:

- Reviewing column names and standardizing field naming conventions.
- Identifying missing and inconsistent values.
- Checking date fields and converting them into appropriate date data types.
- Ensuring quantity, rate and total cost fields use appropriate numeric data types.
- Standardizing currency and status fields.
- Reviewing project identifiers and item-level records.
- Checking WBS codes for consistency and mapping compatibility.
- Reviewing data quality issues before loading the transformed data into the model.

### 4.3 Data Type Standardization

Applied appropriate data types to support accurate calculations and relationships.

| Data Category | Data Type |
|---|---|
| Project identifiers | Text |
| Item identifiers | Text |
| WBS codes | Text |
| Quantity | Decimal Number |
| Rate | Decimal Number |
| Total Cost | Decimal Number |
| Currency | Text |
| Item Date | Date |
| Status | Text |

WBS codes were maintained as text identifiers to preserve their hierarchical structure and leading zeros.

### 4.4 WBS Mapping and Data Integration

The WBS reference dataset was used to establish a structured relationship between individual cost records and their corresponding work-package classifications.

This supports analysis across three levels:

- WBS Level 1
- WBS Level 2
- WBS Level 3

The mapping provides a consistent classification framework for historical and live project expenditure.

### 4.5 Project Metadata Integration

Project-level reference information was incorporated to provide additional analytical dimensions, including project type, location, overall quantity and measurement units.

This enables project expenditure to be interpreted in the context of its underlying project characteristics.

---

## 5. Dimensional Data Modelling — Star Schema

After preparing the datasets, a dimensional data model was developed in Power BI.

A star-schema approach was used to separate transactional cost information from descriptive reference information.

### Central Fact Table

**Fact_Cost**

Contains consolidated item-level cost records from historical and live projects.

The table supports analysis of:

- Project expenditure
- Quantity
- Unit rate
- Currency
- Cost dates
- Actual and forecast status
- Work-package classifications

### Dimension Tables

**Dim_Project**

Provides project-level descriptive information, including project identifiers, project type, location and overall measurement information.

**Dim_Date**

Provides a dedicated calendar structure to support time-based analysis, including:

- Year
- Quarter
- Month
- Month number
- Year-month
- Date hierarchy

**Dim_WBS**

Provides the hierarchical work breakdown structure and corresponding descriptions.

**BCIS_Index**

Stores historical and forecast BCIS index information for cost normalization analysis.

### Modelling Approach

Relationships were established between the central fact table and the supporting dimensions using appropriate project, date and WBS keys.

The dimensional model supports filtering, aggregation and analysis across multiple reporting dimensions while reducing unnecessary duplication of descriptive information.

---

## 6. Currency Standardization and Normalization

Currency standardization is an important component of the project because the historical project datasets do not all use the same currency composition.

Project B contains GBP, USD and EUR records, whereas the other supplied project datasets contain GBP records.

The currency analysis addresses the need to establish a consistent reporting currency before aggregating and comparing project expenditure.

The methodology considers:

- Identifying the original currency of each cost record.
- Classifying single-currency and mixed-currency project data.
- Applying an appropriate exchange-rate conversion methodology.
- Converting non-GBP costs into a common GBP reporting basis.
- Maintaining a distinction between original currency values and standardized reporting values.
- Ensuring currency differences do not distort cross-project cost comparisons.

The general conversion methodology is:

**GBP Cost = Original Currency Cost × Applicable GBP Exchange Rate**

The applicable exchange rate should be selected using a consistent date convention, such as the transaction date or an explicitly defined reporting-date basis.

Currency standardization is also considered when determining whether cost records are suitable for direct benchmarking.

---

## 7. BCIS Index Selection and Cost Normalization

One of the key analytical components of this project is historical construction cost normalization.

Infrastructure projects undertaken in different years may have significantly different cost bases due to construction market inflation and changes in tender prices.

Comparing historical expenditure without adjusting for these movements can produce misleading conclusions.

### 7.1 BCIS Index Selection

The BCIS All-in Tender Price Index (Series 101) was selected as the reference index for the historical cost adjustment methodology.

The supplied index dataset contains quarterly observations and forecast information.

The selection process considers:

- The relevant historical project cost date.
- The corresponding quarterly BCIS index.
- The selected target price period.
- The distinction between historical, provisional and forecast index observations.
- Consistency in the selected normalization reference period.

### 7.2 Historical Index Mapping

Historical cost records are associated with the relevant BCIS index period using their original cost dates.

Where necessary, the index lookup uses the most recent available quarterly observation on or before the applicable cost date.

This establishes a consistent relationship between the original cost period and the selected reference index.

### 7.3 Cost Normalization Calculation

Historical project costs are adjusted using the selected BCIS index values.

**Normalized Cost = Historical Cost × (Target BCIS Index / Historical BCIS Index)**

Where:

- Historical Cost = Original cost expressed in the standardized reporting currency.
- Target BCIS Index = Index value for the selected reference period.
- Historical BCIS Index = Index value corresponding to the original cost period.

The purpose is to express historical expenditure on a common price basis and improve the comparability of costs across different periods.

### 7.4 Normalization Analysis

The analytical framework considers:

- Historical project expenditure.
- Normalized project expenditure.
- Index uplift factors.
- Normalization impact.
- Differences between original and adjusted costs.
- Cost movements across different project periods.

This provides a consistent methodology for evaluating historical infrastructure expenditure in the context of construction price movements.

---

## 8. Cost Benchmarking Methodology

The project also explores cost benchmarking to evaluate the comparability of expenditure across infrastructure projects.

Benchmarking requires more than comparing total project costs because projects may differ in their scope, measurement units, currency composition and work-package characteristics.

### 8.1 Benchmark Eligibility Classification

A benchmark eligibility framework was established to distinguish:

- Comparable: Records considered suitable for direct comparison under the selected eligibility rules.
- Directional Only: Records that may provide contextual information but should not automatically be treated as directly comparable.

### 8.2 Benchmarking Considerations

The methodology considers:

- Currency consistency.
- Comparable cost categories.
- Work-package alignment.
- Quantity and unit consistency.
- Historical cost normalization.
- Differences in project type and scope.

### 8.3 Unit Rate Comparison

Unit-rate analysis provides a more meaningful comparison than total project expenditure when the underlying cost scope and measurement units are consistent.

The general calculation is:

**Unit Rate = Total Cost / Quantity**

Weighted average rates can also be used when aggregating comparable records:

**Weighted Average Rate = Total Comparable Cost / Total Comparable Quantity**

The purpose is to identify differences in unit costs while accounting for the underlying quantities.

---

## 9. Cost Variance Analysis — Actual vs Forecast

The LIVE project dataset contains both Actual and Forecast expenditure records.

This enables analysis of the difference between incurred project costs and expected future expenditure.

### Key Calculations

**Actual Cost**

Aggregates cost records classified as Actual.

**Forecast Cost**

Aggregates cost records classified as Forecast.

**Cost Variance**

Forecast Cost - Actual Cost

**Cost Variance Percentage**

(Cost Variance / Actual Cost) × 100

The analysis is structured to identify differences in project expenditure and highlight potential cost overruns.

Variance analysis can be performed across:

- Project
- Reporting period
- WBS Level 1
- WBS Level 2
- WBS Level 3

The sign convention used identifies a positive variance when forecast expenditure exceeds actual expenditure.

---

## 10. Rate and Quantity Impact Analysis

A further analytical component involves understanding the drivers behind changes in total project expenditure.

Total expenditure is influenced by both the quantity of resources or work undertaken and the applicable unit rates.

The project explores the decomposition of cost changes into:

### Rate Impact

Measures the contribution of changes in unit rates to overall cost movements.

### Quantity Impact

Measures the contribution of changes in quantities to overall cost movements.

The general relationship is:

**Total Cost = Quantity × Rate**

The decomposition methodology compares a defined baseline with the forecast or comparison scenario to isolate rate-driven and quantity-driven changes.

The analytical approach considers the interaction between rate and quantity changes to ensure that the overall cost movement is reconciled without double-counting.

This provides additional insight into whether cost variations are driven by price movements, changes in project quantities or a combination of both.

---

## 11. DAX and Analytical Calculations

DAX was used to develop measures for dynamic cost calculations and analytical reporting.

The measure framework covers:

- Total project cost.
- Actual expenditure.
- Forecast expenditure.
- Normalized cost.
- Historical and current BCIS index references.
- Cost variance and variance percentage.
- Rate impact.
- Quantity impact.
- Project cost contribution.
- Cost aggregation across WBS levels.
- Project-level and time-based analysis.

The measures are designed to respond to project, date and work-package filters through the dimensional data model.

---

## 12. Business Value and Analytical Outcomes

The project demonstrates how raw infrastructure cost information can be transformed into a structured cost intelligence solution.

The analytical framework supports:

- Consolidated reporting across multiple infrastructure projects.
- Improved data consistency through Power Query transformations.
- Structured analysis using a dimensional data model.
- More consistent currency-based cost reporting.
- Historical cost adjustment using BCIS index methodology.
- Better interpretation of cross-project cost comparability.
- Identification of actual-versus-forecast expenditure differences.
- Investigation of rate and quantity-driven cost changes.
- Improved visibility into project expenditure and cost performance.

---

## 14. Technology and Skills

**Tools and Technologies**

- Microsoft Power BI
- Power Query
- DAX
- Microsoft Excel
- Dimensional Data Modelling
- Star Schema Design
- BCIS All-in Tender Price Index

**Technical Skills Demonstrated**

- Data Cleaning and Transformation
- Data Integration
- Data Quality Assessment
- Dimensional Modelling
- Currency Standardization Methodology
- Cost Normalization
- Cost Benchmarking Methodology
- Financial and Operational Analysis
- Forecast and Variance Analysis
- Rate and Quantity Impact Decomposition
- Business Intelligence and Data Storytelling

---

## Conclusion

The Infrastructure Project Cost Intelligence project demonstrates an end-to-end analytical approach to infrastructure project cost management.

It combines data preparation, dimensional modelling, cost standardization, historical cost normalization, benchmarking methodology and financial variance analysis to address practical challenges in project cost evaluation.

The project goes beyond basic expenditure reporting by examining how currency differences, construction price movements, project comparability, unit rates and quantities influence the interpretation of infrastructure project costs.

**Project Domain:** Infrastructure | Construction | Cost Intelligence | Financial Analytics | Business Intelligence

**Tools:** Power BI | Power Query | DAX | Excel | BCIS Index
