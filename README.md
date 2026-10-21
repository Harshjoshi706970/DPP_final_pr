Customer Credit Risk — Holistic Data Preparation & Feature Engineering
An end-to-end data preparation, cleaning, and feature engineering pipeline built for credit risk assessment datasets. This repository contains workflows to ingest multi-format datasets, resolve missing values using statistical and iterative techniques, treat outliers, encode mixed variable types, apply statistical power transformations, and generate domain-specific features.

Table of Contents
Overview

Key Features

Dataset Architecture

Pipeline Workflow

Project Structure

Installation & Setup

Usage

Methodology & Implemented Techniques

License

OverviewCredit risk modeling demands structured, clean, and representative data. Raw records commonly suffer from inconsistent data types, unstandardized scales, extreme financial outliers, and missing entries across demographic and transactional metrics.   This repository demonstrates a production-grade data preprocessing pipeline that transforms raw demographic, behavioral, and financial datasets into an ML-ready dataset of 817 samples and 38 features.


Key FeaturesMulti-Source Ingestion: Ingests tabular CSVs, nested JSON structures, and executes in-memory SQLite queries.   Automated Data Profiling: Integrates ydata-profiling to evaluate distribution skewness, cardinality, and interaction correlations.   Adaptive Imputation: Applies univariate (median/mode), missing-indicator random sampling, and multivariate methods (KNNImputer and IterativeImputer/MICE).   Robust Outlier Handling: Employs Z-score filtering ($\vert{}Z\vert{} < 3$), Interquartile Range (IQR) bounds, percentile trimming (5th–95th), and 5th/95th percentile Winsorization.   Categorical & Numeric Discretization: Implements OrdinalEncoder, LabelEncoder, one-hot encoding, equal-width binning (pd.cut), binary thresholds (Binarizer), and cluster-based discretization (KBinsDiscretizer with Quantile/K-Means).   Mathematical Scaling & Transformations: Implements StandardScaler, MinMaxScaler, Normalizer, MaxAbsScaler, np.log1p, Box-Cox, and Yeo-Johnson power transformations.   Domain Feature Construction: Formulates risk-based indicators such as Debt-to-Income (DTI), Average Monthly Transactions, and Spending-to-Income ratios. 

Dataset ArchitectureThe project harmonizes three distinct data sources:   Source FileFormatKey Fields / Contentscustomer_credit_risk_dataset.csvCSV   Demographics, employment, annual income, credit scores, default flag (1,000 rows, 15 columns).   behavioral_attributes_dataset.jsonJSON   Customer join dates, transaction counts, spending ratios, missed payments (500 rows).   financial_details.sqlSQL Script   In-memory SQLite relational table with income, loan amount, and credit profiles (500 rows).   

1. Multi-Source Ingestion (CSV / JSON / SQLite)
                      │
                      ▼
 2. Automated Profiling (ydata-profiling HTML generation)
                      │
                      ▼
 3. Missing Value Imputation
    ├── SimpleImputer (Median for Age, Mode for Gender/Employment)
    ├── Missing Indicator + Random Sample Imputation (Income)
    └── Multivariate Imputation (KNN & MICE on financial features)
                      │
                      ▼
 4. Outlier Detection & Remediation
    ├── Z-score Filtering (|z| < 3 on Annual Income)
    ├── IQR Trimming (Loan Amount)
    ├── Percentile Trimming (5th-95th percentile on Credit Score)
    └── Winsorization (Clipping Repayment History at 5th/95th)
                      │
                      ▼
 5. Feature Engineering & Encodings
    ├── Date Extraction (Year, Month, Day, Weekday)
    ├── Categorical Encoding (Ordinal, Label, One-Hot)
    └── Binning & Discretization (Equal-width, Quantile, K-Means)
                      │
                      ▼
 6. Scaling, Power Transforms & Feature Construction
    ├── StandardScaler, MinMaxScaler, MaxAbsScaler, Normalizer
    ├── Log1p, Box-Cox, Yeo-Johnson Power Transformations
    └── Derived Metrics (Debt-to-Income, Monthly Transactions)
                      │
                      ▼
 7. Model-Ready Export (final_dataset.csv: 817 rows × 38 cols)
