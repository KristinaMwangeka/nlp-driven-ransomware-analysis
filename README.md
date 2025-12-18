#NLP DRIVEN RANSOMWARE FORUMS WEBSCRAPING AND ANALYSIS

## Project Overview

This project demonstrates an end-to-end **data engineering and data processing pipeline** for collecting, cleaning, and enriching unstructured cybersecurity data from online forums. The goal was to gather real-world discussions on **ransomware experiences, advice, and attacker techniques** and prepare the data for downstream analysis by analytics and research teams.

The final output is a **cleaned, structured, and classified dataset** that supports victim-focused and attacker-focused analysis.

---

## Data Sources

The dataset was collected from **8 publicly accessible discussion threads** across two platforms:

* **Reddit** – Subreddits and individual threads discussing ransomware incidents, recovery advice, and user experiences
* **ESET Forum** – A cybersecurity vendor forum containing technical discussions, incident reports, and mitigation advice

These sources provide diverse perspectives including victims, security practitioners, and technical community members.

---

## Data Engineering Workflow

### 1. Environment Setup

The project was developed and executed in **Google Colab** using Python. The following libraries were installed and configured:

* **Selenium** + **webdriver-manager** – For dynamic web scraping of forum content
* **BeautifulSoup4** – HTML parsing and structured content extraction
* **Requests** – HTTP requests for static pages
* **Pandas** – DataFrame-based data storage and transformation
* **PRAW (Python Reddit API Wrapper)** – Efficient and compliant access to Reddit posts and comments via the Reddit API

This hybrid setup allowed platform-specific optimization while maintaining a unified data pipeline.

---

### 2. Data Collection & Scraping

The scraping process iterated through each target URL and dynamically selected the appropriate extraction method based on platform type:

#### ESET Forum Scraping

* Initialized Selenium when required for dynamically loaded content
* Extracted:

  * Thread title
  * Post author
  * Post/comment content
* Iterated through all article elements within a thread to capture full discussion context

#### Reddit Scraping (via PRAW)

* Retrieved:

  * Main post title, author, and body text
  * All nested comments within the thread
* Captured for each comment:

  * Author
  * Content
  * Direct URL to the comment

All extracted records were appended into a unified Pandas DataFrame named `ALL_SCRAPED_DATA` with the following schema:

| Column Name       | Description                        |
| ----------------- | ---------------------------------- |
| Source Platform   | Reddit or ESET                     |
| Subreddit / Forum | Subreddit name or forum identifier |
| Thread Title      | Title of the discussion thread     |
| Content Type      | Post or Comment                    |
| Author            | Username or forum author           |
| Content           | Textual content                    |
| Source URL        | Direct link to the source          |

---

### 3. Data Cleaning & Text Normalization

The raw scraped text was cleaned and standardized to improve data quality and model performance:

* **Pattern Removal**

  * Used regular expressions to remove ESET quote headers (e.g., "On 3/15/2023 at…")
  * Removed UI artifacts such as "Quote" and "Expand"

* **Handling Deleted or Removed Content**

  * Identified Reddit placeholders such as `[deleted]` and `[removed]`
  * Dropped records that became empty after cleaning

* **Whitespace & Formatting Normalization**

  * Reduced excessive newlines to a maximum of two
  * Stripped leading and trailing whitespace

This resulted in clean, readable, and analysis-ready textual data.

---

### 4. Zero-Shot Text Classification

To enrich the dataset with semantic insights, a **zero-shot text classification** approach was applied.

* **Model Used:** `facebook/bart-large-mnli`
* **Framework:** Hugging Face `transformers`

This approach allowed classification without requiring labeled training data.

#### Classification Labels

Based on project objectives, the following categories were defined:

1. **Advice or solution provided to the victim**
2. **Comment from the victim describing their experience or asking for help**
3. **Description of attacker methods or techniques**

#### Implementation Details

* Implemented a reusable function `classify_comment`
* Truncated text to **1500 tokens** to prevent inference errors
* Assigned each text entry the label with the highest confidence score
* Stored results in a new column: `Predicted_Category`

---

## Final Output

The fully processed dataset was exported as:

**`scraped_ransomware_data_CLASSIFIED.csv`**

This file contains:

* Cleaned forum text
* Structured metadata
* Machine-generated categorical labels

The dataset was shared with **two analysts** to support victim behavior analysis and attacker technique assessment.

---

## Demonstrated Data Engineering Skills

This project highlights the following capabilities:

* Multi-source data ingestion (API-based + web scraping)
* Platform-aware scraping strategies
* Scalable text data cleaning and normalization
* Robust DataFrame-based data modeling
* Integration of NLP models into data pipelines
* Producing analytics-ready datasets for downstream teams
 Build dashboards for real-time ransomware trend monitoring

---

## Author

Kristina Mwangeka
LinkedIn https://www.linkedin.com/in/kristinamwangeka/

*Data Engineering & Analytics Project*
