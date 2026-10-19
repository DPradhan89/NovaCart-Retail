# NovaCart-Retail
# E-Commerce Profitability & Customer Retention Analysis — NovaCart Retail

## Project Overview
I worked as a Data Analyst at **NovaCart Retail**, a mid-sized e-commerce company. The leadership team wanted to know where the company was losing profit. They also wanted to see if their marketing budget and loyalty discounts were working. 

I cleaned the raw dataset using **Python (Jupyter Notebook)**, analyzed business performance in **PostgreSQL**, and built an interactive dashboard in **Power BI**. This project tracks true realized profit, customer acquisition efficiency, and year-on-year customer retention.

---

## Business Problems
NovaCart Retail faced three main challenges:
1. **Dirty Raw Data:** The raw CSV files had wrong data types, missing values, text mixed with numbers, and duplicate order records from failed payment attempts.
2. **Unclear Marketing ROI:** Leadership did not know if spending $42.16 to acquire a new customer was a good or bad investment.
3. **Lower Margins in Top Tiers:** The `VIP` and `Premium` customer segments showed lower profit margins than regular shoppers.

---

## Dataset Summary
The database contains four main tables:
* **`Customers`:** 25,000 unique customer profiles with demographics, segments, and acquisition costs[cite: 3].
* **`Sales`:** 138,116 cleaned orders tracking net sales, discounts, shipping costs, delivery status, and returns[cite: 3].
* **`Order_Items`:** Line-level product quantities, unit prices, and wholesale product costs[cite: 3].
* **`Product_Catalog`:** Product names, categories, brands, and supplier details[cite: 3].

---

## Data Cleaning & Preparation (Python — Jupyter Notebook)
Before loading the data into PostgreSQL and Power BI, I cleaned the raw files in **Jupyter Notebook** using **Pandas** and **NumPy**:

### 1. Fixing Wrong Data Types & Messy Text
* **Date Conversion:** Changed the date columns from string (`STR`) to `DATE` format using `pd.to_datetime(..., format='mixed', errors='coerce')`.
* **Gender Standardization:** Used `np.where()` to keep `'M'` and `'F'` values and transform `'Non-binary'` into `'Others'`.
* **Customer Age Cleaning:** Removed unwanted text strings from the `age` column (such as `'25 years'` and `'19yr'`) and converted the column to a numeric data type.

### 2. Handling Missing Values (NULLs)
* **Delivery Days:** Grouped the data by delivery city using `.groupby()` and filled missing `delivery_days` values with each city's `.median()`.
* **Return Status:** Converted `return_status` into binary numbers (`1` if `'Returned'`, else `0`).
* **Return Reason:** Filled `NULL` values in `return_reason` with `'Others'`.
* **Campaign Name:** Filled missing marketing campaign names with `'Unknown'`.
* **Coupon Code:** Filled missing coupon codes with `'No coupon'`.
* **Customer Age:** Filled missing age values with the average customer age using `.fillna(df['age'].mean())`.

### 3. Removing Duplicate Orders
* The raw `Sales` table had **10 duplicate `order_id` records**.
* These duplicates happened because customers had payment method issues on their first attempt and retried the order.
* I removed the duplicates using `df.drop_duplicates(subset=['order_id'], keep='last')`. I kept the `'last'` row because it contained the final, successful order placement date.
## Key Analytical Findings
### 1. Customer Acquisition ROI (LTV:CAC Ratio)
* The company spent $1,053,993.76 in total to acquire 25,000 customers.
* The average Customer Acquisition Cost (CAC) is $42.16 per customer.
* Out of 138,116 attempted orders, 113,559 orders were successfully completed and kept.
* Each customer completes an average of 4.54 successful orders over their lifetime.
* Each completed order generates $526.23 in realized profit (totaling $59.76M in realized profit across the company).
* The average Customer Lifetime Value (LTV) in realized profit is $2,390 ($2,390.35).
* This gives NovaCart an LTV:CAC ratio of ~57:1 (compared to the industry target of 3:1).
* **Insight:** The company earns back its $42.16 marketing cost on the customer's very first order. NovaCart is spending too                      little on marketing and can safely increase its ad budget to attract more buyers.

