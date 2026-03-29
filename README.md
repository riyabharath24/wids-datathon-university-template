<p align="center">
  <img src="https://www.widsworldwide.org/wp-content/uploads/2023/05/WiDS_logo_nav.png" alt="WiDS Logo" width="250"/>
</p>

<h1 align="center">WiDS Datathon 2026 – Student Submission Template</h1>

Welcome to your project workspace for the WiDS Datathon 2026! This repository serves as your final submission for grading and sharing. All analysis should be done in Google Colab, and your final results will be presented in a standalone slide deck.

---

## Getting Started

### 1. Clone or Fork this Repository
Click the **Fork** button at the top-right of this page to create your own copy under your GitHub account.

### 2. Edit the Notebook in Colab  
Click below to open the notebook directly in Google Colab:  
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](notebook.ipynb)

- Do all data cleaning, EDA, modeling, and analysis in the notebook.  
- Use Markdown cells to explain each step.

---

## Problem Statement

This year’s WiDS Datathon challenge offers **two powerful routes**. Each one addresses real-world problems caused by wildfires. Your team must choose **ONE** route and clearly indicate your choice in both your notebook and slide presentation.

---

### 🔹 Route 1: Accelerating Equitable Evacuations

**Core Question:**  
*How can we reduce delays in evacuation alerts and improve response times for the communities that are most at risk?*

This route focuses on analyzing how and when evacuation alerts are triggered — and how we can improve timeliness and fairness in communication, especially for vulnerable populations.

**Suggested Starting Points:**
- Compare `date_modified` and `effective` fields in WatchDuty to identify alert lags.
- Integrate NOAA weather data (wind, humidity) to model fire spread and shrinking evacuation windows.
- Map low-mobility zones (e.g. road access, car ownership) against health vulnerabilities (e.g. asthma, elderly population).
- Build a risk surface using perimeter snapshots, alert timestamps, and report logs to assess delays.
- Use `timestamp_reported` and classification reports to study "pending" alerts and delays.

**Why this matters:**  
Improved risk dashboards, real-time alerts, and support systems for people with disabilities, pets, or other special needs.

---

### 🔹 Route 2: Designing for Economic Resilience

**Core Question:**  
*How can wildfire disruption analytics inform stronger economic safety nets for affected workers, families, and small businesses?*

This route is about quantifying how wildfires affect employment, income, and tourism — and using that insight to design better protections for vulnerable communities.

**Suggested Starting Points:**
- Use `geo_event_type` and evacuation zones to estimate lost workdays and industry impact.
- Overlay fire risk maps with labor statistics to simulate “fire leave” policy impacts.
- Map wildfire frequency against Airbnb density and seasonal tourism data to assess local economic fragility.
- Explore long-term effects like secondary illness, displacement, or financial strain.

**Why this matters:**  
Supports for gig workers, targeted aid for small businesses, and policy tools for economic recovery.

---

**Clearly state your chosen route at the top of your notebook and slide deck.** Your work should combine **data analysis**, **modeling**, and **real-world relevance** to propose actionable insights.

---

## Project Title & Team Info

**Project Title**: _Your Title Here_  
**Team Name**: _Your Team Name_  
**University**: _Your University_  
**Course**: _Course Name (e.g., Data Science Capstone)_  
**Term**: _Quarter/Semester & Year_  

**Team Members**:  
- Name 1 (GitHub: [@username](https://github.com/username))  
- Name 2  
- Name 3  
- Name 4  

---

## Dataset Overview

Summarize the datasets you used and how you processed them.

- `infrastructure.csv`: Metadata and coordinates of infrastructure
- `fire_perimeters.geojson`: Timestamped fire perimeter polygons
- `evacuation_zones.csv`: (Optional) evacuation declarations
- `watch_duty_change_log.csv`: Alerts and timestamps
- (Optional) NOAA weather data or census data

Mention any merging, filtering, or assumptions.

---

## Approach

Explain your full workflow:
- Preprocessing and cleaning
- Spatial joins, feature engineering, timestamp analysis
- Modeling (if applicable): regression, classification, clustering, etc.
- Evaluation metrics used (AUC, F1, RMSE, etc.)
- Tools used (e.g., pandas, geopandas, scikit-learn)

---

## Results

Report your final results and key insights:
- Metrics: Precision, Recall, ROC AUC, RMSE, etc.
- Key findings or visualizations (include in slides)
- Any limitations or ethical considerations

---

## Team Contributions

| Name             | Contributions                                                                  |
|------------------|--------------------------------------------------------------------------------|
| Riya Bharathwaj  | EDA, Feature engineering, modeling, building solution, presentation prep       |
| Ting-ya Chang    | EDA, geospatial joins, Research/Outreach, building solution, presentation prep |
| Saehee Eom       | EDA, Feature engineering, modeling, building solution, presentation prep       |
| Tanmayee Kolli   | EDA, Research/Outreach, building solution, presentation prep                   |
| Simran Mallik    | EDA, preprocessing, Research/Outreach, building solution, presentation prep    |



---

## How to Reproduce

1. Clone this repository:
   ```bash
   git clone https://github.com/YOURTEAM/wids-2026-project.git
   cd wids-2026-project

---

## Questions?

Visit the community hub: [WiDS Community](https://community.widsworldwide.org)

