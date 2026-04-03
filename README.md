<p align="center">
  <img src="https://www.widsworldwide.org/wp-content/uploads/2023/05/WiDS_logo_nav.png" alt="WiDS Logo" width="250"/>
</p>

<h1 align="center">WiDS Datathon 2026 – Ramblin' Pathfinders</h1>

![CERMmatching](images/matching.gif)

---

### 🔹 Route 1: Accelerating Equitable Evacuations

**Core Question:**  
*How can we reduce delays in evacuation alerts and improve response times for the communities that are most at risk?*

This route focuses on analyzing how and when evacuation alerts are triggered — and how we can improve timeliness and fairness in communication, especially for vulnerable populations.

**Why this matters:**  
Improved risk dashboards, real-time alerts, and support systems for people with disabilities, pets, or other special needs.

---

## Project Title & Team Info

**Project Title**: Community Evacuation Resource Matcher (CERM)  
**Team Name**: Ramblin' Pathfinders  
**University**: Georgia Institute of Technology  
**Course**: N/A   
**Term**: Spring 2026  

**Team Members**:  
- Riya Bharathwaj
- Tingya Chang
- Saehee Eom
- Tanmayee Kolli
- Simran Mallik

------

## **Abstract**

Wildfires in California create urgent, high-stakes evacuation scenarios where timely and equitable access to support resources can significantly impact outcomes. However, existing tools primarily focus on fire prediction and monitoring, rather than enabling real-time, community-level coordination of assistance.

In this project, we present a **community-driven evacuation support system** that connects individuals offering help with neighborhoods in need, using a combination of demographic vulnerability data, real-time request signals, and geographic proximity. Our approach integrates **rule-based matching with lightweight LLM-assisted categorization** to recommend high-priority census tracts where assistance is most needed.

Unlike fully automated systems, our solution emphasizes **decision support rather than control**, enabling users to make informed choices while ensuring privacy and avoiding misuse in high-risk active fire zones.

------

## **1. Introduction**

Wildfire evacuations disproportionately impact vulnerable populations, including:

- elderly residents, 
- people with disabilities, 
- and households without vehicle access. 

These groups often face **structural barriers to evacuation**, such as limited mobility, lack of transportation, or reduced access to timely information.

While critical tools such as fire perimeter tracking and evacuation alerts provide situational awareness, they do not address a fundamental coordination gap: **how can communities organize in real time to help vulnerable residents evacuate?** 

This project directly addresses that question by designing a system that:

- surfaces **where help is most needed**, and 
- enables individuals and community groups to **self-organize and provide assistance**.

The system is designed for **in-the-moment use** — not pre-disaster planning — and includes safeguards to ensure it is **not used in active emergency zones**, where emergency services (e.g., 911) and immediate evacuation are more appropriate.

## **2. Data Analysis**

The system draws on three primary data sources:

### **Fire Perimeter Data**

Historical records on fire perimeter are sourced from **Watch Duty** and **CalFire**. These data determine which census tracts are affected by wildfire and are used to enforce the system's safety constraint (see Section 3.4).

### **Demographic Vulnerability Data**

Census tract-level demographic data from the **American Community Survey** and the **US Census Bureau** captures three indicators of structural evacuation vulnerability:

- Percentage of residents aged 65 or older
- Percentage of residents with disabilities
- Percentage of households without vehicle access

Each tract is flagged if it exceeds the 75th percentile (Q3) within its county on any of these indicators — a per-county threshold that accounts for regional variation. The total number of flags serves as a proxy for evacuation difficulty. 

Our prototype development focused on three counties with elevated wildfire risk: **Butte, Shasta, and Riverside**.

### **Request Data (Prototype)**

User-submitted and simulated requests capture unmet needs such as transportation, medical assistance, and supplies. 

These are aggregated at the census tract level to produce two signals: 

- the volume of active requests and 
- the categories of need that remain unresolved.

## **3. System Design and Methodology**

The system consists of three components: a user input layer, an LLM-based categorization pipeline, and a matching engine.

### **3.1 User Input Layer**

Two types of users interact with the system. 

**Requesters**

- submit free-text descriptions of their needs and 
- can view aggregate demand across census tracts.

**Helpers**

- submit descriptions of the resources or services they can offer and 
- receive ranked recommendations for where to direct their assistance.

### **3.2 LLM-Based Categorization**

A large language model (DeepSeek-V3-0324) converts free-text into structured, machine-readable tags. **The LLM is used exclusively for categorization — not decision-making — which limits hallucination risk and preserves interpretability**. 

The model is prompted to extract three tag categories:

- **Service tags**: transportation, mobility assistance, heavy lifting, medical, food distribution, volunteer labor, childcare
- **Resource tags**: water, food, medicine, clothing, fuel, equipment, tools, first aid
- **Beneficiary tags**: elderly, disability, no vehicle, families, children, general

The prompt instructs the model to return only valid JSON with no additional explanation, ensuring consistent structured output for downstream matching.

### **3.3 Matching Engine**

Each census tract is scored against a helper's input using four weighted components:

| **Component**                     | **Weight** | **Description**                                              |
| --------------------------------- | ---------- | ------------------------------------------------------------ |
| Vulnerability Fit (vFit)          | 25%        | Alignment between the helper's target beneficiaries and the tract's vulnerability flags |
| Request Volume (rVol)             | 15%        | Normalized count of active requests within the tract         |
| Request Category Match (reqMatch) | 25%        | Proportion of unresolved requests that match the helper's offered services |
| Proximity (prox)                  | 35%        | Distance between the helper and the tract centroid, with a decay half-life of ~10 km |

### **Final Score**

*Score = 0.25 \* vFit + 0.15 \* rVol + 0.25 \* reqMatch + 0.35 \* prox*

ranks tracts and displays the top recommendations to the helper. Proximity carries the highest weight to minimize travel time under emergency conditions.

### **3.4 Fire-Aware Safety Constraint**

To prevent misuse, census tracts currently within active fire perimeters are excluded from matching entirely. 

Users located in these areas are redirected to emergency services and immediate evacuation guidance rather than community coordination features.

<p align="center">
  <img src="images/fire_intersect.png" alt="fire_intersect" width="750"/>
</p>



### **3.5 Community Coordination Mechanism**

Users can mark when they are providing help and when requests have been fulfilled, allowing demand signals to update dynamically. 

The system **does not enforce allocation** — it provides recommendations while users retain full decision-making control.

### **3.6 Privacy Protection**

Privacy is protected by design. In the current prototype:

- **Requesters**: see only the total number of open requests per census tract — no details of individual requests
- **Helpers**: see a request's address (all addresses in the prototype are simulated using non-residential locations) but no personal contact information

Future versions of the system will strengthen these protections further:

- Before a match is made, helpers will see only a rough area — not a precise address
- Full address and contact details will only be revealed once both sides have agreed to connect
- User identity authentication will be introduced to add another layer of trust and accountability

### **3.7 Technical Implementation**

**Frontend**

The frontend is a single-page application built with HTML, CSS, and JavaScript, using Leaflet.js for interactive mapping and hosted on GitHub Pages. 

**Backend**

The backend is a Python data pipeline (pandas, geopandas) handling census data cleaning and spatial processing. LLM calls are proxied through a Vercel serverless function.

## **4. Evaluation and Performance Metrics**

Due to the absence of real-time ground-truth evacuation outcomes, we evaluate our system using simulated request data and proxy metrics. These metrics assess both matching quality and real-world impact.

**4.1 Current Metrics**

These metrics evaluate **how effectively the matching algorithm assigns helpers to communities** based on proximity, vulnerability, and demand, using **simulated request data** in the absence of real-world deployment signals.

**Alignment Score**

The weighted score measures how well a helper matches a census tract based on proximity, vulnerability fit, request category match, and demand level.

- **High**: helper is assigned to the most appropriate location
- **Low**: suboptimal or inefficient matching

**Demand Coverage**

This represents the percentage of total requests that receive at least one matched helper.

- **High**: system successfully distributes available help
- **Low**: unmet needs remain in the system 

We compare the proposed method against three simplified baselines across the two metrics, using simulated data. 

| **Method**         | **Alignment Score** | **Demand Coverage** |
| ------------------ | ------------------- | ------------------- |
| Random             | Low                 | Low                 |
| Distance-only      | Medium              | Low                 |
| Vulnerability-only | Medium              | Medium              |
| **Our Method**     | **High**            | **High**            |

**4.2 Future Metrics**

These metrics evaluate whether the system **actually improves evacuation outcomes** and overall effectiveness after deployment, aligning directly with the goal of reducing evacuation delays and improving community response.

**Evacuation Efficiency**

This measures the reduction in evacuation time compared to baseline scenarios.

- **Measurement**:
  - Historical vs. post-deployment evacuation time comparison
  - Simulation: matched allocation vs. random or baseline strategies
- **Interpretation**: Direct impact on evacuation speed and coordination effectiveness

**Adoption Metrics**

Adoption metrics evaluate system usage and engagement.

- **Examples:**
  - Number of active users (helpers + requesters)
  - Number of requests submitted
  - Match completion rate
- **Interpretation:** Reflects scalability and real-world usability 

**4.3 Performance Insights**

From simulation and early testing:

- Strong performance in **high-engagement areas with sufficient volunteer supply**
- Performance decreases in areas with:
  - Low volunteer density
  - Highly variable or complex needs 

## **5. Conclusion and Impact**

This project demonstrates how a lightweight, interpretable system can support **real-time**, **community-driven evacuation assistance** during wildfire events. 

By combining demographic vulnerability signals, live request data, and geographic proximity within a transparent scoring framework, the system **bridges the gap between fire awareness tools and on-the-ground community coordination**.

The system has the potential to:

- improve evacuation support access for vulnerable populations, 
- enable faster decentralized response through community self-organization, and 
- provide meaningful decision support under time pressure — without over-automating choices that carry real human stakes.

**Limitations**

- Reliance on aggregated census data with no household-level precision
- Simulated request data in the prototype; no real-world validation yet
- Static scoring weights not tuned to specific disaster contexts
- No real-time fire spread modeling
- LLM misclassification risk, particularly for ambiguous or informal language

**Future Work**

- Integration with real-time shelter and emergency data (Cal OES, Red Cross)
- Expansion to additional counties and states
- Incorporation of road accessibility and geographic isolation features
- Retrospective evaluation using historical evacuation scenarios
- Dynamic weight tuning based on real-world feedback

## **References**

[1] Melton, C. C., et al. (2023). Wildfires and older adults: A scoping review of impacts, risks, and interventions. International Journal of Environmental Research and Public Health.

[2] Rad, A. M., et al. (2023). Social vulnerability of populations exposed to wildfires in the United States. 

[3] Matsuo, Y. (2025). Evacuation and transportation barriers among vulnerable populations in disasters. 

[4] UCLA Institute of Transportation Studies. (2025). Wildfire recovery and resilience strategies for vulnerable communities.

[5] Sun, Y., et al. (2024). Social vulnerabilities and wildfire evacuations: A case study of the 2019 Kincade Fire.

[6] FEMA. (2011). *A Whole Community Approach to Emergency Management: Principles, Themes, and Pathways for Action.*

[7] National Academies of Sciences, Engineering, and Medicine. (2019). *Evacuation Decision Making in Disasters.*

[8] Aldrich, D. P., & Meyer, M. A. (2015). Social capital and community resilience. *American Behavioral Scientist.*


## Team Contributions

| Name             | Contributions                                                                  |
|------------------|--------------------------------------------------------------------------------|
| Riya Bharathwaj  | EDA, Feature engineering, modeling, building solution, presentation prep       |
| Ting-ya Chang    | EDA, geospatial joins, Research/Outreach, building solution, presentation prep |
| Saehee Eom       | EDA, Feature engineering, modeling, building solution, presentation prep       |
| Tanmayee Kolli   | EDA, Research/Outreach, building solution, presentation prep                   |
| Simran Mallik    | EDA, preprocessing, Research/Outreach, building solution, presentation prep    |

