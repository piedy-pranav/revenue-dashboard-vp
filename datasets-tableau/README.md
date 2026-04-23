# Tableau Public Dashboard — VP of Revenue eCommerce Overview

**Goal:** Build a 4-chart dashboard for the VP of Revenue showing:
1. Sales by Month (revenue trend)
2. Order Volume by Month (order count trend)
3. Top 10 Most Frequently Purchased Products
4. % of Orders from Sao Paulo Customers Each Month

**Files used (all in this folder):**

| File | Key columns |
|---|---|
| `Copy of orders_rev_df.csv` | `order_id`, `customer_id`, `order_purchase_timestamp` |
| `Copy of df_Customers.csv` | `customer_id`, `customer_city` |
| `Copy of df_OrderItems.csv` | `order_id`, `product_id`, `price` |
| `Copy of df_Products.csv` | `product_id`, `product_category_name` |
| `Copy of df_Payments.csv` | `order_id`, `payment_value` |

---

## Part 1 — Connect & Relate the Data

1. Open **Tableau Public** (desktop app). On the start screen click **"Text file"** under Connect.
2. Navigate to this `datasets-tableau` folder and open **`Copy of orders_rev_df.csv`**. This becomes your primary (anchor) table.
3. You are now on the **Data Source** tab. You will see the table canvas in the center.
4. In the **left panel** under "Files", click **"Add"** (or drag) to add a second file. Select **`Copy of df_Customers.csv`**. Tableau will ask how to relate it — a noodle (relationship line) appears.
   - In the relationship dialog: set **`Customer Id`** (orders_rev_df) = **`Customer Id`** (df_Customers). Click OK.
5. Repeat for **`Copy of df_OrderItems.csv`**:
   - Relate on **`Order Id`** (orders_rev_df) = **`Order Id`** (df_OrderItems). Click OK.
6. Repeat for **`Copy of df_Payments.csv`**:
   - Relate on **`Order Id`** (orders_rev_df) = **`Order Id`** (df_Payments). Click OK.
7. Repeat for **`Copy of df_Products.csv`**:
   - Relate on **`Product Id`** (df_OrderItems) = **`Product Id`** (df_Products). Click OK.
8. Your data model should now show 5 tables connected by relationship lines.
9. **Fix date field:** Click the column header **`Order Purchase Timestamp`** in the preview grid at the bottom. If the data type icon shows `Abc` (text), click it and change to **Date & Time**. Confirm when prompted.
10. Click the **Sheet 1** tab at the bottom to start building.

> **Tip:** If Tableau auto-creates joins instead of relationships, undo (Ctrl+Z) and drag the table onto the canvas area (not onto an existing table box) to use the logical/relationship model instead.

---

## Part 2 — Sheet 1: Sales by Month

1. In the bottom tab bar, double-click **"Sheet 1"** and rename it **Sales by Month**.
2. In the **Data pane** (left side), find **`Order Purchase Timestamp`** under the orders_rev_df table. Drag it to the **Columns** shelf.
   - The field shows as `YEAR(Order Purchase Timestamp)`. Click the dropdown arrow on it → select **Month** (the continuous option that shows "May 2016", not just "May"). If you see two "Month" options, pick the lower one that shows full dates like "May 2016".
3. From the **df_Payments** table in the Data pane, drag **`Payment Value`** to the **Rows** shelf.
   - It auto-aggregates as `SUM(Payment Value)`. That is correct.
4. You now have a line chart. If it shows as bars, click **"Show Me"** (top right) and select the **line chart** icon.
5. Right-click the Y-axis → **Format** → under Numbers choose **Currency (Custom)**, Symbol `$`, Decimal places `0`.
6. Right-click on the chart title ("Sheet 1") → **Edit Title** → type `Monthly Revenue`.
7. Drag **`SUM(Payment Value)`** from the Rows shelf (or from Data pane) onto **Label** in the Marks card to show values on the line. (Optional but useful for a VP view.)

---

## Part 3 — Sheet 2: Order Volume by Month

1. Click the **"New Worksheet"** icon (the icon with a `+` and a chart, bottom tab bar). Rename it **Order Volume by Month**.
2. Drag **`Order Purchase Timestamp`** to the **Columns** shelf → set to **Month** (continuous, same as Part 2 step 2).
3. Drag **`Order Id`** (from orders_rev_df) to the **Rows** shelf.
   - By default it aggregates as `COUNT(Order Id)`. Click the dropdown on the pill → **Measure** → **Count (Distinct)** to count unique orders.
4. You should now have a line chart of unique orders per month. If not, use **Show Me** to select the line chart.
5. Right-click Y-axis → **Format** → Numbers → **Number (Custom)**, decimal places `0`.
6. Edit chart title → `Monthly Order Volume`.

---

## Part 4 — Sheet 3: Top 10 Products by Frequency

1. Create a new worksheet. Rename it **Top 10 Products**.
2. From the **df_Products** table, drag **`Product Category Name`** to the **Rows** shelf.
3. From the **df_OrderItems** table, drag **`Order Id`** to the **Columns** shelf → aggregate as **Count (Distinct)** (click the pill dropdown → Measure → Count Distinct).
4. Click the **Sort Descending** button in the toolbar (the `ZA↓` icon) to sort by count descending.
5. Now filter to top 10: In the **Data pane**, right-click **`Product Category Name`** → **Filter**.
   - Go to the **Top** tab → select **By field: Top 10 by Count Distinct of Order Id**. Click OK.
6. Change the chart type: click **Show Me** → select **horizontal bar chart**.
7. Drag **`CNTD(Order Id)`** from the Columns shelf onto **Color** in the Marks card (optional, makes bars easier to read).
8. Edit chart title → `Top 10 Products by Order Frequency`.
9. Right-click the X-axis → **Format** → Numbers → Number (Custom), decimal 0.

---

## Part 5 — Sheet 4: % Orders from Sao Paulo by Month

This chart requires a **Calculated Field**.

1. Create a new worksheet. Rename it **% Sao Paulo Orders by Month**.
2. Create a calculated field: In the Data pane, click the dropdown at the top (next to the search box) → **Create Calculated Field** (or right-click any blank area in the Data pane).
3. Name it **`Sao Paulo Order %`**. Enter this formula:

   ```
   COUNTD(IF LOWER([Customer City]) = "sao paulo" THEN [Order Id] END)
   /
   COUNTD([Order Id])
   * 100
   ```

   Click **OK**. The field appears in the Data pane under Measures.

4. Drag **`Order Purchase Timestamp`** to the **Columns** shelf → set to **Month** (continuous).
5. Drag your new **`Sao Paulo Order %`** field to the **Rows** shelf.
   - It should show `AGG(Sao Paulo Order %)`. That is correct.
6. Set chart type to line chart (use Show Me if needed).
7. Right-click the Y-axis → **Format** → Numbers → **Percentage**, decimal places `1`. *(Note: since the field is already multiplied by 100, format as Number with 1 decimal instead, and add `%` suffix manually in a label if needed. Alternatively remove the `* 100` from the formula and format as Percentage.)*
8. Edit chart title → `% of Orders from Sao Paulo by Month`.

---

## Part 6 — Assemble the Dashboard

1. Click the **"New Dashboard"** icon (the icon with a grid and `+`, bottom tab bar). Rename it **VP Revenue Overview**.
2. On the left panel, under **Size**, choose **Automatic** (so it fits any screen) or set a fixed size like **1200 x 900 px**.
3. Drag sheets onto the dashboard canvas in this recommended layout:

   ```
   ┌─────────────────────┬─────────────────────┐
   │   Sales by Month    │  Order Volume/Month  │
   │    (top left)       │    (top right)       │
   ├─────────────────────┼─────────────────────┤
   │  Top 10 Products    │  % Sao Paulo/Month   │
   │  (bottom left)      │   (bottom right)     │
   └─────────────────────┴─────────────────────┘
   ```

   Drag **"Sales by Month"** sheet to the top-left quadrant. Drag **"Order Volume by Month"** to the top-right. Drag **"Top 10 Products"** to the bottom-left. Drag **"% Sao Paulo Orders by Month"** to the bottom-right.

4. **Add a title:** From the left panel, drag a **"Text"** object to the very top of the dashboard. Type: `eCommerce Revenue Overview — VP Dashboard`.
5. **Add a date filter (optional but recommended):** Click one of the line charts → click the dropdown arrow on the sheet title in the dashboard → **Filters** → **Order Purchase Timestamp**. In the filter card that appears on the dashboard, click its dropdown → **Apply to Worksheets** → **All Using This Data Source**. This makes one date filter control all charts.
6. Clean up: click on each sheet in the dashboard and uncheck **"Show sheet title"** from the Layout pane if the chart already has a descriptive title from the axis/header.

---

## Part 7 — Publish to Tableau Public

1. Go to **File** → **Save to Tableau Public As...**
2. Sign in with your Tableau Public account (free account at public.tableau.com).
3. Name the workbook `UCLA Industry Seminar — eCommerce VP Dashboard`.
4. Click **Save**. Tableau will open the published dashboard in your browser.
5. Copy the public URL from your browser and share it here for review.

---

## Troubleshooting

| Issue | Fix |
|---|---|
| Dates show as numbers (e.g. `20180620`) | Go to Data Source tab, change column type to Date |
| Relationship error / null values | Verify join keys match exactly (`order_id` vs `Order Id`). Tableau auto-capitalizes. |
| Sao Paulo % shows 0 | Open calculated field and check: try `"sao paulo"` vs `"são paulo"` — use `LOWER()` to normalize |
| Chart shows `Abc` instead of data | Check the field is dragged to the correct shelf (Rows/Columns vs Marks) |
| Top 10 filter missing category names | The Products table must be connected via OrderItems → Products relationship |
