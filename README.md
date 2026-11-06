# E-commerce Order Issue & Support Analytics
 
A Python and Pandas data analysis project that consolidates order, refund and complaint data into one order-level view and ranks orders by how urgently the customer support team should follow up.
 
> **Note:** This is a simulated industry case study based on a Flipkart-style operations scenario, completed as a course project. All data is synthetic and does not represent real Flipkart customers or orders. This project is not affiliated with Flipkart.
 
---
 
## Business problem
 
The customer support team of a large e-commerce platform keeps its data in three separate places:
 
- **Orders** in a CSV file
- **Refund requests** in an Excel file
- **Customer complaints** in a JSON file
Because nothing is connected, support staff have to check three sources to understand a single order. The data also contains duplicate records, inconsistent status labels and missing delivery dates. As a result, delayed deliveries, stuck refunds and repeat complaints are easy to miss.
 
**Goal:** turn the raw data into one reliable, order-level report that shows which orders need attention first.
 
---
 
## Approach
 
| Step | What was done |
|------|---------------|
| 1. Data ingestion | Loaded orders (CSV), refunds (Excel) and complaints (JSON) and assessed structure, volume and completeness |
| 2. Data cleaning | Removed duplicate records, standardized inconsistent status and category labels (e.g. `" delivered"`, `"DELIVERED"`), validated date and amount formats, and separated legitimately missing delivery dates (pending, shipped or cancelled orders) from records needing review |
| 3. Referential checks | Flagged refund and complaint records whose order IDs do not exist in the order data |
| 4. Delivery analysis | Compared actual vs. promised delivery dates to find late orders and ranked cities by delivery delays |
| 5. Refund mapping | Aggregated refund requests per order and identified pending and failed refunds |
| 6. Complaint analysis | Analyzed complaints by type and status, and identified orders and customers with repeated complaints |
| 7. Consolidation | Aggregated one-to-many refund and complaint records to order level before joining, so the final dataset has exactly one row per order; orders with no refunds or complaints are kept. Join integrity is verified with `validate="one_to_one"` and assertions |
| 8. Prioritization | Classified every order as High, Medium or Low priority using three business rules (below) |
| 9. Reporting | Exported a detailed order-level CSV report sorted by priority and an Excel summary of the priority distribution |
 
### Priority rules
 
An order is checked against three issue conditions:
 
1. Delivered **more than 3 days** after the promised delivery date
2. Has **at least one pending or failed** refund request
3. Has received **more than one** customer complaint
| Priority | Rule |
|----------|------|
| High | 2 or more conditions met |
| Medium | Exactly 1 condition met |
| Low | No conditions met |
 
Delivered orders with incomplete delivery information are flagged separately for manual review.
 
---
 
## Key findings
 
**Data quality**
- Removed **70 duplicate records** across the three sources (24 orders, 21 refunds, 25 complaints), leaving 1,600 orders, 1,350 refund requests and 1,500 complaints.
- **38 issue records** (18 refunds, 20 complaints) reference order IDs that do not exist in the order data and were set aside for review.
- **13 orders** marked *Delivered* have no actual delivery date and were flagged rather than silently dropped.
**Delivery performance**
- **651 of 1,113** assessable delivered orders (**58.5%**) arrived after the promised date, and **234 (21.0%)** were more than 3 days late.
- **Jaipur** had the most late deliveries (64), while **Indore** had the highest late rate (**67.1%**).
**Refunds**
- **642 of 1,350** refund requests (**47.6%**) are still pending (454) or failed (188), representing about **₹73.7 lakh** in requested refunds.
**Complaints**
- **881 of 1,500** complaints (**58.7%**) are still open or in progress.
- **Delivery Delay** is the most common complaint type (245), but no single category dominates: all seven types fall within a narrow range, so complaint volume is spread across the whole fulfilment process.
- **395 orders** received more than one complaint.
**Support priority**
- **234 orders (14.6%)** are High priority, 678 (42.4%) Medium and 688 (43.0%) Low, giving the support team a focused first queue instead of 1,600 undifferentiated orders.
---
 
## Outputs
 
| File | Description |
|------|-------------|
| `outputs/flipkart_order_issue_report.csv` | Detailed order-level report with delivery, refund, complaint and priority information, High priority first |
| `outputs/flipkart_issue_summary.xlsx` | Order counts and issue totals for each priority category |
 
---
 
## Tech stack
 
- **Python**
- **Pandas** for data loading, cleaning, merging and aggregation
- **Google Colab**
- Input formats: CSV, Excel, JSON. Output formats: CSV, Excel
---
 
## Repository structure
 
```
├── data/
│   ├── orders.csv
│   ├── refunds.xlsx
│   └── customer_complaints.json
├── outputs/
│   ├── flipkart_order_issue_report.csv
│   └── flipkart_issue_summary.xlsx
├── Flipkart_Order_Issue_Resolution_&_Customer_Support_Analytics.ipynb
└── README.md
```
 
---
 
## How to run
 
1. Open the notebook in Google Colab.
2. Upload the three files from the `data/` folder to the Colab session.
3. Click **Runtime → Run all**.
4. The two reports are generated in the session's file panel, ready to download.
---
 
## Author
 
**Keshav Mittal**: M.Sc. Artificial Intelligence student, Berlin
[LinkedIn](https://www.linkedin.com/in/keshavmittal-ai/) · [GitHub](https://github.com/TechwithKeshav)
 
