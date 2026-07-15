# RFM Customer Segmentation

A customer segmentation project using RFM (Recency, Frequency, Monetary) analysis and K-Means clustering on the Online Retail II dataset. Customers are grouped into actionable segments to support targeted retention, re-engagement, and loyalty strategies.

## Overview

The project processes raw e-commerce transaction data, computes RFM metrics per customer, removes statistical outliers, clusters customers with K-Means, and assigns each customer to a business-friendly segment label (e.g. `RETAIN`, `RE-ENGAGE`, `NURTURE`, `REWARD`).

## Dataset

**Online Retail II** — UK-based online retail transactions.

| Column | Description |
|---|---|
| InvoiceNo | 6-digit transaction number. Prefix `C` indicates a cancellation. |
| StockCode | 5-digit product code. |
| Description | Product name. |
| Quantity | Units purchased per transaction. |
| InvoiceDate | Date and time of transaction. |
| UnitPrice | Price per unit (GBP). |
| CustomerID | 5-digit customer identifier. |
| Country | Customer's country. |

## Methodology

### 1. Data Exploration
Inspected shape, dtypes, duplicates, missing values, and statistical summaries. Reviewed all non-standard `StockCode` prefixes (`DOT`, `DCGS`, `D`, `M`, `C2`, `C3`, `BANK CHARGES`, `S`, `TEST`, `GIFT`, `PADS`, `SP1002`, `ADJUST2`, `AMAZONFEE`) to determine which represent genuine product sales versus administrative entries (postage, discounts, manual adjustments, samples, test data).

### 2. Data Cleaning
- Kept only invoices matching the standard 6-digit format.
- Kept only stock codes matching standard product formats (5-digit, 5-digit + letter suffix, or `PADS`).
- Dropped rows with missing `CustomerID`.
- Removed rows with zero or negative price.

### 3. Feature Engineering (RFM)
- **Monetary Value** — sum of `Quantity × Price` per customer.
- **Frequency** — count of unique invoices per customer.
- **Recency** — days since each customer's last invoice, relative to the most recent invoice in the dataset.

### 4. Outlier Handling
Outliers in Monetary Value and Frequency were identified using the IQR method (1.5× IQR beyond Q1/Q3) and separated from the main population so they don't distort the clustering of typical customers. These outliers are later re-labeled as their own high-value segments rather than discarded.

### 5. Clustering
- Features scaled with `StandardScaler`.
- K-Means evaluated for k = 2 to 12 using inertia (elbow method) and silhouette score.
- Final model: **K-Means with k = 4** on the non-outlier population.

### 6. Segment Labeling
Non-outlier clusters and outlier groups were mapped to business-relevant segment names:

| Segment | Group | Description |
|---|---|---|
| RETAIN | Cluster 0 | High-value, regular buyers — not always recent. Focus on loyalty retention. |
| RE-ENGAGE | Cluster 1 | Lower-value, infrequent, inactive buyers. Focus on win-back campaigns. |
| NURTURE | Cluster 2 | Low-value, low-activity but recent buyers. Focus on relationship building. |
| REWARD | Cluster 3 | High-value, frequent, active buyers. Focus on loyalty rewards. |
| PAMPER | Monetary outliers | High spenders, infrequent purchases. |
| UPSELL | Frequency outliers | Frequent buyers, lower spend per purchase. |
| DELIGHT | Monetary + Frequency outliers | Top-tier customers, high spend and high frequency. |

## Tech Stack

- Python
- pandas
- matplotlib / seaborn
- scikit-learn (`KMeans`, `StandardScaler`, `silhouette_score`)

## Project Structure

```
rfm-customer-segmentation/
├── data/
│   └── online_retail_II.xlsx
├── main.ipynb
└── README.md
```

## Getting Started

```bash
git clone https://github.com/RiteshSnippet/rfm-customer-segmentation.git
cd rfm-customer-segmentation
pip install pandas matplotlib seaborn scikit-learn openpyxl
jupyter notebook main.ipynb
```

## Results

The final output is a customer-level table containing RFM metrics, cluster assignment, and business segment label — ready for use in targeted marketing or CRM workflows.

## Notes

- Monetary values are in British pounds (£), matching the dataset's origin.
- Currency conversion reference used in the notebook: as of 15 July 2026 (9:00 AM UTC), £100 ≈ ₹12,892.30.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

## Author

**Ritesh Kumar Behera**
GitHub: [@RiteshSnippet](https://github.com/RiteshSnippet)