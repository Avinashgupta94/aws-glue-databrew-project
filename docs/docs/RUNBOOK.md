# Runbook — AWS Glue DataBrew / Glue Studio Project

This runbook explains how to reproduce, inspect, and run the work in this repository.

- DataBrew recipe: `docs/CleanCustomer-recipe.json`  
- Example datasets: `data/customers.csv`, `data/sales.csv`  
- Proof screenshots: `screenshots/`

---

## Prerequisites
1. An AWS account with permissions to use **S3**, **AWS Glue DataBrew**, **AWS Glue Studio**, **IAM**, and **Amazon S3**.
2. AWS Console access in the same region used while creating the DataBrew project (for example: `ap-south-1`).
3. `customers.csv` and `sales.csv` uploaded to the `data/` folder in this repo (or to an S3 bucket you control). If using S3, note the bucket and path for each dataset.

---

## 1) Inspect files in this repo
- `README.md` — project overview and screenshots list.  
- `docs/CleanCustomer-recipe.json` — exported DataBrew recipe (JSON).  
- `data/` — sample CSV files used for the demo (optional; you can also use S3 sample data).  
- `screenshots/` — images proving the steps and outputs.

---

## 2) Import DataBrew recipe into AWS Glue DataBrew
Use these steps to import the JSON recipe so you can reproduce the exact recipe steps.

1. Open the AWS Console → **Glue DataBrew** → **Recipes**.
2. Click **Import recipe** (top-right) → choose `docs/CleanCustomer-recipe.json` from this repo (download locally first).
3. Give the imported recipe the name `CleanCustomer` (or keep the original name).
4. Inspect the recipe steps in the DataBrew UI to confirm they match the project (you will see steps like delete columns, reformat DoB, split address, rename, merge name, uppercase, mask DoB, etc.)

> Note: If you prefer to import directly from S3, upload `docs/CleanCustomer-recipe.json` to an S3 bucket and import by S3 path.

---

## 3) Reproduce the DataBrew demo (interactive)
This reproduces the flow used to create the screenshots and the processed outputs.

1. Open **Glue DataBrew** → **Projects** → **Create project**.
2. Name the project `CleanCustomer-project`.
3. Choose dataset:
   - Option A: Use the `data/customers.csv` file from this repo (upload to your S3 and point DataBrew dataset to it).
   - Option B: Point to your sample S3 dataset path (e.g., `s3://your-bucket/getting-started-with-glue-studio-me/datafiles/customers/`).
4. When project opens, choose **Recipes** → **Import steps** → select the imported `CleanCustomer` recipe or apply steps manually.
5. Click **Profile** to run a profile job and validate column statistics (missing values, unique counts, data types).
6. Preview the output and take screenshots (these are the project screenshots included in `screenshots/`).

---

## 4) Export recipe (already included)
- The exported recipe JSON is in `docs/CleanCustomer-recipe.json`. Use it to import the exact same steps into any DataBrew environment.

---

## 5) Create ETL job in AWS Glue Studio (visual) — join Customers + Sales
If you want to reproduce the Glue Studio job that joins Customers and Sales and writes aggregated output:

1. Open **Glue Studio** → **Jobs** → **Visual** → **Create**.
2. Add two **Data source - S3** nodes:
   - Customers dataset S3 path (or Data Catalog table if you crawled it).
   - Sales dataset S3 path.
3. For each source, add a **Transform - Change Schema / Select fields** node to align column names (drop unused columns).
4. Add a **Transform - Join** node:
   - Join condition: `Customers.customer_id = Sales.customer_id` (use the actual key column names present).
5. Add **Transform - Aggregate** node if you want totals per customer or product (e.g., SUM(Total_Sales) GROUP BY Customer_Id).
6. Add a **Data target - S3** node:
   - Choose output format `JSON` or `Parquet` (Parquet is recommended for analytics).
   - Choose partitioning (optional) and output path `s3://your-bucket/processed/aws-glue-databrew/`.
7. Save and Run the job. Check the Glue job runs and logs for success.

---

## 6) How to store artifacts in this repo
- `docs/` — keep exported recipe JSON and runbook or job config.
- `data/` — where sample CSVs live (committed for demo; heavy/real data should not be committed).
- `screenshots/` — proof images used for the README.
- `README.md` — project summary (already present).

---

## 7) Quick troubleshooting & tips
- **Git vs GitHub UI**: You can upload files via GitHub web UI (Add file → Upload) — no local git required. You used that successfully.
- **If the DataBrew import fails**: Open the JSON file and verify encoding is UTF-8 and the JSON structure begins with a recipe object (the file in `docs/` should be correct).
- **IAM missing permissions**: Ensure your user/role has `databrew:*`, `glue:*`, `s3:*` (or scoped-down equivalents). Minimal: `databrew:ImportRecipe`, `databrew:CreateProject`, `glue:CreateJob`, `s3:GetObject`, `s3:PutObject`.
- **Regional mismatch**: Ensure the datasets and Glue/DataBrew project are in the same AWS Region.

---

## 8) Where to add/attach the Glue Studio job script (optional)
- You can export Glue job Python script from Glue Studio and store it under `docs/` as `glue_job_clean_customer.py` for reproducibility.

---

## 9) Validation checklist (what I checked while preparing this repo)
- [x] README with project overview + screenshots list.
- [x] `docs/CleanCustomer-recipe.json` included and importable.
- [x] `screenshots/` contains images to show dataset preview, recipe steps, profiled view, lineage, etc.
- [x] `data/` contains small sample CSVs (only for demo).
- [x] RUNBOOK describing reproduction steps (this file).

---

## Contact
If anything is unclear or you need the exported Glue job script, contact Avinash Gupta — https://github.com/Avinashgupta94

---
