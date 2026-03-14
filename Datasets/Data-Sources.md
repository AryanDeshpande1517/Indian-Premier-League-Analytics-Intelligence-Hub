# 📂 Dataset Documentation

## IPL Ball-by-Ball Match Data

**Official Source:**  
https://cricsheet.org/downloads/

The dataset consists of **1,100+ IPL match JSON files** containing complete ball-by-ball information for Indian Premier League matches.

Each JSON file contains detailed match data including:

- Match metadata
- Teams and venues
- Toss decisions
- Player participation
- Delivery-level scoring events
- Wickets and extras
- Innings-level summaries

## 📁 Dataset Structure (Original Format)

The original dataset is stored as individual **match-level JSON files**.

Each file represents a single IPL match.

Example structure:
ipl_male_json/
match_id.json

Each match file contains nested hierarchical data including:

- Match metadata
- Player information
- Ball-by-ball deliveries
- Innings summaries

## ⚙️ Data Engineering Process

Since the dataset was provided in **nested JSON format**, it could not be directly used for analytics.

A custom **Python ETL pipeline** was built to:

- Parse nested JSON structures
- Extract match-level metadata
- Extract ball-by-ball deliveries
- Generate structured analytical tables
- Normalize team and venue names
- Aggregate innings-level summaries
- Prepare datasets for Star Schema modeling in Power BI

The ETL pipeline is documented inside: Scripts/IPL_Extraction.ipynb

## ⚠️ Licensing & Redistribution

This repository does **not redistribute raw match JSON files**.

To reproduce this project:

1. Download the IPL dataset from Cricsheet.
2. Run the ETL notebook provided in the Scripts folder.
3. Generate structured CSV datasets.
4. Import the datasets into Power BI.
5. Apply DAX measures documented in the repository.

## 📊 Data Prepared for Modeling

After running the ETL pipeline, the following structured datasets were generated:

- IPL Matches
- IPL Deliveries (Ball-by-Ball)
- IPL Match Summary
- IPL Over Summary
- Team Dimension
- Player Dimension

These datasets were modeled using a **Star Schema design** optimized for Power BI analytics.