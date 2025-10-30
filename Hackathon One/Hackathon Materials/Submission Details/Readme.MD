# 🧭 AI4S Hackathon — Final Submission Guide  
**Repository:** `/Final_Submission_Guide`  
**Version:** v1.0 | Updated: October 2025  

This README provides a **step-by-step guide** on how participants should prepare and submit their **final hackathon deliverables** for the **AI4S Hackathon**.  
It explains how to create your team’s GitHub Organization, organize your repositories, upload your presentation slides, and ensure your submission is complete and ready for judging.

---

## 📘 Overview

The AI4S Hackathon emphasizes **transparency**, **reproducibility**, and **collaboration**.  
All final submissions must be hosted on GitHub for easy access, version control, and long-term visibility.  
Each team will submit their work as a **GitHub Organization** named after their team.  

Your GitHub Organization will contain:
1. Repositories for your **code**, **data**, and **documentation**  
2. A **team directory** listing all members  
3. A **downloadable link** to your final presentation slides  

---

## 📂 Step 1: Create a GitHub Organization

### 1️⃣ Navigate to the Organization Creation Page
- Go to [https://github.com/organizations/new](https://github.com/organizations/new)  
- Choose the **Free Plan**.  
- Name your organization after your **team name** (e.g., `AgriVisionAI`, `CropSense`, `TeamMaizeTech`).  

### 2️⃣ Add Members
Once created:
- Go to your organization’s page → **People → Invite Member**  
- Add all your teammates using their GitHub usernames.  
- Assign everyone the **Member** role.  

### ✅ Naming Convention
Use consistent formatting:  

---

## 🧩 Step 2: Create Required Repositories

Inside your organization, create the following **four repositories**:

| Repository Name | Purpose |
|-----------------|----------|
| `project-code` | Contains all code, notebooks, and scripts |
| `project-data` | Contains small sample datasets, schema, and metadata |
| `project-docs` | Contains the technical narrative, reports, and additional notes |
| `project-slides` | Contains your final presentation slides |

Each repository should have a clear **README.md** file explaining its contents.

---

## 🧱 Step 3: Set Up Repository Structure

### Example Directory Layout

```bash
/project-code
├── README.md
├── requirements.txt
├── notebooks/
│   ├── 01_data_preprocessing.ipynb
│   ├── 02_feature_engineering.ipynb
│   └── 03_model_training.ipynb
├── src/
│   ├── model.py
│   ├── pipeline.py
│   └── utils.py
└── tests/
    └── test_pipeline.py

/project-docs
├── TECHNICAL_NARRATIVE.pdf
├── METHODOLOGY.md
├── LICENSE
└── README.md

/project-slides
├── slides.pdf
└── slides.pptx

# TeamName — AI4S Hackathon Submission

## Project Title
AI-Powered Credit Scoring for Inclusive Agri-Finance

## Summary
A machine-learning system that predicts farmer creditworthiness using synthetic and open agricultural data.

## Team Members
- Daisy Jelagat — Product Lead (@daisyj)
- John Doe — Data Scientist (@johndoe)
- Alice Mwangi — Finance Specialist (@alicem)

## Quick Links
- [Code Repository](https://github.com/TeamName/project-code)
- [Technical Narrative](https://github.com/TeamName/project-docs/TECHNICAL_NARRATIVE.pdf)
- [Slides (Download)](https://github.com/TeamName/project-slides/raw/main/slides.pdf)

## Contact
teamname@ai4s-hackathon.org
