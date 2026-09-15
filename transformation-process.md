# Power Query Transformation Process

## ARTHADṚṢṬI

### Amazon Sales Intelligence & Business Performance System

This document describes the data preparation and transformation process used to convert the raw Amazon India sales dataset into a structured dataset suitable for analysis and Power BI reporting.

Power Query was used as the primary data preparation layer before data modeling, DAX measure development, and dashboard creation.

---

## 1. Transformation Objective

The raw dataset contained transactional sales records with inconsistencies in formatting, missing values, categorical fields, geographic information, and status descriptions.

The objective of the transformation process was to:

* Inspect the quality and structure of the raw dataset
* Identify missing and inconsistent values
* Standardize data types
* Clean categorical and geographic fields
* Prepare numerical fields for analysis
* Standardize order status information
* Create a reliable analytical dataset
* Maintain the original business meaning of the source data
* Prepare the dataset for Power BI data modeling and DAX calculations

The transformation process focused on data quality and analytical reliability rather than modifying valid business information unnecessarily.

---

## 2. Source Dataset

The project uses an Amazon India sales transaction dataset.

### Dataset characteristics

| Attribute                | Details                         |
| ------------------------ | ------------------------------- |
| Dataset Type             | Amazon India sales transactions |
| Analysis Period          | 31 March 2022 to 29 June 2022   |
| Approximate Orders       | 120,000                         |
| Approximate Order Lines  | 129,000                         |
| Approximate Units Sold   | 117,000                         |
| Sales Currency           | INR                             |
| Primary Fulfilment Types | Amazon, Merchant                |
| Geographic Coverage      | India                           |

The source dataset contains transactional, product, fulfilment, payment-related, promotional, and geographic information.

---

## 3. Original Columns

The raw dataset contained fields including:

* Index
* Order ID
* Date
* Status
* Fulfilment
* Sales Channel
* Ship Service Level
* Style
* SKU
* Category
* Size
* ASIN
* Courier Status
* Qty
* Currency
* Amount
* Ship City
* Ship State
* Ship Postal Code
* Ship Country
* Promotion IDs
* B2B
* Fulfilled By

These fields were inspected before applying transformations.

---

## 4. Data Profiling

The first stage of the Power Query workflow was data profiling.

Column quality, distribution, and profiling tools were used to understand the condition of the source data before making transformation decisions.

The profiling process focused on:

* Valid values
* Errors
* Empty values
* Distinct values
* Unique values
* Minimum and maximum values
* Data types
* Category distributions
* Geographic consistency
* Status distributions

This ensured that transformations were based on observed data characteristics rather than assumptions.

---

## 5. Date Validation

The `Date` column was inspected and converted to the appropriate date data type.

The dataset covered:

**31 March 2022 to 29 June 2022**

The date field was validated to ensure that it could be used reliably for:

* Monthly sales analysis
* Time-based filtering
* Trend analysis
* Date slicers
* DAX time-based calculations

No major date-quality issue was identified during profiling.

---

## 6. Numerical Data Preparation

### 6.1 Amount

The `Amount` field was inspected for:

* Missing values
* Zero values
* Negative values
* Decimal values
* Invalid entries

The profiling process showed that the majority of values were valid, while a smaller proportion contained blanks.

The dataset contained:

* Positive sales amounts
* Zero-value records
* Blank amount values
* No negative sales amounts identified

Blank amount values were handled carefully rather than being automatically replaced with arbitrary values.

This was important because replacing missing financial values with zero without understanding their meaning could distort sales calculations.

---

### 6.2 Quantity

The `Qty` field was validated as a numerical field.

The observed characteristics included:

* Maximum quantity per record: 5
* No zero quantities identified
* No negative quantities identified

The field was retained for unit-sales analysis.

---

## 7. Currency Validation

The `Currency` field was examined alongside the `Amount` column.

The dataset primarily contained INR transactions.

Blank currency and amount values were investigated during profiling to ensure that missing values did not represent a different currency or a separate transaction type.

This helped prevent incorrect aggregation of monetary values.

---

## 8. Fulfilment Data

The `Fulfilment` field contained two major fulfilment categories:

* Amazon
* Merchant

The `Fulfilled By` field was also examined.

The observed relationship was:

* Amazon fulfilment records may have blank `Fulfilled By` values
* Merchant fulfilment records included Easy Ship information

Blank values in `Fulfilled By` were therefore not automatically treated as data errors because their meaning was related to the fulfilment process.

This distinction was important for preserving the operational meaning of the dataset.

---

## 9. Order Status Standardization

The `Status` field contained multiple order-status descriptions.

Important status values included:

* Cancelled
* Shipped
* Shipped - Delivered to Buyer

The status information was analyzed to support order-performance reporting.

Because the raw dataset does not contain a simple `Delivered` status value, delivered orders were interpreted using the appropriate existing status description rather than inventing a new raw status.

Where grouped status logic was required for reporting, it was separated from the original status information.

This helped preserve the source data while allowing business-level categorization.

---

## 10. Geographic Data Cleaning

Geographic fields required additional preparation because city and state values contained variations in formatting.

The following fields were reviewed:

* Ship City
* Ship State
* Ship Postal Code
* Ship Country

### State

The state field was cleaned and standardized to improve consistency in geographic analysis.

After cleaning:

* 37 distinct states/regions were identified
* No unique state values remained that required separate standardization

### City

City names were standardized using text-cleaning and proper-case transformations where appropriate.

After transformation:

* Approximately 4,002 distinct city values
* Approximately 2,057 unique city values

The purpose was to reduce formatting differences while retaining the geographic identity of the records.

### Country

The `Ship Country` field was checked for consistency.

The dataset was primarily associated with India, with a very small number of blank values.

---

## 11. Category Standardization

The product category field was inspected for consistency.

The major categories identified were:

* Blouse
* Bottom
* Ethnic Dress
* Kurta
* Saree
* Set
* Top
* Western Dress

The category values were retained as business dimensions for product and sales analysis.

---

## 12. Promotion Data

The `Promotion IDs` field was inspected because promotional information can contain a large number of distinct values.

After cleaning and profiling, the promotion field contained a high number of distinct and unique values.

The field was retained for potential promotional analysis rather than unnecessarily reducing its granularity.

This allows future analysis of promotional effectiveness without changing the underlying transaction records.

---

## 13. Text Cleaning

Text fields were reviewed for formatting inconsistencies.

Typical Power Query operations included:

* Trim
* Clean
* Proper Case where appropriate
* Data type correction
* Standardization of categorical values
* Handling of blank values

These transformations were applied selectively.

Proper Case was particularly useful for geographic fields such as city names.

The goal was not to transform every text column indiscriminately, but to improve consistency where formatting affected analysis.

---

## 14. Handling Missing Values

Missing values were investigated based on the meaning of each field.

The following principle was followed:

> A blank value was not automatically considered an error.

For example, a blank `Fulfilled By` value could be associated with Amazon fulfilment and therefore had a business meaning.

Similarly, blank monetary fields required investigation before being classified as zero.

Missing values were therefore handled according to their business context.

---

## 15. Data Type Standardization

Appropriate data types were assigned to analytical fields.

| Field              | Intended Data Type                              |
| ------------------ | ----------------------------------------------- |
| Order ID           | Text                                            |
| Date               | Date                                            |
| Status             | Text                                            |
| Fulfilment         | Text                                            |
| Sales Channel      | Text                                            |
| Ship Service Level | Text                                            |
| Style              | Text                                            |
| SKU                | Text                                            |
| Category           | Text                                            |
| Size               | Text                                            |
| ASIN               | Text                                            |
| Courier Status     | Text                                            |
| Qty                | Whole Number                                    |
| Currency           | Text                                            |
| Amount             | Decimal Number                                  |
| Ship City          | Text                                            |
| Ship State         | Text                                            |
| Ship Postal Code   | Text                                            |
| Ship Country       | Text                                            |
| B2B                | Logical/Text depending on source representation |

Correct data types were necessary for reliable aggregation, filtering, sorting, and DAX calculations.

---

## 16. Before and After Transformation

### Before Power Query

The source data contained:

* Inconsistent text formatting
* Blank values
* Mixed data-quality conditions
* Geographic naming variations
* Raw order-status descriptions
* Fields requiring type validation
* Data requiring profiling before analysis

### After Power Query

The transformed dataset provided:

* Standardized data types
* Cleaner geographic fields
* Validated numerical fields
* Structured categorical dimensions
* Reviewed missing values
* Consistent status information
* Analysis-ready transactional data

The transformation stage therefore acted as the quality-control layer between the raw dataset and the Power BI analytical model.

---

## 17. Transformation Workflow

The Power Query process followed this sequence:

### Stage 1: Source Connection

The Amazon sales dataset was loaded into Power Query.

### Stage 2: Data Profiling

Column quality, distribution, distinct values, and data types were examined.

### Stage 3: Data Type Correction

Fields were assigned appropriate numerical, date, text, and logical data types.

### Stage 4: Data Cleaning

Unnecessary formatting inconsistencies were addressed using appropriate text and value transformations.

### Stage 5: Missing-Value Investigation

Blank values were analyzed according to their business meaning.

### Stage 6: Geographic Standardization

City, state, and country fields were cleaned for consistent geographic reporting.

### Stage 7: Category and Status Preparation

Product categories and order statuses were reviewed and prepared for dashboard analysis.

### Stage 8: Validation

The transformed dataset was checked against the source data to ensure that the transformation process did not unintentionally alter the underlying business information.

### Stage 9: Power BI Model Loading

The cleaned dataset was loaded into Power BI for data modeling and DAX measure development.

---

## 18. Relationship with the Power BI Model

After transformation, the cleaned transactional data was loaded into Power BI as:

`Fact_Amazon_Sales`

A separate `_Measures` table was used to organize DAX measures.

The transformed transaction table served as the foundation for:

* Sales analysis
* Order analysis
* Product analysis
* Fulfilment analysis
* Geographic analysis
* KPI calculations
* Dashboard filtering

Power Query therefore formed the ETL layer of the ARTHADṚṢṬI system.

---

## 19. Data Quality Principles

The transformation process followed five major principles:

### 1. Preserve Business Meaning

Valid source information was not changed without a reason.

### 2. Transform Only Where Necessary

Cleaning operations were applied when they improved analytical consistency.

### 3. Investigate Before Replacing

Missing or unusual values were investigated before assigning replacements.

### 4. Separate Preparation from Analysis

Power Query was used for data preparation, while DAX was used for analytical calculations.

### 5. Validate After Transformation

The final dataset was checked to ensure that transformation did not introduce unintended changes.

---

## 20. Final Outcome

The Power Query transformation process converted the raw Amazon sales dataset into a cleaner and structured analytical dataset suitable for Power BI.

The resulting data supported the development of the ARTHADṚṢṬI Business Intelligence system, including:

* Executive marketplace reporting
* Sales performance analysis
* Order and fulfilment analysis
* Product-level analysis
* Geographic distribution analysis
* KPI monitoring
* Interactive filtering

Power Query was therefore a critical part of the project rather than simply a preliminary cleaning step.

It established the data-quality foundation required for reliable Power BI analysis and business reporting.

---

## 21. Evidence Included in the Repository

The repository contains supporting development evidence where applicable:

* Raw dataset screenshot
* Power Query data profiling screenshot
* Power Query transformation screenshots
* Transformed data screenshot
* Data model screenshot
* DAX development evidence
* Final Power BI dashboard screenshots

These files document the progression from raw transactional data to the final Business Intelligence system.

---

## 22. Next Stage

After completing the Power Query transformation process, the cleaned dataset was used for:

1. Data modeling
2. DAX measure development
3. KPI creation
4. Dashboard development
5. Validation
6. Business insight generation

The next layer of the project is documented through the DAX and data-model documentation in this repository.
