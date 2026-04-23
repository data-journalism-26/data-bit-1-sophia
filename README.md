# Inside Algeria’s Migration Crackdown

This data journalism project investigates Algeria’s migration control practices using a newly compiled dataset based on official military reports.

🔗 **Read the article:**  
👉 👉 https://data-journalism-26.github.io/data-bit-1-sophia/

Interactive article built with Svelte and Observable Plot. For grading, please view the repository contents and build output in this submission.

---

## 🔍 About the project

Algeria remains a “black box” in migration governance. Unlike neighboring countries, it does not formally cooperate with the European Union on migration control. Yet, evidence suggests a steady intensification of enforcement practices.

This project reconstructs these dynamics by combining:

- scraped official military reports  
- external reporting from NGOs and media  
- qualitative insights from expert interviews  

The result is a rare longitudinal perspective on migration-related arrests between **2015 and 2025**.

---

## 📊 Data & Methods

### Sources
- Algerian Ministry of Defence operational reports (primary source)
- Archived news articles (secondary source)
- NGO reporting (Amnesty International, Human Rights Watch, Alarm Phone Sahara)

### Methods
- Web scraping in **R** (`rvest`, `tidyverse`)
- Manual archival research
- Data cleaning and harmonization
- Visualization using **Svelte + Observable Plot**

### Sample
- 100+ reports
- Time span: 2015–2025

---

## ⚠️ Data limitations

Official figures are not independently verifiable and should be interpreted with caution.

They are used here to identify **patterns and trends**, not exact migration flows.

---

## 🖥️ Project structure

```txt
src/                    # Svelte source code (article + visualization)
static/data/            # Dataset used in the visualization
build/ (optional)       # Production build output

Algeria_Defense_...R    # Scraping and data processing script
df_immigration_...xlsx  # Compiled dataset

README.md

Final submission version.
