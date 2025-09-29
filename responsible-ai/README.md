# 📊 FAOSTAT Access Manual  
**Food and Agriculture Organization (FAO) Statistics Division**

[FAOSTAT](https://www.fao.org/faostat/en/#data) is the world’s largest database on food and agriculture, providing **over 245 countries and territories with 20,000+ time series datasets** on production, trade, food security, emissions, and more.

This manual guides you through accessing and using FAOSTAT data for AI4S projects.

---

## 1. 🔑 No Login Required
FAOSTAT is open access. You do **not** need an account to browse or download data.  

---

## 2. 🌍 Navigating the FAOSTAT Portal
1. Go to **[FAOSTAT](https://www.fao.org/faostat/en/#data)**.  
2. On the **left menu**, select a **Domain** (e.g., Crops, Trade, Climate, Food Security).  
3. Inside each domain, choose a **Dataset** (e.g., *Production > Crops and livestock products*).  
4. You will see a dashboard with filters (Country, Item, Year, Element).  

---

## 3. 📑 Selecting Data
- **Country/Region** → e.g., Kenya, East Africa, Sub-Saharan Africa  
- **Item** → e.g., Maize, Coffee, Cocoa, Sorghum  
- **Element** → e.g., Production Quantity, Yield, Area harvested  
- **Year** → Select single or multiple years (1961 → present)  

⚡ Pro tip: Use the **multi-select checkboxes** to pull multiple indicators at once.  

---

## 4. 📥 Downloading Data
Once you’ve applied filters:  
1. Click **“Download Data”** (top right corner).  
2. Choose your format:  
   - `.CSV` (recommended for Python/Excel)  
   - `.XLS` (Excel)  
   - `.JSON` (for APIs)  

Example file name:  
`Production_Crops_Livestock_2024-09-29.csv`

---

## 5. ⚙️ API Access
FAOSTAT also provides an API for **programmatic access**:  

**Base URL:**  
```

[https://fenixservices.fao.org/faostat/api/v1/en/](https://fenixservices.fao.org/faostat/api/v1/en/)

```

**Structure:**  
```

/{domain}/{dataset}?area={country_code}&item={item_code}&element={element_code}&year={year}

```

**Example:** Get maize production for Kenya in 2020:  
```

[https://fenixservices.fao.org/faostat/api/v1/en/QCL/production?area=404&item=56&element=5510&year=2020](https://fenixservices.fao.org/faostat/api/v1/en/QCL/production?area=404&item=56&element=5510&year=2020)

````

- `QCL` = Crops and Livestock Production domain  
- `area=404` = Kenya  
- `item=56` = Maize  
- `element=5510` = Production (tonnes)  

📖 Full API docs: [FAOSTAT API Guide](https://www.fao.org/faostat/en/#data/API)

---

## 6. 🐍 Using in Python
Example: Download production data using `requests` and `pandas`.

```python
import requests
import pandas as pd

url = "https://fenixservices.fao.org/faostat/api/v1/en/QCL/production?area=404&item=56&element=5510"
response = requests.get(url)

data = response.json()
df = pd.DataFrame(data['data'])

print(df.head())
````

---

## 7. 🔒 Licensing & Attribution

FAOSTAT data is **open access** under FAO’s data policy.
✅ Always include attribution:

> “Source: FAO, FAOSTAT database. Accessed [date].”

---

## 8. 🚨 Common Pitfalls

* Large queries (>50,000 rows) may take long → filter by fewer countries/items/years.
* Country codes follow **FAO coding system**, not ISO (check FAOSTAT’s country codes list).
* Some datasets update annually; always check the **“Last update”** timestamp on each dataset page.

---

## ✅ Quick Checklist

* [ ] Identify the dataset domain (Crops, Trade, Emissions, etc.)
* [ ] Select country, item, element, year
* [ ] Download as CSV/XLS or query via API
* [ ] Cite FAOSTAT in your project

---

## 📎 Useful Links

* FAOSTAT Home → [https://www.fao.org/faostat/en/#data](https://www.fao.org/faostat/en/#data)
* API Documentation → [https://www.fao.org/faostat/en/#data/API](https://www.fao.org/faostat/en/#data/API)
* Country & Item Codes → [https://www.fao.org/faostat/en/#definitions](https://www.fao.org/faostat/en/#definitions)

---