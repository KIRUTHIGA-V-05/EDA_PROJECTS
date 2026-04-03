# E-Commerce Data Analysis — Brazilian Olist Dataset

An exploratory data analysis (EDA) project on the Brazilian Olist e-commerce dataset. The analysis examines customer behavior, delivery performance, product category trends, payment patterns, and review scores across 9 interconnected datasets.

Dataset source: [Kaggle — Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

---

## Objective

- Understand customer behavior and order patterns across regions and time
- Analyze delivery performance and its effect on customer satisfaction
- Identify top-performing product categories by order volume
- Examine how payment type relates to purchase value
- Derive business insights that can guide logistics and marketing decisions

---

## Dataset

The dataset consists of 9 CSV files that are merged into a single master dataframe for analysis.

| File | Description |
|---|---|
| `olist_customers_dataset.csv` | Customer IDs and state locations |
| `olist_orders_dataset.csv` | Order lifecycle timestamps |
| `olist_order_items_dataset.csv` | Items within each order |
| `olist_order_payments_dataset.csv` | Payment type and amount |
| `olist_order_reviews_dataset.csv` | Customer review scores and comments |
| `olist_products_dataset.csv` | Product details and category |
| `olist_sellers_dataset.csv` | Seller location information |
| `olist_geolocation_dataset.csv` | ZIP code to coordinate mapping |
| `product_category_name_translation.csv` | Portuguese to English category names |


---


## Pipeline Overview

### 1. Data Loading
All 9 datasets loaded individually using pandas, with an initial structural overview of each.

### 2. Data Cleaning
- Dropped rows with missing `customer_id` or `customer_state`
- Removed duplicate rows in the geolocation dataset
- Merged all 9 datasets into a single master dataframe `orders_full` using left joins on `customer_id`, `order_id`, `product_id`, and `seller_id`
- Handled missing values in `product_category_name_english` and `product_weight_g`
- Removed duplicate rows from the merged dataframe
- Removed price outliers using the IQR method (1.5x threshold)

### 3. Feature Engineering
| Feature | Description |
|---|---|
| `purchase_year` | Year extracted from order purchase timestamp |
| `purchase_month` | Month extracted from order purchase timestamp |
| `purchase_day` | Day of month |
| `purchase_weekday` | Day of week (0 = Monday) |
| `delivery_time_days` | Days between purchase and delivery |

---

## Visualizations and Key Insights

**Distribution of Delivery Time**
Most deliveries are completed within 5–15 days. The distribution is right-skewed with a long tail of delays beyond 50 days and extreme cases exceeding 150 days, pointing to occasional logistics failures.

**Top 10 Product Categories by Order Count**
Bed, Bath and Table dominates with nearly 12,000 orders. Health and Beauty ranks second with around 9,000 orders. Furniture, Sports, and Computers Accessories cluster closely in the 8,000–8,500 range, reflecting balanced demand across home and lifestyle segments.

**Delivery Time vs Review Score (scatter)**
Review scores are spread across the full range (1–5) even at short delivery times, indicating delivery alone does not determine satisfaction. However, extreme delays above 150 days are associated with more 1-star reviews.

**Price Distribution by Payment Type**
Credit card and boleto users make slightly higher-value purchases with more variation. Debit card users show the narrowest spend range. Voucher transactions produce occasional very high outliers likely representing bulk or gift purchases.

**Orders Over Time**
Order volume grew steadily from late 2016, peaked sharply in November 2017 (exceeding 8,000 orders), and stabilized at 7,000–8,000 through the first half of 2018 before declining. The near-zero count in September 2018 is likely a data completeness issue rather than a real business drop.

**Review Score vs Delivery Time by Category (bubble chart)**
Most categories cluster between 8–13 days delivery and achieve review scores between 3.8 and 4.5. Categories with delivery times beyond 15 days see review scores fall to 2.5–3.5. Higher-priced categories do not consistently achieve better reviews — delivery time is a stronger driver of satisfaction than price or category.

---

## Key Business Takeaways

- Keeping delivery times within 12 days is the most reliable way to maintain review scores above 4.0
- Household essentials and personal care dominate demand — strong inventory priority for these categories is justified
- Credit card is the dominant payment method and associated with higher-value purchases
- The November 2017 spike likely reflects a seasonal event (Black Friday) and represents the platform's peak demand period
- Extreme delivery delays are rare but disproportionately damaging to review scores

---

## How to Run

```bash
pip install -r requirements.txt
jupyter notebook E_Commerce_analysis.ipynb
```

Update the file paths in Cell 4 to point to your local copies of the CSV files before running.

---

## Tech Stack

- Python 3
- pandas, numpy — data manipulation and merging
- matplotlib, seaborn — visualization

