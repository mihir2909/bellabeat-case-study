# 📊 Bellabeat Smart Device Data Analysis

> **Google Data Analytics Capstone Project**  
> *Analyzing non-Bellabeat smart device fitness tracker data to uncover consumer usage trends and provide strategic marketing recommendations for the **Bellabeat App**.*

---

## 🎯 Phase 1: Ask

### Business Task
Analyze non-Bellabeat smart device fitness data (Fitbit) to discover consumer usage patterns and trends regarding daily activity, sleep, and physical engagement. Apply these insights to the **Bellabeat App** to guide marketing strategies, increase user engagement, and highlight features that drive customer growth.

### Key Stakeholders
* **Urška Sršen:** Cofounder and Chief Creative Officer (Executive lead driving product strategy)
* **Sando Mur:** Cofounder and key member of the Bellabeat executive team
* **Bellabeat Marketing Analytics Team:** Primary team responsible for data collection, analysis, and strategic recommendations

### Core Guiding Questions
1. What are the primary trends in consumer smart device usage?
2. How do these trends apply to Bellabeat's target audience of health-conscious women?
3. How can these trends inform marketing strategies and feature promotion for the Bellabeat App?

---

## 📁 Phase 2: Prepare

### Data Source Information
* **Dataset Name:** [FitBit Fitness Tracker Data](https://www.kaggle.com/datasets/arashnic/fitbit) (Public Domain / CC0 on Kaggle via Mobius)
* **Dataset Contents:** Contains personal fitness tracker data from eligible Fitbit users, including minute-level/daily-level output for physical activity, heart rate, steps, calories, and sleep monitoring.
* **Primary Tables Used:**
  * `dailyActivity_merged.csv`: Tracks daily steps, total distance, activity intensities, and calories burned.
  * `sleepDay_merged.csv`: Tracks daily sleep logs, total sleep minutes, and time spent in bed.

### Data Credibility & Limitations (ROCCC Analysis)
* **Reliable:** ⚠️ **Low** — Small sample size ($N = 30\text{--}33$ unique users).
* **Original:** ⚠️ **Low** — Third-party survey data collected via Amazon Mechanical Turk.
* **Comprehensive:** ⚠️ **Moderate** — Tracks activity and sleep, but lacks demographic data (e.g., gender, age). Because Bellabeat specifically targets **women**, this is a critical limitation.
* **Current:** ⚠️ **Low** — Data collected in 2016 (may not fully reflect modern post-pandemic fitness habits).
* **Cited:** ✅ **High** — Open-source public domain dataset published on Kaggle.
