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

--

## 🧹 Phase 3: Process

### Data Staging & Cleaning (SQL - BigQuery)

1. **Created Clean Staging Tables:** Created duplicate copies of raw tables (`daily_activity_cleaned` and `sleep_day_cleaned`) to perform transformations while preserving original raw data integrity.
2. **Standardized Column Names:** Converted all field names across tables to standard `snake_case` naming conventions (e.g., `user_id`, `total_steps`, `total_minutes_asleep`, `activity_date`).
3. **Data Deduplication:** Identified and filtered out duplicate sleep records from the staging dataset to ensure accurate nightly sleep aggregates.
4. **Merged Datasets:** Applied a `LEFT JOIN` on `user_id` and date fields to merge `sleep_day_cleaned` into `daily_activity_cleaned`, creating a unified primary dataset (`daily_activity_and_sleep`) while retaining all daily activity logs.
   
---

## 🔍 Phase 4: Analyze

### 1. Overall User Averages
* **Average Daily Steps:** ~7,638 steps (below the 10,000 daily recommendation).
* **Average Daily Sleep:** ~419 minutes (~7.0 hours).
* **Average Sedentary Time:** ~991 minutes (~16.5 hours per day).

```sql
SELECT 
  ROUND(AVG(total_steps), 2) AS avg_daily_steps,
  ROUND(AVG(total_minutes_asleep), 2) AS avg_sleep_minutes,
  ROUND(AVG(sedentary_minutes), 2) AS avg_sedentary_minutes
FROM `fitbit_data.daily_activity_and_sleep`;

### 2. Day-of-Week Trends
* **Most Active Days:** Tuesdays (~8,125 steps) and Saturdays (~8,153 steps).
* **Most Sedentary Day:** Mondays (~1,027 sedentary minutes).
* **Longest Sleep Day:** Sundays (~453 minutes / 7.5 hours).
  ```sql
   SELECT 
  day_of_week,
  ROUND(AVG(total_steps), 2) AS avg_steps,
  ROUND(AVG(total_minutes_asleep), 2) AS avg_sleep_mins,
  ROUND(AVG(sedentary_minutes), 2) AS avg_sedentary_mins
FROM `fitbit_data.daily_activity_and_sleep`
GROUP BY day_of_week
ORDER BY avg_steps DESC;

### 3. User Segmentation (Activity Tiers)

| Activity Tier | Criteria (Avg Daily Steps) | User Count | Percentage |
| :--- | :--- | :--- | :--- |
| **Low Active** | 5,000 – 7,499 steps | 9 | 27.27% |
| **Somewhat Active** | 7,500 – 9,999 steps | 9 | 27.27% |
| **Sedentary** | < 5,000 steps | 8 | 24.24% |
| **Highly Active** | ≥ 10,000 steps | 7 | 21.21% |
  ```sql
WITH user_tiers AS (
  SELECT 
    user_id,
    ROUND(AVG(total_steps), 2) AS avg_daily_steps,
    CASE 
      WHEN AVG(total_steps) < 5000 THEN 'Sedentary'
      WHEN AVG(total_steps) BETWEEN 5000 AND 7499 THEN 'Low Active'
      WHEN AVG(total_steps) BETWEEN 7500 AND 9999 THEN 'Somewhat Active'
      ELSE 'Highly Active'
    END AS activity_tier
  FROM `fitbit_data.daily_activity_and_sleep`
  GROUP BY user_id
)

SELECT 
  activity_tier,
  COUNT(user_id) AS total_users,
  ROUND(COUNT(user_id) * 100.0 / 33, 2) AS percentage
FROM user_tiers
GROUP BY activity_tier
ORDER BY total_users DESC;

#### Key Insight:
Over **51% of users** fall into the `Sedentary` or `Low Active` categories (averaging under 7,500 steps/day), while only **21.21%** reach the universally recommended 10,000 daily step target.
```
---

## 📊 Phase 5: Share

### Interactive Executive Dashboard
* **Tableau Public Dashboard:** [Bellabeat Executive Dashboard](https://public.tableau.com/authoring/BellabeatSmartDeviceDataAnalysis/Dashboard1/Bellabeat%20Executive%20Dashboard#1)

### Key Insights Summary
* **Daily Activity Deficit:** The average user logs **~7,638 steps per day**, falling below the recommended 10,000-step daily target.
* **Midweek Engagement Dip:** Activity peaks on **Tuesdays (~8,125 steps)** and **Saturdays (~8,153 steps)**, but declines significantly midweek (Wednesday–Thursday).
* **Calorie Burn Dynamics:** Strong positive linear relationship between steps and calories burned. High-calorie outliers with low step counts indicate non-step workouts (e.g., swimming, cycling, yoga).
* **Sleep Latency Gap:** Users average **~419 minutes (~7.0 hours)** of asleep time, but consistently spend additional time awake in bed prior to falling asleep.

---

## 💡 Phase 6: Act

### Strategic Marketing & Product Recommendations for Bellabeat

1. **Midweek Activity Nudges (Bellabeat App):**
   * *Action:* Implement personalized push notifications on Wednesday and Thursday afternoons encouraging users to complete short 15-minute walks or quick home workouts to counteract midweek activity slumps.
2. **Bedtime & Wind-Down Reminders (Leaf & Time Integration):**
   * *Action:* Introduce smart evening alerts paired with guided meditation audio tracks in the Bellabeat App to help users minimize sleep latency (time spent awake in bed).
3. **Holistic Exercise & Calorie Recognition:**
   * *Action:* Market non-step activity logging (yoga, swimming, strength training) directly through the Bellabeat App so users who don't hit 10,000 steps still receive credit for active calorie burn.
