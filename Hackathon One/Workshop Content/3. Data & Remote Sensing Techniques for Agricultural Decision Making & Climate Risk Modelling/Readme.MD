# 🌍 Workshop 3: Data & Remote Sensing Techniques for Agricultural Decision-Making and Climate Risk Modelling  

---

## 🧭 Overview  

This workshop demonstrates how to transform raw satellite and climate data into actionable, farm-level insights and machine learning–ready features. Participants will walk through a complete end-to-end geospatial data pipeline — from defining areas of interest (AOI) to generating features that support yield prediction, drought early warning, and agricultural policy-making.  

By the end, participants will understand how Earth observation (EO) data can drive smarter agricultural and climate decisions through machine learning and geospatial analytics.

---

## 📚 From Pixels to Use Cases and Policy

### Step-by-Step Pipeline  
1. **Define Area of Interest (AOI) & import farm polygons**
   - **What to do:** Select the geographic extent to analyze (district/county) and load farm polygons (GeoJSON / WKT / Shapefile). Use a coarse AOI first (admin boundary) then intersect with farm polygons to limit processing cost.
   - **Why it matters:** Restricting processing to the intersected AOI + farm polygons saves compute and ensures outputs are farm-focused.
   - **Practicals:** Validate polygon geometry (no self-intersections), ensure consistent CRS (use EPSG:4326 or match GEE's projection), and include `farm_id`, `crop_type`, and any available `yield` or `management` attributes.
   - **Pitfalls:** Tiny or incorrect polygons (smaller than pixel size) produce mixed-pixel issues; consider buffering very small farms or aggregating to village-level units.

2. **Pre-process Sentinel-2 imagery (cloud masking, composites)**
   - **What to do:** Filter Sentinel-2 L2A by date range and AOI; apply cloud & shadow masks; create temporal composites (e.g., median/monthly).
   - **Recommended approach:** Use Scene Classification Layer (SCL) or QA60 band to mask clouds & shadows; generate cloud-free composites using median or percentile reducers.
   - **Parameters:** Temporal window aligned to crop calendar (e.g., monthly or key phenological windows); choose composite method (median reduces haze/noise; 75th percentile can preserve vegetation signal).
   - **Tools / functions:** GEE `filterDate()`, `map()` with cloud mask function, `reduce()` or `median()` compositing, and `.clip()` to AOI.
   - **Pitfalls:** Over-aggressive masking removes valid pixels in persistently cloudy areas — consider longer windows or gap-filling methods.

3. **Calculate key Vegetation Indices (NDVI, NDMI, NDWI)**
   - **What to do:** Compute indices pixel-wise from band values and create index time-series.
   - **Bands to use:**  
     - NDVI: (B8 - B4) / (B8 + B4) — NIR (B8), Red (B4)  
     - NDMI: (B8 - B11) / (B8 + B11) — NIR (B8), SWIR1 (B11)  
     - NDWI: (B3 - B8) / (B3 + B8) — Green (B3), NIR (B8)
   - **Temporal handling:** Store index images per composite period (e.g., monthly NDVI) and maintain consistent date stamps for alignment with climate data.
   - **Quality control:** Clip indices to valid ranges, mask invalid or low-quality pixels, and visualize histograms to detect anomalies.
   - **Pitfalls:** Band numbering differs across sensors — confirm band names and scaling (Sentinel-2 uses reflectance * 1e4 in some APIs).

4. **Construct climate anomalies from CHIRPS rainfall data**
   - **What to do:** Aggregate daily CHIRPS into seasonal totals, compute long-term baseline means, and calculate anomalies (current season − baseline).
   - **Baseline choices:** Use a multi-year baseline (e.g., 5–10 years). Ensure the baseline window is climatologically relevant and excludes major regime shifts if possible.
   - **Aggregation:** Sum daily rainfall across the same phenological window used for indices (e.g., May–Aug). Optionally compute standardized anomalies (z-scores) for comparability.
   - **Interpretation:** Negative anomalies indicate drought risk; combine anomaly flags with NDMI declines for robust stress detection.
   - **Pitfalls:** CHIRPS resolution (~5km) may not capture micro-climate variation; interpret with caution for heterogeneous landscapes.

5. **Extract features for each farm polygon**
   - **What to do:** Reduce raster/index layers over each polygon to compute summary statistics per farm and time step.
   - **Common stats:** mean, median, max, min, standard deviation, percentiles (10th, 90th), and counts of valid pixels. Also compute phenological metrics (peak NDVI date, area under curve).
   - **Implementation:** Use GEE `reduceRegions()` or local tools (`rasterstats`, `zonal_stats`) to produce a tidy table with columns like `date`, `farm_id`, `NDVI_mean`, `NDMI_mean`, `rainfall_anomaly`.
   - **Smoothing & feature engineering:** Apply rolling-window smoothing (e.g., 15-day) and derive engineered features such as `ndvi_max`, `ndmi_min_during_flowering`, `days_below_stress_threshold`.
   - **Pitfalls:** Mixed pixels and edge effects can bias small farms — consider excluding perimeter pixels or use median instead of mean for robustness.

6. **Explore embeddings for quick land use mapping**
   - **What to do:** Use pre-computed pixel embeddings (e.g., Alpha Earth Embeddings) to perform similarity searches or unsupervised classification.
   - **Demos:**  
     - **Cosine similarity:** identify pixels similar to a reference (e.g., healthy maize) to map like-areas.  
     - **Clustering (K-means):** cluster pixel embeddings to derive coarse LULC classes without labeled training data.
   - **Benefits:** Fast, scalable LULC proxies and similarity-based filtering for anomaly detection.
   - **Pitfalls:** Embeddings depend on the pretraining domain — validate clusters with ground truth or high-confidence samples.

7. **Generate machine-learning-ready feature tables**
   - **What to do:** Consolidate temporal and static features into a single dataset for modeling (one row per farm-per-season or farm-per-date).
   - **Feature set examples:** seasonal max NDVI, mean NDMI during critical window, cumulative rainfall, rainfall anomaly, embedding-cluster id, farm area, soil type, historical yields, and management variables.
   - **Data hygiene:** Handle missing values (impute or flag), normalize or standardize features, and encode categorical variables (one-hot or target encoding).
   - **Pitfalls:** Leakage (using future info) and non-stationarity across years/regions — use proper temporal cross-validation strategies.

8. **Modeling & validation framing**
   - **What to do:** Choose modeling approach (regression/classification) and validation scheme that reflects real-world use (e.g., predict next season’s yield).
   - **Validation best-practices:** Use Group K-Fold or spatial cross-validation to avoid over-optimistic results when farms are spatially clustered.
   - **Metrics:** RMSE or MAE for regression; F1-score, precision/recall for classification. Report uncertainty bounds where possible.
   - **Pitfalls:** Overfitting to local idiosyncrasies and poor generalization across seasons/regions without proper validation.

9. **Interpretation & decision-rule generation**
   - **What to do:** Translate model outputs and index trends into actionable rules (e.g., drought alert if `rainfall_anomaly < -25%` and `NDMI_drop > threshold`).
   - **Decision supports:** Create thresholds, confidence levels and human-in-the-loop checks before automated actions (e.g., insurance payouts or advisories).
   - **Pitfalls:** Single-indicator triggers can be noisy; prefer multi-factor triggers with explainability and appeal pathways.

10. **Policy & reporting outputs**
    - **What to do:** Aggregate outputs to district or county levels for policy use, create map products and dashboards with clear caveats and confidence intervals.
    - **Ethics & governance:** Ensure consent and data agreements for farm-level data; anonymize or aggregate when sharing publicly.
    - **Pitfalls:** Public release of farm-level maps can breach privacy — aggregate appropriately and consult data owners.

**General Tips & Best Practices**
- **Align temporal windows** with agronomic calendars (planting, flowering, harvest).  
- **Prefer robust statistics** (median, percentiles) for smallholder contexts with mixed pixels.  
- **Document all preprocessing choices** (masks, composites, thresholds) in code and README.  
- **Communicate uncertainty** clearly — use probability language and confidence bands, not absolute claims.  
- **Validate with ground truth** where possible (farmer surveys, yield plots) and log limitations.


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