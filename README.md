# **Apple Store – Sales & Category Performance Analysis**


## Project Background

( This is my first port folio piece, written on October 18th 2025 )
<br><br>

The Apple Store Analytics Project analyzes sales data across multiple locations to uncover which products, categories, and regions generate the highest revenue. By consolidating transactional, product, and regional datasets, the project aims to deliver clear, actionable insights into business performance and profitability.

This project uses four integrated datasets / tables ( synthetic data downloaded from Kaggle ) :
- sales (for revenue, quantity)
- products (to identify product name and category)
- category (to group and rank by category)
- stores (optional, for regional insights)
  
<br><br>
**Insights and Recommendations Focus**

- **Top Revenue Drivers**    : Identify the products and categories responsible for the largest share of sales.
- **Pricing and Margins**    : Evaluate the average selling price (ASP) to understand pricing trends.
- **Regional Performance**   : Compare category and product success across store locations.
  <br><br>

**Downloadable / external sources**
- The 4 cleaned csv files.
- The Master table.
- Tableau file.
  <br><br>

**Data Cleaning Note**
Using excel : 
- All headers are fixed to lower case.
- All date field are converted from dd/mm/yyyy to yyyy-mm-dd ( for use on Google's Big Query ).
- No other cleaning is needed, the data is clean , maybe because it is a synthetic piece made for port folio use.
<br><br>

Once the files are ready, they are brought to Google Big Query.

The first thing to do after the tables are made, I did Integrity check, to make sure there is no missing link / foreign key between each main table.

<pre style="font-size:13px; line-height:1.25;"><code>
-- 1. Products missing in products.csv
SELECT DISTINCT sales.product_id
FROM dev-smoke-471022-i9.apple_store_data.sales AS sales
LEFT JOIN dev-smoke-471022-i9.apple_store_data.product AS product
  ON sales.product_id = product.product_id
WHERE product.product_id IS NULL;

-- 2. Stores missing in stores.csv
SELECT DISTINCT sales.store_id AS missing_store_id
FROM dev-smoke-471022-i9.apple_store_data.sales AS sales
LEFT JOIN dev-smoke-471022-i9.apple_store_data.stores AS stores
  ON sales.store_id = stores.store_id
WHERE stores.store_id IS NULL;

-- 3. Categories missing in categories.csv
SELECT DISTINCT product.category_id AS missing_category_id
FROM dev-smoke-471022-i9.apple_store_data.product AS product
LEFT JOIN dev-smoke-471022-i9.apple_store_data.category AS category
  ON product.category_id = category.category_id
WHERE category.category_id IS NULL;
</code></pre>

All of these did not return anything, that means integrity check is successful.
<br><br>

In order for me to understand total revenue from what, and from where, I need to create a master table from every table available and join them together. In order for me to do that, I have to join them one by one to simplify the SQL query. Here is the plan :

<img width="1256" height="407" alt="tables" src="https://github.com/user-attachments/assets/a0d9e5c5-018a-43dc-8b1e-1175f4fd4e1d" />
<br><br>
<br><br>

- **Table Join 1** - an inner join of both **category** and **product** table.
<pre style="font-size:13px; line-height:1.25;"><code>
CREATE TABLE `dev-smoke-471022-i9.apple_store_data.product_info` AS
SELECT 
    product.product_id, 
    product.product_name,
    product.launch_date, 
    product.price, 
    category.category_name 
FROM `dev-smoke-471022-i9.apple_store_data.product` AS product
INNER JOIN `dev-smoke-471022-i9.apple_store_data.category` AS category 
    ON product.category_id = category.category_id;
</code></pre>

<br><br>
- **Table Join 2** - an inner join with **sales** table.
<pre style="font-size:13px; line-height:1.25;"><code>
CREATE TABLE `dev-smoke-471022-i9.apple_store_data.product_sales` AS
SELECT 
    product_info.product_id, 
    product_info.product_name, 
    product_info.launch_date, 
    product_info.price, 
    product_info.category_name, 
    sales.sale_id, 
    sales.sale_date, 
    sales.store_id, 
    sales.quantity
FROM `dev-smoke-471022-i9.apple_store_data.product_info` AS product_info
INNER JOIN `dev-smoke-471022-i9.apple_store_data.sales` AS sales 
    ON product_info.product_id = sales.product_id;
</code></pre>

<br><br>
- **Table Join 3** - an inner join with **stores** table.
<pre style="font-size:13px; line-height:1.25;"><code>
CREATE TABLE `dev-smoke-471022-i9.apple_store_data.product_final` AS
SELECT 
    product_sales.product_id, 
    product_sales.product_name, 
    product_sales.launch_date, 
    product_sales.price, 
    product_sales.category_name, 
    product_sales.sale_id, 
    product_sales.sale_date, 
    product_sales.quantity,
    product_sales.store_id, 
    stores.store_name,
    stores.city,
    stores.country
FROM `dev-smoke-471022-i9.apple_store_data.product_sales` AS product_sales
INNER JOIN `dev-smoke-471022-i9.apple_store_data.stores` AS stores 
    ON product_sales.store_id = stores.store_id;
</code></pre>

<br><br>
Join result : the **product_final** table

<img width="220" height="366" alt="table - product_final" src="https://github.com/user-attachments/assets/dc268b63-010c-48c2-98c4-de89dfa499c1" />

<br><br>

**NOTE :** Cutoff date for the data is 2024-11-12. The reason the sales in 2024 is lower compared to the previous year, is because the sales data shown is just for 11 months instead of 12.

Once the **product_final** table are completely joined, I brought that to Tableau for visualization. You can download the tableau file HERE.
<br><br>

## 01 - Revenue by Category

<img width="2010" height="1119" alt="01 - Revenue by Category" src="https://github.com/user-attachments/assets/b9b8f1d0-4a3f-4f11-88e2-5df0a17189be" />
<br><br>

**Overall Observation**

The chart clearly shows **uneven revenue distribution** across product categories. Some categories are contributing disproportionately more to total revenue, highlighting where the company’s core business lies and where growth opportunities may exist.

**Top Revenue Drivers**

- **Tablet, Accessories, and Smartphone** are the **top three revenue generators**, for total revenue of 2.75 billion between 2020 and 2024.
- **Desktop, Wearable , Smartphone** and **Audio** products are **the mid performer**, for total revenue of 2.76 billion, which is 44.8 percent of total revenue between 2020 and 2024 , on par with the top revenue generators.
- **Smart Speaker, Streaming Device, Subscription Service** products are the **lowest performer**, for total revenue of 0.66 billion, which is just 10.6 percent of total revenue between 2020 and 2024.
<br><br>

## 02 - Revenue vs ASP by Category

<img width="1874" height="1139" alt="02 - Revenue VS ASP by Category " src="https://github.com/user-attachments/assets/9f16f80c-3e45-4b66-8497-a1730e4b8e5d" />

**Overall Observation**

Premium categories such as **Tablets, Smartphones, and Accessories** drive the highest revenue despite relatively lower sales volumes, confirming a **value-driven portfolio** rather than a volume-driven one.

The visualization shows that **high average selling prices (ASP)**, not quantity sold, are the main revenue lever — while mid-tier categories like **Audio and Laptops** sustain stable contributions through balanced pricing and volume.

Lower-revenue categories such as **Smart Speakers** and **Streaming Devices** highlight potential areas for product repositioning or marketing focus to improve portfolio balance.
<br><br>

## 03 - Revenue by Store / Region

<img width="1875" height="1141" alt="03 - Revenue by Country" src="https://github.com/user-attachments/assets/97f91d2d-30eb-489f-8bd1-17f6deebb416" />

**Overall Observation**

The chart reveals a **highly uneven global revenue distribution**, with the **United States** far ahead of all other markets.

It contributes **over $1.2 B**, roughly double the next group of high-performing countries.

**Australia, China,** and **Japan** follow as secondary leaders, each generating **$500 M–$700** M, forming a strong upper tier of mature, high-income markets.

A **mid-tier cluster**—including **Canada, UAE, UK, France, Germany**, and **Mexico**—shows **moderate but stable revenue** between **$250 M–$450 M**, indicating consistent sales and established presence.

At the lower end, **Spain, Austria, Netherlands, Taiwan, Colombia, Italy, South Korea, Thailand,** and **Singapore** record less than $200 M each, highlighting **smaller or developing markets** with limited revenue contribution so far.




