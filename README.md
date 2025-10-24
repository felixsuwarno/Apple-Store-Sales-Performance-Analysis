**Apple Store – Sales & Category Performance Analysis**


## Project Background

( written on October 18th 2025 )
<br><br>

The Apple Store Analytics Project analyzes sales data across multiple locations to uncover which products, categories, and regions generate the highest revenue. By consolidating transactional, product, and regional datasets, the project aims to deliver clear, actionable insights into business performance and profitability.

This project uses four integrated datasets / tables :
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
- [🧾 4_Cleaned_CSV_tables.zip](https://github.com/user-attachments/files/23109988/4_Cleaned_CSV_tables.zip)  
- [🗃 master_table.zip](https://github.com/user-attachments/files/23109993/master_table.zip)  
- [💾 SQL Codes – Apple Store Project.zip](https://github.com/user-attachments/files/23110005/SQL.Codes.-.Apple.Store.Project.zip)  
- [📊 tableau_file.zip](https://github.com/user-attachments/files/23110008/tableau_file.zip)  
<br>
  
**Data Cleaning Note**
Using excel : 
- All headers are fixed to lower case.
- All date field are converted from dd/mm/yyyy to yyyy-mm-dd ( for use on Google's Big Query ).
- No other cleaning is needed, the data is clean , maybe because it is a synthetic piece made for port folio use.
<br><br>

Once the files are ready, they are brought to Google Big Query.

In order for me to understand total revenue from what, and from where, I need to create a master table from every table available and join them together. In order for me to do that, I have to join them one by one to simplify the SQL query. Here is the plan :

<img width="1256" height="407" alt="tables" src="https://github.com/user-attachments/assets/a0d9e5c5-018a-43dc-8b1e-1175f4fd4e1d" />
<br><br>
<br><br>

Join result : the **product_final** table ( this the "master table" in downloadables section )

<img width="220" height="366" alt="table - product_final" src="https://github.com/user-attachments/assets/dc268b63-010c-48c2-98c4-de89dfa499c1" />

<br><br>

**NOTE :** Cutoff date for the data is 2024-11-12. The reason the sales in 2024 is lower compared to the previous year, is because the sales data shown is just for 11 months instead of 12.

Once the **product_final** table are completely joined, I brought that to Tableau for visualization. 
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
<br><br>

**Overall Observation**

Premium categories such as **Tablets, Smartphones, and Accessories** drive the highest revenue despite relatively lower sales volumes, confirming a **value-driven portfolio** rather than a volume-driven one.

The visualization shows that **high average selling prices (ASP)**, not quantity sold, are the main revenue lever — while mid-tier categories like **Audio and Laptops** sustain stable contributions through balanced pricing and volume.

Lower-revenue categories such as **Smart Speakers** and **Streaming Devices** highlight potential areas for product repositioning or marketing focus to improve portfolio balance.
<br><br>

## 03 - Revenue by Store / Region

<img width="1875" height="1141" alt="03 - Revenue by Country" src="https://github.com/user-attachments/assets/97f91d2d-30eb-489f-8bd1-17f6deebb416" />
<br><br>

**Overall Observation**

The chart reveals a **highly uneven global revenue distribution**, with the **United States** far ahead of all other markets.

It contributes **over $1.2 B**, roughly double the next group of high-performing countries.

**Australia, China,** and **Japan** follow as secondary leaders, each generating **$500 M–$700** M, forming a strong upper tier of mature, high-income markets.

A **mid-tier cluster**—including **Canada, UAE, UK, France, Germany**, and **Mexico**—shows **moderate but stable revenue** between **$250 M–$450 M**, indicating consistent sales and established presence.

At the lower end, **Spain, Austria, Netherlands, Taiwan, Colombia, Italy, South Korea, Thailand,** and **Singapore** record less than $200 M each, highlighting **smaller or developing markets** with limited revenue contribution so far.
<br><br>

## 04 - Revenue and Quantity Trend by Year

<img width="1873" height="1139" alt="04 - Revenue and Quantity Trend by Year" src="https://github.com/user-attachments/assets/d008dce0-9802-4eb8-8c3d-2003be8897d6" />
<br><br>

**Overall Observation**

Revenue and quantity moved in parallel throughout 2020–2024, showing **stable pricing and consistent monthly demand** with **no strong seasonality**.
The sharp 2024 quantity drop reflects **partial-year data (cutoff: Nov 11 2024)** rather than an actual decline.
Overall performance is **steady but flat**, indicating a **mature, volume-driven market** where future growth will depend on **innovation, new product lines, or market expansion**.

**Key Insights:**
- Revenue and quantity remain tightly correlated → pricing stability confirmed.
- Minimal seasonality → demand evenly distributed year-round.
- 2024 dip due to incomplete data, not market weakness.
- Long-term stability suggests the need for **new growth levers** to prevent stagnation.
<br><br>

## Final Recommendations


### **For Top Performers – 44.5 % of revenue**

**Goal:** Defend this position, try to maximize profitability.
- These are the company’s financial backbone — losing share here would hit revenue hard.
- Focus on margin defense and brand strength, not heavy acquisition spend.
  
**Action:**
- Maintain premium pricing and supply reliability.
- Direct marketing budget mainly toward loyalty, retention, and ecosystem continuity, not awareness.
<br><br>

### **For Mid Performers – 44.8 % of revenue**

**Goal:** Optimize, and leverage scale to lift profit.
- These categories already rival top performers in total revenue but likely have tighter margins or stagnant growth.
- Opportunity lies in upselling or version differentiation (e.g., “Pro” editions, design refreshes).
- Marketing here yields high returns because infrastructure already exists.
  
**Action:**
- Prioritize optimization marketing (targeted ads, bundles, seasonal pushes).
- Invest in data-driven promotions, not new product launches.
<br><br>

### **For Low Performers – 10.6 % of revenue**

**Goal:** Re-evaluate, decide whether to fix or phase out.
- These three categories (Smart Speaker, Streaming Device, Subscription Service) deliver only one-tenth of total revenue.
- First step: review profit margins — if they’re slim or negative, consider cutting SKUs or repositioning instead of spending more.
- If margins are healthy but volume is low, test digital-only marketing, niche bundles, or partnerships (e.g., bundle smart speakers with premium audio products).

**Action:**
- Pilot one focused improvement experiment, not broad spending.
- If ROI < benchmark, reallocate budget to mid-tier products.
<br><br>

### **Business Actions based on geography**

1. **Defend Core Markets** (Top Tier)
	- Preserve leadership in the United States, Australia, China, and Japan through premium positioning, customer retention, and supply stability.
	- Focus on profit protection rather than volume growth in these mature markets.
2. **Expand Strategic Growth** (Mid Tier)
	- Increase share in mid-performing markets through localized campaigns, pricing adjustments, and channel optimization.
	- These regions offer the best balance between scalability and marketing efficiency.
3. **Rationalize or Test** (Low Tier)
	- Perform profitability and ROI assessments for smaller markets.
	- Where margins are thin, consider digital-only approaches or strategic partnerships to minimize cost exposure before deeper expansion.
---
In summary:
The company’s profit growth potential lies in deepening mid-tier market penetration, while maintaining margin discipline in top markets and experimenting selectively in low-revenue regions.





