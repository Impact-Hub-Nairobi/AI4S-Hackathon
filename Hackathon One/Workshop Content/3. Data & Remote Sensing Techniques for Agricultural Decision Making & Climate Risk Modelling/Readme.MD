# 🌍 Workshop 3: From Pixels to Use Cases and Policy  
### AI4S Hackathon – Data & Remote Sensing Techniques for Agricultural Decision-Making and Climate Risk Modelling  

---

## 🧭 Overview  

This workshop demonstrates how to transform raw satellite and climate data into actionable, farm-level insights and machine learning–ready features. Participants will walk through a complete end-to-end geospatial data pipeline — from defining areas of interest (AOI) to generating features that support yield prediction, drought early warning, and agricultural policy-making.  

By the end, participants will understand how Earth observation (EO) data can drive smarter agricultural and climate decisions through machine learning and geospatial analytics.

---

## 📚 Workshop Flow  

### Step-by-Step Pipeline  
1. Define Area of Interest (AOI) & import farm polygons.  
2. Pre-process Sentinel-2 imagery (cloud masking & composites).  
3. Calculate vegetation and water indices (NDVI, NDMI, NDWI).  
4. Construct climate anomalies using CHIRPS rainfall data.  
5. Extract and summarize features over farm polygons.  
6. Use embeddings for rapid land-use mapping.  

**Outcome:** Generate evidence for:  
- Crop yield modelling.  
- Drought and flood early warning.  
- Policy targeting and agricultural decision-making.  

---

## ⚙️ Tools & Data Stack  

### 🧩 Compute and Libraries  
- **Google Earth Engine (GEE):** Core for large-scale geospatial processing.  
- **Geemap:** Python bridge to visualize and interact with GEE in notebooks.  
- **Pandas / GeoPandas:** Manage tabular and spatial data efficiently.  
- **Matplotlib:** Quick visualizations for time-series data.  
- **Scikit-learn:** Apply ML tasks like clustering (K-means).  

### 🛰️ Imagery and Climate Data  
- **Sentinel-2 L2A:** 10m optical imagery (Red, NIR, SWIR bands + QA).  
- **CHIRPS Daily:** Rainfall data (5 km resolution) for seasonal totals & anomalies.  
- **ERA5-Land Monthly:** Temperature and meteorological variables.  
- **Google Alpha Earth Embeddings V1:** Precomputed spectral–temporal vectors for unsupervised classification.  

### 🌾 Ground Data  
- AOIs and polygons from ECAAS or project-level datasets.  
- Ancillary information such as crop type (e.g., maize) and historical yield.  

---

## 🧹 Pre-Processing Choices  

- **Temporal Window:** Align analysis with local crop calendars (e.g., May–August for maize in Uasin Gishu).  
- **Cloud Masking:** Use QA60/SCL bands to remove cloudy pixels; generate median composites.  
- **Spatial Coverage:** Restrict computations to intersected AOIs and farm polygons.  

**Goal:** Clean, efficient, and reliable datasets for modeling.

---

## 🌿 Vegetation & Water Indices  

| Index | Formula | Interpretation | Purpose |
|--------|-----------|----------------|-----------|
| **NDVI** | (NIR − Red) / (NIR + Red) | Vegetation health & biomass | Crop vigor |
| **NDMI** | (NIR − SWIR) / (NIR + SWIR) | Plant moisture content | Water stress |
| **NDWI** | (Green − NIR) / (Green + NIR) | Surface water presence | Flooding / irrigation |

Together, these indices provide a holistic view of vegetation conditions, moisture stress, and water distribution.  

---

## 🌦️ Climate Anomaly Construction  

1. **Current Season Total:** Aggregate CHIRPS rainfall for the growing period.  
2. **Baseline:** Compute long-term (e.g., 2015–2020) average rainfall.  
3. **Anomaly:** `Current Total − Baseline Mean`  

- **Positive anomaly:** Wetter-than-average season.  
- **Negative anomaly:** Drier-than-average season.  

> Example: A negative rainfall anomaly + falling NDMI indicates early drought stress.  

---

## 📊 Feature Extraction Over Polygons  

Convert pixel-level data into structured tabular form.  

- Use `reduceRegions()` in GEE to summarize NDVI, NDMI, rainfall anomaly, etc., per polygon.  
- Generate tidy tables with:
  - `date`
  - `farm_ID`
  - `NDVI_mean`
  - `NDMI_mean`
  - `rainfall_anomaly`

**Smoothing:** Apply rolling mean to reduce noise.  

**Quick Interpretation:**  
- Rising NDVI + Stable NDMI → Healthy crops.  
- Falling NDMI → Water stress.  
- Low NDVI + High NDWI → Waterlogging.  

---

## 🧠 Embeddings Demo  

**Alpha Earth Embeddings** represent pixels as feature vectors for similarity and clustering.  

- **Cosine Similarity Map:** Identify similar vegetation patterns across AOIs.  
- **K-Means Clustering:** Group pixels into land-cover types (e.g., cropland, forest, built-up).  

> Enables fast, unsupervised mapping without raw imagery processing.  

---

## 🤖 ML Framing for Yield Prediction  

| Component | Description |
|------------|-------------|
| **Target (Y):** | Crop yield (Mt/ha) or yield category (Low, Medium, High). |
| **Features (X):** | Seasonal max NDVI, avg NDMI, cumulative rainfall, anomaly index. |
| **Validation:** | Group K-Fold CV to generalize across regions/years. |
| **Metrics:** | RMSE (regression), F1-Score (classification). |

---

## 🧭 Policy and Decision-Making  

Outputs inform practical, policy-level actions:  
- **Early Warnings:** NDMI-based drought alerts.  
- **Insurance:** Satellite-derived, auditable climate indices.  
- **Targeted Interventions:** Pinpoint resource allocation.  
- **Environmental Monitoring:** Detect deforestation and land-use change.  

---

## ⚖️ Governance, Ethics & Risk  

> *As discussed in our first webinar (Sharon’s session)*  

Using geospatial and climate data responsibly is critical.  

- **Data Sovereignty:** Respect ownership and consent for farm-level data.  
- **Privacy:** Avoid publishing identifiable maps.  
- **Bias & Coverage:** Account for cloud gaps and mixed pixels in smallholder regions.  
- **Uncertainty:** Communicate confidence levels and model limitations transparently.  

---

## 🧰 Why This Matters for the AI4S Hackathon  

Responsible, data-driven insights are the backbone of agricultural innovation.  
This workflow empowers participants to **embed geospatial intelligence** into their prototypes, helping farmers, policymakers, and agribusinesses make **real-time, evidence-based decisions** that strengthen food systems and resilience to climate risk.  

📺 **Recording:** [Responsible AI for Agriculture Workshop – Watch Here](#)  

---

## 🏁 Contributors  
Facilitator: *[Name of Facilitator]*  
Prepared for: **AI4S Hackathon – Impact Hub Nairobi**  
Consultant: *Daisy Jelagat*  

---