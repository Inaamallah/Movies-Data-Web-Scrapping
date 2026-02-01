# 🎬 TMDB Movie Data Scraper

![Python](https://img.shields.io/badge/Python-3.x-blue?style=for-the-badge&logo=python)
![Libraries](https://img.shields.io/badge/Library-BeautifulSoup%20%7C%20Pandas%20%7C%20Requests-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

## 🧐 The Problem
As a data enthusiast, I wanted to analyze the relationship between **production budgets** and **user reception** for currently popular and upcoming movies. However, I faced a common roadblock:

* **Outdated Data:** Most public datasets (like on Kaggle) are static and months or years old.
* **Missing Fields:** Simple lists often lack critical details like *Budget*, *Revenue*, or specific *MPA Ratings*.
* **Unstructured Formats:** The data available on the web is often trapped in HTML tags, making it impossible to analyze directly in Excel or SQL.

## 💡 The Solution
I built a custom **End-to-End Web Scraper** using Python. This tool automates the entire data collection pipeline:

1.  **Navigates** the "Popular Movies" section of The Movie Database (TMDB).
2.  **Iterates** through movie cards to capture initial metadata (Titles and Links).
3.  **"Digs Deeper"** by visiting each individual movie profile to scrape granular details (Budget, Genres, Runtime).
4.  **Cleans & Structures** the data into a Pandas DataFrame.
5.  **Exports** a ready-to-analyze `Movies_Scrapped_Data.csv` file.

---

## 📂 Data Fields Extracted
The script extracts **10 key data points** for every movie:

| Column Name | Description | Example |
| :--- | :--- | :--- |
| **Movie Name** | Title of the film |
| **Link** | URL to the movie details page |
| **MPA** | Age rating certification |
| **User Score** | Audience rating percentage |
| **Overview** | Brief plot summary |
| **Genre** | Comma-separated list of genres |
| **Duration** | Runtime of the movie |
| **Released Date** | Official release date  |
| **Budget** | Production cost (Cleaned) |
| **Revenue** | Box office earnings (Cleaned) |

---

## 🛠️ Tech Stack
The project is built using the following Python libraries:

* **[Requests](https://pypi.org/project/requests/):** For sending HTTP requests to the target website.
* **[BeautifulSoup (bs4)](https://pypi.org/project/beautifulsoup4/):** For parsing HTML content and traversing the DOM tree.
* **[Pandas](https://pandas.pydata.org/):** For data manipulation, cleaning, and exporting to CSV.

---

## 🚀 How It Works (Code Structure)

The script is modularized into five main functions to ensure maintainability:

### 1. `fetching_cards(main_url)`
* **Input:** The main URL of the TMDB popular movies page.
* **Action:** Requests the page and uses `BeautifulSoup` to find all HTML containers (`div.card style_1`) representing individual movies.
* **Output:** A list of raw HTML card elements.

### 2. `extracting_title_link(cards)`
* **Action:** Loops through the cards to extract the **Movie Name** and **Relative Link**.
* **Logic:** Appends the base domain (`https://www.themoviedb.org`) to relative paths to create working URLs.
* **Output:** A DataFrame containing titles and links.

### 3. `extracting_data(df)`
* **Action:** The core scraping engine. It iterates through every link in the previous DataFrame.
* **Features:**
    * **Robust Error Handling:** Uses `try-except` blocks to skip unreleased movies without crashing.
    * **Data Cleaning:** Removes text labels (e.g., "Budget ") to leave clean numerical strings.
    * **Default Values:** Assigns `'N/A'` or `'Not Rated'` if data is missing (common for unreleased films).
* **Output:** A DataFrame with detailed movie metrics.

### 4. `merging_dataframes(df, df_result)`
* **Action:** Concatenates the initial metadata with the detailed metrics.
* **Output:** One final, consolidated DataFrame.

### 5. `main()`
* **Action:** The orchestrator function that calls all steps in order and saves the final file.

---


## 📊 Sample Data Output
*From the generated CSV file:*

```csv
Movie Name,MPA,User Score,Budget,Revenue
The Wrecking Crew,R,65,-,-
Greenland 2: Migration,PG-13,65,"$90,000,000.00","$11,416,907.00"
Zootopia 2,PG,76,"$150,000,000.00","$1,744,338,246.00"
