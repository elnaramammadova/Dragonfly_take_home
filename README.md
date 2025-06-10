# Junior Data Scientist Take-Home Task: Product Catalogue Creation

This project demonstrates the process of ingesting, cleaning, exploring, and deduplicating product catalogue data from two sources to create a unified master catalogue. The workflow is implemented in a Jupyter Notebook and leverages modern data science tools and techniques for entity resolution.

## Table of Contents

- [Overview](#overview)
- [Data Sources](#data-sources)
- [Workflow](#workflow)
    - [1. Data Ingestion](#1-data-ingestion)
    - [2. Data Exploration & Cleaning](#2-data-exploration--cleaning)
    - [3. Product Deduplication](#3-product-deduplication)
        - [3.1. Text Standardization](#31-text-standardization)
        - [3.2. Fuzzy Matching](#32-fuzzy-matching)
        - [3.3. Hybrid Feature-Based Entity Resolution](#33-hybrid-feature-based-entity-resolution)
- [Outputs](#outputs)
- [Requirements](#requirements)
- [How to Run](#how-to-run)
- [Notes](#notes)

---

## Overview

The goal is to merge two product catalogues, identifying and consolidating duplicate entries using a combination of text cleaning, TF-IDF vectorization, cosine similarity, and fuzzy matching. The final output is a master catalogue with deduplicated and enriched product information.

## Data Sources

- `bd_technologies.csv`: Product catalogue from Source BD.
- `ts_technologies.csv`: Product catalogue from Source TS.

## Workflow

### 1. Data Ingestion

- Load both CSV files into pandas DataFrames.
- Reset indices for consistency.

### 2. Data Exploration & Cleaning

- Display sample rows, column names, and summary statistics.
- Generate data quality reports (missing values, data types).
- Identify and handle duplicates.
- Clean missing values (replace empty/space-only fields with NaN).
- Standardize data types (convert relevant columns to string).
- Standardize URLs (remove protocols, `www`, and trailing slashes).
- Clean and normalize category columns.
- Remove hyphens from specific columns for consistency.
- Combine and normalize category information for better matching.

### 3. Product Deduplication

#### 3.1. Text Standardization

- Lowercase, remove punctuation, extra whitespace, and stopwords.
- Apply lemmatization to key text fields.
- Concatenate relevant columns into a single `combined` field for each product.

#### 3.2. Fuzzy Matching

- Use TF-IDF vectorization and cosine similarity to pre-filter top candidate matches.
- Apply fuzzy string matching (RapidFuzz) to the top candidates for each product.
- Store best matches and their scores.
- Merge matched records, preferring longer descriptions and combining URLs/categories.
- Identify unmatched records from each source.

#### 3.3. Hybrid Feature-Based Entity Resolution

- Normalize and block on categories to reduce candidate pairs.
- Use `recordlinkage` and `rapidfuzz` to compute fuzzy scores for candidate pairs.
- Fuse matched records and append unmatched records from each source.
- Output a comprehensive master catalogue with provenance and match scores.

## Outputs

- `data/bd_technologies_cleaned.csv`: Cleaned BD catalogue.
- `data/ts_technologies_cleaned.csv`: Cleaned TS catalogue.
- `data/master_catalogue_fuzzy_matching.csv`: Master catalogue from fuzzy matching.
- `master_catalogue_hybrid.csv`: Master catalogue from hybrid entity resolution.

## Requirements

- Python 3.7+
- pandas
- numpy
- scikit-learn
- nltk
- rapidfuzz
- recordlinkage
- tqdm
