# PBI - Big Data Analytics @Kimia Farma
Analysis of Kimia Farma's Business Performance (2020-2023)
## Challenge
Final Project as a Big Data Analytics Intern is to evaluate Kimia Farma's business performance within 2020-2023 periods. Below is the challenge steps to finish the projects:
### 1. Importing Dataset to BigQuery
Dataset consists the following tables:
- kf_final_transaction
- kf_inventory
- kf_kantor_cabang
- kf_product
  
### 2. Creating Analytical Table
Here we create analytical table though Joining pre-provided tables into a new analytical table in oder to making dashboard reports.
The columns contatined in the analytical table are:
- transaction_id
- date
- branch_id
- branch_name
- kota
- provinsi
- rating_cabang
- customer_name
- product_id
- product_name
- actual_price
- discount_percentage
- persentase_gross_laba
- nett_sales
- nett_profit
- rating_transaksi

<details>
  <summary> Clink to View Query </summary>
  
```sql
CREATE TABLE kimia_farma.kf_analytics AS(
SELECT
  t.transaction_id,
  t.date,
  t.branch_id,
  k.branch_name,
  k.kota,
  k.provinsi,
  k.rating as rating_cabang,
  t.customer_name,
  p.product_id,
  p.product_name,
  p.price as actual_price,
  t.discount_percentage,
  t.price * (1-t.discount_percentage) as nett_sales,
  CASE
    WHEN p.price <= 50000 THEN "10%"
    WHEN p.price BETWEEN 50001 and 100000 THEN "15%"
    WHEN p.price BETWEEN 100001 and 300000 THEN "20%"
    WHEN p.price BETWEEN 300001 and 500000 THEN "25%"
    ELSE "30%"
  END as persentase_gross_laba,
  t.price * (1-t.discount_percentage) *
    CASE
      WHEN p.price <= 50000 THEN 0.1
      WHEN p.price BETWEEN 50001 and 100000 THEN 0.15
      WHEN p.price BETWEEN 100001 and 300000 THEN 0.2
      WHEN p.price BETWEEN 300001 and 500000 THEN 0.25
      ELSE 0.3
    END as nett_profit,
  t.rating as rating_transaksi

FROM kimia_farma.kf_final_transaction as t
LEFT JOIN kimia_farma.kf_kantor_cabang as k
  ON t.branch_id = k.branch_id
LEFT JOIN kimia_farma.kf_product as p
  ON t.product_id = p.product_id
ORDER BY t.date desc, k.provinsi, k.kota, t.customer_name
);
```
</details>


### 3. Creating Report
Dashboard report created in the Looker Studio
<details>
  <summary>Clink to View Link </summary>
  https://lookerstudio.google.com/reporting/de1ff1d6-7487-4b29-a3c5-3f50a79ca9df
</details>


