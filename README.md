# AWS Glue DataBrew Project

This project demonstrates end-to-end data preparation, cleaning, and transformation using **AWS Glue DataBrew** with Amazon S3 datasets (`customers.csv` and `sales.csv`).  
The goal is to showcase practical **ETL (Extract, Transform, Load)** skills that align with **Data Engineer I** requirements at Amazon and other top companies.

---

## 🚀 Project Overview
- Built a **DataBrew project** to clean and transform raw customer and sales data stored in **Amazon S3**.  
- Created **recipes** with multiple transformation steps for data cleaning, formatting, and enrichment.  
- Generated **data lineage** to track the flow from raw data → transformed dataset → output in JSON/Parquet.  
- Ensured data quality using **DataBrew profiling** (column statistics, missing values, uniqueness, PII checks).  
- Created an **ETL job in AWS Glue Studio** to combine Customers and Sales datasets and write processed data back to S3.

---

## 📂 Project Structure
```
aws-glue-databrew-project/
├── data/                      # optional sample CSVs (if included)
├── docs/                      # optional extra documentation
├── screenshots/               # screenshots proving work (see list below)
└── README.md                  # this file
```

---

## 🗂 Datasets
- **Customers dataset** — customer details (Prefix, First_Name, Last_Name, Gender, DoB, Address, Zip, City, Country)  
- **Sales dataset** — transaction records (Txn_Date, Customer_Id, Product_Id, Quantity, Total_Sales)  

> Both datasets are AWS sample CSVs used for learning and demonstration.

---

## 🛠 Tools & Services
- **AWS Glue DataBrew** — data profiling, interactive recipe authoring, cleaning  
- **AWS Glue Studio** — visual ETL job for joins, aggregates, and final output  
- **Amazon S3** — raw and processed data storage  
- **AWS Glue Data Catalog** — metadata & schema (optional)

---

## 🔧 Key Transformations (CleanCustomer recipe)
1. Remove unnecessary columns (`Middle_Name`, `Suffix`).  
2. Reformat `DoB` to `MM-dd-yyyy` and mask later for privacy.  
3. Remove special characters from `Address`.  
4. Split `Address` into address parts using delimiter (`;`).  
5. Rename split columns:  
   - `Address_3` → `City`  
   - `Address_4` → `Zip`  
   - `Address_5` → `Country`  
6. Merge `Prefix + First_Name + Last_Name` → `Name` (or `Full_Name`).  
7. Convert `Name` to UPPERCASE.  
8. Mask `DoB` values (e.g., `1970-01-01` → `####-##-##`) for privacy in outputs (if required).

---

## 🔗 Data Lineage & Quality
- **Lineage:** Raw CSV (S3) → DataBrew recipe (transformations) → DataBrew output / Glue Studio job → Processed files (S3 JSON/Parquet).  
- **Quality checks:** Column statistics, uniqueness, missing-value reports, simple PII identification were run in DataBrew.

---

## 📸 Required screenshots (place in `screenshots/` folder)
Put these images into `screenshots/` with exactly these filenames so image links render correctly on GitHub:

- `customers_dataset_preview.png`  
- `sales_dataset_preview.png`  
- `customers_column_statistics.png`  
- `clean_customer_project_creation.png`  
- `clean_customer_provisioning.png`  
- `clean_customer_recipe_steps.png`  
- `clean_customer_profiled_view.png`  
- `customers_address_city_country.png`  
- `customers_data_lineage.png`

(If some screenshots don’t exist, include the ones you have and remove unused names.)

---

## 🎯 Learning Outcomes
- Hands-on experience with DataBrew recipes, profiling, and Glue Studio.  
- Practical understanding of designing a production-like ETL pipeline.  
- Portfolio-ready demonstration of AWS data-workflow skills for interviews.

---

## 🤝 Notes
- Datasets used are **AWS sample data** and contain no real customer info.  
- This project is for learning and portfolio purposes only.
