# STADIOalot — Product Returns: Data Extract

Seven tables covering **1 July 2023 to 30 June 2026** — the three years of overlap you asked for, bounded by your shortest source (clickstream). Joined on `Order_ID`, `Order_Item_ID`, `Customer_ID`, `SKU` and `Seller_ID`, which are consistent across every table. Customer data is anonymised: no names, phone numbers, email addresses or street addresses. All money is in **South African rand**. Datetimes are `YYYY-MM-DD hh:mm:ss`.

Table numbering follows the section numbers in your request, so section 2 is `table2_orders_and_transactions.csv` and so on.

## There is no `Returned` column — by design

Your section 4 says: *"During data preparation, a new variable called Returned can be created. Returned = 1 would mean the item was returned within the agreed return period."* So the target is yours to build, and this extract does not pre-compute it. What it gives you is the evidence:

```
Returned = 1 where the Order_Item_ID appears in table4_returns.csv, else 0
```

Two things to decide while you do that. First, the **agreed return period** — `Return_Date` runs from 1 to 29 days after delivery here, so a 30-day policy captures all of them, but a shorter policy would not and the choice is yours to state. Second, items that were **never delivered** (`Delivery_Date` blank, `Delivery_Outcome` of Failed or Returned to sender) cannot be customer returns at all. Scoring them 0 mixes "kept it" with "never received it".

## Files

### `table2_orders_and_transactions.csv`
One row per order item. Rows: 48,788

| Column | Type | Description |
|---|---|---|
| `Order_ID` | String | Unique identifier for each order. |
| `Order_Item_ID` | String | Unique identifier for each individual item within an order. **This is the grain of the prediction** — one row per order item. |
| `Customer_ID` | String | Anonymised customer identifier, consistent across tables. |
| `SKU` | String | Product identifier, links to the product catalogue. |
| `Seller_ID` | String | Seller responsible for the item. |
| `Order_Date` | Datetime | `YYYY-MM-DD hh:mm:ss`. |
| `Quantity` | Integer | Units purchased. Always greater than 0. |
| `Unit_Price` | Decimal | Actual unit price paid, **ZAR**. |
| `Listed_Price` | Decimal | Normal listed price before discount, ZAR. |
| `Discount_Amount` | Decimal | Listed price less price paid, ZAR. 0.00 where no promotion applied. |
| `Promotion_Applied` | Boolean | True where a promotion or discount was applied. |
| `Promotion_Type` | Categorical | Type of promotion. Blank where none applied. |
| `Basket_Value` | Decimal | Total order value in ZAR. **Order-level — repeats across the order's items.** |
| `Basket_Item_Count` | Integer | Total units in the order. Order-level, repeats. |
| `Payment_Method` | Categorical | Credit Card / Debit Card / EFT / Wallet / Buy Now Pay Later / Voucher. |

### `table3_product_catalogue.csv`
One row per SKU. Rows: 3,209

| Column | Type | Description |
|---|---|---|
| `SKU` | String | Product identifier matching the orders table. |
| `Product_Name` | String | Name or description. |
| `Product_Category` | Categorical | **Standardised** main category — the cleaned value. |
| `Product_Category_Raw` | String | **The original source value, kept unstandardised as you asked.** Same category appears as e.g. Clothing / clothing / Apparel / CLOTHING / Fashion. |
| `Product_Subcategory` | Categorical | More detailed classification. Blank where not supplied. |
| `Brand` | Categorical | Brand. Blank where not supplied. |
| `Size` | Categorical | Size where applicable (`S`-`XXL` for clothing, `UK 4`-`UK 12` for shoes). **Blank for categories without sizing** — an absence, not a missing value. |
| `Colour` | Categorical | Colour or variant. Blank where not applicable. |
| `Product_Attributes` | String | Other listing attributes. Blank where none. |
| `Listed_Price` | Decimal | Normal listed selling price, ZAR. |
| `Seller_ID` | String | Seller of the product. |
| `Average_Rating` | Decimal | 1.0 to 5.0. Blank where unavailable. |
| `Review_Count` | Integer | Reviews available. Blank where unavailable. |
| `Listing_Age` | Integer | Days the listing had existed. Blank where not calculable. |

### `table4_returns.csv`
One row per return. Rows: 7,657

| Column | Type | Description |
|---|---|---|
| `Return_ID` | String | Unique identifier for each return. |
| `Order_ID` | String | Links the return to the original order. |
| `Order_Item_ID` | String | **Identifies the exact item returned. This is the join key for building your target.** |
| `Customer_ID` | String | Same anonymised identifier as the orders table. |
| `SKU` | String | Product returned. |
| `Seller_ID` | String | Seller responsible. |
| `Return_Date` | Datetime | When the return was recorded, `YYYY-MM-DD hh:mm:ss`. |
| `Return_Reason_Code` | Categorical | SIZE / FIT / NOTASDESC / DAMAGED / FAULTY / WRONGITEM / LATE / CHANGEDMIND / QUALITY / COLOUR. |
| `Return_Reason` | String | Description corresponding to the code. |
| `Item_Condition` | Categorical | Unopened / Opened, unused / Used / Damaged / Incomplete. |
| `Refund_Outcome` | Categorical | Full refund / Partial refund / Exchange / Store credit / Rejected. |
| `Refund_Amount` | Decimal | Amount refunded, ZAR. 0.00 where the refund was rejected. |

### `table5_customer_purchase_and_return_history.csv`
**One row per order item** — a point-in-time snapshot. Rows: 48,643

| Column | Type | Description |
|---|---|---|
| `Customer_ID` | String | Anonymised identifier matching the orders table. |
| `Order_ID` | String | The order this snapshot was taken for. |
| `Order_Item_ID` | String | **The primary key, with Customer_ID.** One snapshot per order item. |
| `Snapshot_Date` | Datetime | Equal to that order's `Order_Date`. Every figure in the row reflects what was known at this moment and nothing after it. |
| `Customer_Tenure` | Integer | Days since the customer's first recorded order. |
| `Previous_Orders` | Integer | Orders placed **before** this one. 0 on a first order. |
| `Previous_Items_Purchased` | Integer | Items purchased before this one. |
| `Previous_Returns` | Integer | Items previously returned. |
| `Historical_Return_Rate` | Decimal | `Previous_Returns / Previous_Items_Purchased`, 0 to 1. **Blank where there is no prior purchase** — undefined, not 0. |
| `Previous_Category_Orders` | Integer | Prior purchases from this item's category. |
| `Previous_Category_Returns` | Integer | Prior returns from this item's category. |
| `Average_Order_Value` | Decimal | Historical average order value, ZAR. Blank on a first order. |
| `Days_Since_Last_Order` | Integer | Days since the previous order. Blank on a first order. |

### `table6_marketplace_seller_data.csv`
**One row per order item** — a point-in-time snapshot. Rows: 48,643

| Column | Type | Description |
|---|---|---|
| `Seller_ID` | String | Unique seller identifier. |
| `Order_ID` | String | The order this snapshot was taken for. |
| `Order_Item_ID` | String | **The primary key, with Seller_ID.** One snapshot per order item. |
| `Snapshot_Date` | Datetime | Equal to that order's `Order_Date`. |
| `Seller_Rating` | Decimal | Rating **as at the order**, 1.0 to 5.0. |
| `Seller_Total_Orders` | Integer | Completed orders handled **before** this one. |
| `Seller_Return_Count` | Integer | Returned items linked to the seller before this order. |
| `Seller_Return_Rate` | Decimal | `Seller_Return_Count / Seller_Total_Orders`, 0 to 1. Blank for a seller's first order. |
| `Average_Shipping_Time` | Decimal | Historical average shipping days. Blank for a first order. |
| `Complaint_Count` | Integer | Complaints recorded before this order. |
| `Late_Shipment_Rate` | Decimal | Historical late proportion, 0 to 1. Blank for a first order. |
| `Seller_Tenure` | Integer | Days the seller has been active at the time of the order. |

### `table7_delivery_and_logistics.csv`
One row per order item. Rows: 48,788

| Column | Type | Description |
|---|---|---|
| `Order_ID` | String | Links delivery to the original order. |
| `Order_Item_ID` | String | Item-level delivery record. |
| `Distribution_Centre` | Categorical | Fulfilling DC. |
| `Dispatch_Date` | Datetime | When the order left the DC. |
| `Promised_Delivery_Date` | Datetime | Promised delivery date or time. |
| `Delivery_Date` | Datetime | Actual delivery. **Blank where delivery failed or the parcel went back to sender.** |
| `Delivery_Region` | Categorical | General delivery area — province. No street addresses, as you asked. |
| `Delivery_Attempts` | Integer | Attempts required. |
| `Delivery_Outcome` | Categorical | Delivered / Delivered to pickup point / Failed - customer unavailable / Returned to sender. |
| `On_Time_Delivery` | Boolean | True where delivery beat the promised date. **Blank where the item was never delivered** — undefined, not False. |
| `Fulfilment_Type` | Categorical | Own Fleet / Courier Partner / Seller Fulfilled / Pickup Point. |

### `table8_clickstream_and_search.csv`
One row per product interaction within a session. Rows: 42,374

| Column | Type | Description |
|---|---|---|
| `Session_ID` | String | Unique browsing session identifier. |
| `Customer_ID` | String | Anonymised identifier **where the customer was signed in**. Blank for anonymous sessions. |
| `Session_Date` | Datetime | When the session occurred. |
| `Search_Query` | String | Search entered. Blank where the customer browsed without searching. |
| `SKU` | String | Product interacted with. |
| `Product_View` | Boolean | Whether the product was viewed. |
| `Added_To_Basket` | Boolean | Whether it was added to the basket. |
| `Purchase_Completed` | Boolean | Whether the interaction resulted in a purchase. |
| `Session_Abandoned` | Boolean | Whether the session ended without a purchase. |
| `Order_ID` | String | **The link you asked for** — the order the session led to. Blank where the session did not convert. |

## Data quality notes

This is an extract from live operational systems and has **not** been cleaned. Expect:

- Missing values in optional / conditionally-populated fields
- Inconsistent casing and stray whitespace in some free-text and categorical fields
- A small number of duplicate records
- Occasional out-of-range or implausible values from data-capture errors
- Some date fields captured in inconsistent formats

## Why tables 5 and 6 are shaped the way they are

Your section 5 says these variables *"must only use information that was known before the order being predicted"*, and section 6 says *"the value that was known at the time of the order should be used so that future information does not influence an older prediction"*.

A single current row per customer and per seller cannot satisfy that — it would carry today's return rate back onto an order from two years ago, which is exactly the leak you were guarding against. So both tables are supplied **as one snapshot per order item**, with a `Snapshot_Date` equal to that order's `Order_Date`, and every count in the row computed from events strictly before it.

That means you can join them straight onto the orders table on `Order_Item_ID` with no as-of logic and no leakage:

```
orders  ⟕  customer_history  on Order_Item_ID
        ⟕  seller_data       on Order_Item_ID
```

It also means these tables repeat a customer and a seller many times over, which is intended. Do not de-duplicate them to one row per customer or per seller — that would discard the history and keep an arbitrary snapshot.

**Blank is not zero in these tables.** `Historical_Return_Rate` is blank on a customer's first purchase because a rate over zero prior items is undefined, not 0.0. The same applies to `Average_Order_Value`, `Days_Since_Last_Order`, `Seller_Return_Rate`, `Average_Shipping_Time` and `Late_Shipment_Rate`. Imputing 0 would tell your model that a brand-new customer has a perfect return record.

## The data quality issues you flagged

- **Inconsistent product categories.** As requested, `Product_Category_Raw` keeps the original source value and `Product_Category` holds the cleaned one. The same category arrives as `Clothing`, `clothing`, `Apparel`, `CLOTHING`, `Clothing & Apparel` and `Fashion`. Standardising it is a real step, and both columns are present so you can show the before and after.
- **Missing values stay identifiable.** They are empty fields, never a default or a placeholder. Nothing has been imputed.
- **Duplicate records exist** in the orders, catalogue, returns and delivery tables at roughly 0.3%, from double-submitted records upstream. Identify them on each table's key before counting.
- **Consistent dates.** `YYYY-MM-DD hh:mm:ss` throughout, with a small number of rows arriving from a legacy system in a different format — worth catching in preparation.
- **Categorical values** carry casing and whitespace variants (`Delivered`, `DELIVERED`, ` Delivered `). Normalise before grouping.
- **Monetary values** are all ZAR, in `Unit_Price`, `Listed_Price`, `Discount_Amount`, `Basket_Value`, `Average_Order_Value` and `Refund_Amount`.

## Delivery data and prediction timing

You noted this yourself in section 7: *"Some delivery information only becomes available after an order has already been placed... it should not be used in a model that is meant to predict return risk before the order is confirmed."* That is correct and worth holding to. `Delivery_Date`, `On_Time_Delivery`, `Delivery_Attempts` and `Delivery_Outcome` are all post-order facts. They are genuinely useful for explaining *why* returns happen — late delivery is associated with a higher return rate in this data — but a model that uses them is answering a different, later question than a model scoring at checkout. Decide which question you are answering, and say so.

**`Basket_Value` and `Basket_Item_Count` are order-level** and repeat across the items of an order. Summing them across items multiplies the basket by its item count.

**Split by customer, not by order item.** `Customer_ID` recurs across orders, and a customer's own return history is one of the stronger signals here — a random row-level split will put the same customer on both sides and flatter your results.

_Synthetic data supplied for the CAP182 capstone project. Not real client data._
