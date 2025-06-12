# Shield Insurance Power BI Pilot Project

## 🛡️ Driving Data-Driven Decisions at Shield Insurance 🛡️

Welcome to the GitHub repository for the Shield Insurance Power BI Pilot Project! This project demonstrates a powerful analytics solution designed to empower Shield Insurance with actionable insights into its key performance metrics. Developed as a Proof of Concept (POC) in collaboration with AtliQ Technologies, this Power BI dashboard aims to revolutionize data-driven decision-making, ensuring Shield Insurance continues to provide reliable and comprehensive protection to its customers.

---

## 🎯 The Challenge: Unlocking Business Intelligence 🎯

Shield Insurance, renowned for its commitment to customer care and security, sought to enhance its strategic capabilities by leveraging its vast datasets. The goal was clear: transform raw data into a dynamic Power BI dashboard, providing real-time, actionable insights into crucial business performance indicators.

AtliQ Technologies was tasked with this pilot project to showcase its expertise in delivering tailored analytical solutions. This repository details the journey from data acquisition to insightful visualization, proving our capability to meet Shield Insurance's specific needs.

---

## ✨ Project Goals & Deliverables (The "ASK") ✨

This pilot project focuses on delivering a robust Power BI dashboard that provides comprehensive insights into:

### Key Performance Metrics at a Glance:
* **Total Customers:** A clear count of the customer base.
* **Total Revenue:** The overall financial performance.
* **Daily Revenue Growth (DRG):** Average daily increase in revenue.
* **Daily Customer Growth (DCG):** Average daily increase in customer count.

### Trend Analysis & Growth:
* **Month-over-Month Change %:** Percentage changes in key metrics (Total Customers, Total Revenue, DRG, DCG).
* **Customer & Revenue Trends:** Visualizations showing daily customer and revenue growth trends by month.
* **Interactive Trend Switch:** A user-friendly switch to toggle between revenue and customer trend graphs.

### Deep Dive into Customer & Sales Segments:
* **Customer Age Group Segmentation:** Breaking down customers into specific age brackets: `18-24`, `25-30`, `31-40`, `41-50`, `51-65`, and `65+`.
* **Revenue & Customer Distribution:**
    * Total revenue split by age group and city.
    * Total customers split by age group and city.
* **Dedicated Sales Mode Analysis Page:**
    * Total customers split percentage by sales mode (e.g., Offline-Agent, Online-App).
    * Total revenue split percentage by sales mode.
    * Trend of sales mode performance over months.
* **Dedicated Age Group Analysis Page:**
    * Age group vs. Expected Settlement.
    * Age group vs. Sales Mode preference.
    * Age group vs. Policy Preference.

### Dynamic Filtering for Granular Insights:
* Comprehensive filters for `Sales Mode`, `Age Group`, `City`, `Month`, and `Policy ID`.

---

## ⚙️ Technical Implementation & Data Journey ⚙️

This section details the technical aspects of the project, from data sourcing to the final analytical model.

### 📊 Data Storage & Sources:
All datasets are securely stored and consolidated on the **Code basis website platform**.

### 🛠️ Tools Utilized:
* **Microsoft Excel:** For initial data review and basic manipulations.
* **Power BI Desktop:** The core tool for data modeling, analysis, and visualization.
* **DAX Studio & DAX:** For advanced data analysis expressions and query optimization within Power BI.

### 🗃️ Dataset Overview:
The project leverages five key CSV files, each providing crucial information for a holistic view of Shield Insurance's operations:

1.  **`dim_customer.csv`**: Contains customer demographics (`customer_code`, `dob`, `city`).
2.  **`dim_date.csv`**: Provides date hierarchy information (`date`, `mmm_yy`, `day_type`, `week_no`).
3.  **`dim_policies.csv`**: Details about insurance policies (`policy_id`, `base_cover`, `base_premium_amt(INR)`).
4.  **`fact_premiums.csv`**: Records of policy sales (`date`, `customer_code`, `policy_id`, `sales_mode`, `final_premium_amt(INR)`).
5.  **`fact_settlements.csv`**: Information on policy settlements (`age`, `settlement%`).

### 🧹 Data Cleaning & Transformation Highlights:
Performed using **Microsoft Excel** and **Power Query** in Power BI:
* Removed empty columns from `fact_settlements`.
* Split the `sales_mode` column in `fact_premiums` by delimiter '-' and renamed resulting columns to `Mode: Online/Offline` and `Mode: Through Medium` for clearer categorization.
* Ensured correct data types for all columns, notably converting `Settlement %` to a numeric format.

### 🧠 Data Modeling:
A well-structured star schema was implemented to ensure efficient data retrieval and accurate calculations. The relationships between tables are designed for optimal performance and data integrity.

![Screenshot 2024-09-23 211849](https://github.com/user-attachments/assets/eeda14a6-e3c2-4f51-bf67-46a04bebad2c)
*The above image displays the data model established in Power BI, showcasing the relationships between the various dimension and fact tables.*

### ➕ Custom Columns & Measures (DAX Power!):

Several calculated columns and robust DAX measures were created to derive the required insights:

#### New Calculated Columns:
* **`dim_customer` Table:**
    * `Age` = `2023 - YEAR(dim_customer[dob].[Date])`
    * `AgeGroup` = `SWITCH(TRUE(), dim_customer[Age] >= 18 && dim_customer[Age] <= 24, "18-24", ...)` (Categorizes customers into specified age ranges)
    * `SettlementINDecimal` = `RELATED(fact_settlements[settlement %])` (Retrieves settlement percentage for each customer)
* **`dim_date` Table:**
    * `Month` = `FORMAT(dim_date[date].[Date], "MMM")`
    * `SortedBy` = `SWITCH(TRUE(), dim_date[Month]="Nov", 1, ...)` (Custom sort order for months for consistent trend visualization)

#### Key DAX Measures:
* **`Total Customers`** = `COUNT(dim_customer[customer_code])`
* **`Total Revenue`** = `SUM(fact_premiums[final_premium_amt(INR)])`
* **`DRG (Daily Revenue Growth)`** = `VAR _tot_rev = [Total Revenue] VAR _date = DISTINCTCOUNT(dim_date[date]) RETURN DIVIDE(_tot_rev, _date, 0)`
* **`DCG (Daily Customer Growth)`** = `VAR _tot_cus = [Total Customers] VAR _date = DISTINCTCOUNT(dim_date[date]) RETURN DIVIDE(_tot_cus, _date, 0)`
* **`LM Customers`**, **`LM Revenue`**, **`LM DCG`**: Measures to calculate values for the Last Month, enabling month-over-month comparisons.
* **`%CustomerChange`**, **`%DRGChange`**, **`%DCGChange`**, **`%RevenueChange`**: Percentage change calculations for month-over-month growth.
* **`Cus%`** & **`Rev%`**: Percentage of total customers and revenue for various segments.
* **`Expected Settlement`** = `ROUND(SUMX(fact_premiums, fact_premiums[final_premium_amt(INR)]*(1+RELATED(dim_customer[SettlementINDecimal]))), 0)` (Calculates the expected settlement amount based on premium and settlement percentage).

---

## 📈 Key Insights & Strategic Recommendations (The "ACT") 📈

The Power BI dashboard revealed several compelling insights, forming the basis for strategic recommendations:

### Data-Backed Insights:
* **Strong Customer Base & Revenue:** Shield Insurance proudly serves **26,841 customers** and has generated a substantial **989.25 million** in total revenue.
* **Delhi: A Powerhouse Region:** **Delhi** emerges as the top-performing city, contributing **11,007 customers** and **401.57 million** in revenue.
* **The Dominant 31-40 Age Group:** This age segment leads with **11,171 customers** and the highest revenue contribution of **343.76 million**, highlighting a strong appeal to younger and middle-aged demographics.
* **Volatile Monthly Trends:**
    * **March 2023** showcased remarkable growth with an **85% increase in revenue** and an **82% rise in customer numbers**.
    * However, **April 2023** experienced a significant downturn, with revenue plummeting by **41.73%** and customers by **41.41%**. This sharp fluctuation requires immediate attention.
* **Offline Agents Remain Key:** The **offline agent channel** is the primary business driver, accounting for **55.41% of customers** and **55.67% of total revenue**.
* **Balanced Revenue Across Sales Modes:** While offline agents lead, revenue distribution across all sales modes is relatively balanced, ranging from **12.60% to 16.27%**.
* **Most Popular Policy by Customers:** **"POL4321HEL"** is the most preferred policy, chosen by **4,434 customers**.
* **Highest Revenue-Generating Policy:** **"POL2005HEL"** stands out for its revenue generation, amounting to **324.26 million**.

### 💡 Recommendations: Strategic Actions for Shield Insurance 💡

Based on the comprehensive analysis of the Power BI dashboard, the following strategic recommendations are put forth to drive sustained growth, optimize operations, and enhance customer satisfaction for Shield Insurance:

1.  **Customer Growth & Stability:**
    * **Implement Predictive Analytics:** Utilize advanced predictive analytics to proactively manage and mitigate revenue fluctuations. This will allow for more stable financial performance.
    * **Target Key Demographics:** Develop targeted marketing campaigns and product offerings specifically for younger (18-24) and older (65+) demographics to unlock new growth opportunities and expand the customer base.

2.  **Age Group Focus: Strengthen Retention:**
    * **Tailored Offerings for High-Revenue Group:** Intensify customer retention efforts for the high-revenue-generating **31-40 age group**. This includes developing more tailored products, personalized communication, and loyalty programs that resonate with their specific needs and preferences.

3.  **Geographical Strategy: Replicate Success:**
    * **Expand Successful Strategies:** Analyze the successful strategies employed in **Delhi NCR** (your top-performing region) and replicate these best practices in other key regions, such as **Mumbai** and **Chennai**, to stimulate growth and market penetration.

4.  **Sales Channel Optimization: Enhance Digital Engagement:**
    * **Invest in Digital Platforms:** Enhance and optimize digital sales platforms (online app, website) to better engage younger customers who show a strong preference for online channels. Improve user experience, streamline the digital purchasing process, and increase online visibility.

5.  **Policy & Risk Management: Tailored for Settlement Groups:**
    * **Develop Specific Policies:** Create and promote tailored insurance policies for age groups identified with high settlement rates, specifically **31-40, 25-30, and 41-50**. By offering policies that address their unique risk profiles, Shield Insurance can improve overall risk management and reduce potential liabilities.

6.  **Product & Channel Strategy: Customized Offerings:**
    * **Age-Group Specific Customization:** Customize product offerings based on specific age group preferences.
    * **Multi-Channel Engagement:** Ensure a seamless and consistent customer experience across all sales channels (offline agents, online app, website) to foster multi-channel engagement and maximize customer satisfaction and retention.

---

## 🚀 Experience the Live Dashboard! 🚀

Dive into the interactive Power BI dashboard yourself to explore the data and insights firsthand!

🔗 [**Live Dashboard Link**](https://app.powerbi.com/view?r=eyJrIjoiMDdjNzI3ODUtZTJiYS00YmRhLWJhYjEtNTUxYzhmMGFkNzU4IiwidCI6ImM2ZTU0OWIzLTVmNDAzMi1hYWU5LWQ0MjQ0ZGM1YjJjNCJ9)

---

## 🙏 Thank You! 🙏

Thank you for exploring the Shield Insurance Power BI Pilot Project. This initiative exemplifies how data analytics can drive informed decisions and foster continuous growth for businesses like Shield Insurance.

Feel free to reach out with any questions or feedback.

---
