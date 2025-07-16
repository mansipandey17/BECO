#  EcoCommerce Analytics Dashboard – Unlocking Green Growth for Beco

##  Project Overview

This end-to-end analytics project simulates a real-world data scenario for **Beco**, a sustainable D2C brand. Using SQL, Power BI, Excel, and Python, I analyzed dummy e-commerce data to extract actionable insights, track KPIs, and quantify environmental impact.

 **Goal:** Blend data analytics with sustainability insights to empower smarter, greener business decisions.

---

##  Dataset Description

A comprehensive dummy dataset (~1000 rows per table) created to reflect Beco’s business model, containing 7 interconnected tables:

| Table Name     | Description                                                               |
|----------------|---------------------------------------------------------------------------|
| `orders`       | Order-level data: date, amount, promo, delivery partner, fulfillment time |
| `order_items`  | Product breakdown per order: quantity, price, category                    |
| `products`     | SKU details: eco type (bamboo, biodegradable), MRP, rating                |
| `customers`    | Customer profile: city, gender, loyalty tier, join date                   |
| `eco_impact`   | CO₂ saved, plastic avoided, water saved per product                       |
| `inventory`    | Stock levels by warehouse and date                                        |
| `feedback`     | Customer reviews, ratings, and sentiment notes                            |

📎 **Download the dataset:**  
[Beco_EcoCommerce_Dataset.xlsx](https://github.com/user-attachments/files/21249586/Beco_EcoCommerce_Dataset.xlsx)

---

## SQL Insights Extracted

SQL queries were written in **MySQL**, covering business KPIs, customer trends, and environmental metrics.

---

### 🔹 1. Total Revenue, Orders, and AOV

```sql
SELECT COUNT(DISTINCT order_id) AS total_orders,ROUND(SUM(total_amount), 2) AS total_revenue,ROUND(AVG(total_amount), 2) AS avg_order_value
FROM orders;

---

 2. Top 5 Cities by Revenue
SELECT c.city,ROUND(SUM(o.total_amount), 2) AS total_revenue
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
GROUP BY c.city
ORDER BY total_revenue DESC
LIMIT 5;
 Reveals top-performing cities for geo-targeted strategies.

3. Top-Selling Product Categories
SELECT category,SUM(quantity) AS total_units_sold
FROM order_items
GROUP BY category
ORDER BY total_units_sold DESC;
Identifies best-selling categories (e.g., Bathroom, Kitchen, Cleaning).

4. Products with Highest CO₂ Savings
SELECT p.product_name,e.co2_saved_kg
FROM eco_impact e
JOIN products p ON e.product_id = p.product_id
ORDER BY e.co2_saved_kg DESC
LIMIT 5;
 Highlights the most sustainable products — great for ESG reports.

 5.New vs. Returning Customers
SELECT CASE WHEN order_count = 1 THEN 'New Customer'ELSE 'Returning Customer'END AS customer_type,COUNT(*) AS num_customers
FROM (SELECT customer_id, COUNT(order_id) AS order_countFROM ordersGROUP BY customer_id) AS customer_orders
GROUP BY customer_type;
 Helps in understanding customer loyalty and retention rates.

 6.Avg. Fulfillment Time by Delivery Partner
SELECT delivery_partner,ROUND(AVG(fulfillment_time), 1) AS avg_fulfillment_hours
FROM orders
GROUP BY delivery_partner
ORDER BY avg_fulfillment_hours;
 Evaluates delivery partner efficiency.

 7. Average Rating by Product Category
SELECT p.category,ROUND(AVG(f.rating), 2) AS avg_rating
FROM feedback f
JOIN products p ON f.product_id = p.product_id
GROUP BY p.category
ORDER BY avg_rating DESC;
 Measures satisfaction across product types.
