# LakeTwin-AI: Big Data Analytics and Machine Learning for Urban Lake Monitoring in Bengaluru

\<p align="center">
&#x20; \<strong>LAKE 2026 · 15th Biennial Lake Symposium\</strong>\<br>
&#x20; 18–20 November 2026 · Chanakya University, Bengaluru
\</p>

\<p align="center">   \<em>Conservation of Wetlands, Traditional Knowledge with Innovative Nature-Based Technologies for Viksit Bharat\</em> \</p>

\<p align="center">   \<strong>Research Project | Satellite Remote Sensing | Geospatial Analytics | Machine Learning\</strong> \</p>

---

## 1. Project Overview

**LakeTwin-AI** is a satellite-based urban lake monitoring research project that combines remote sensing, geospatial analysis, statistical methods, Big Data technologies, and exploratory machine learning to investigate seasonal surface-water patterns across mapped urban lake polygons in Bengaluru, India.

Urban lakes are important for groundwater recharge, biodiversity, flood management, and ecological balance. However, monitoring numerous lakes repeatedly through field surveys can be time-consuming and resource-intensive.

This project explores how multi-year satellite observations and computational analysis can support the screening and monitoring of urban lake environments.

The study uses **Copernicus Sentinel-2 Surface Reflectance Harmonized imagery** accessed through Google Earth Engine and analyses observations from 2020 to 2025.

### Research Question

> To what extent can multi-year satellite observations reveal consistent seasonal patterns in surface-water indicators across mapped urban lake polygons in Bengaluru, and how do data gaps affect the reliability of those patterns?

---

## 2. Research Objectives

1. Analyse seasonal variation in satellite-derived water indicators across mapped Bengaluru-area lake polygons from 2020 to 2025.
2. Examine whether observed seasonal patterns recur across years and individual lake polygons.
3. Assess missing satellite observations and their potential influence on the results.
4. Explore spectral groupings using unsupervised machine learning.
5. Demonstrate a computational workflow using Google Earth Engine, Python, and PySpark.

---

## 3. Study Area and Data

The study focuses on a Bengaluru-area geographical window:

- **Longitude:** 77.35°E–77.85°E
- **Latitude:** 12.75°N–13.25°N

The source lake-polygon collection contains 1,350 mapped polygons. Of these, **933 polygons intersect the selected study region** and were included in the analysis.

These polygons are not necessarily unique or currently active lakes. The dataset contains repeated names and should not be interpreted as a complete census of Bengaluru's lakes.

### Dataset Summary

| Parameter                          | Description                        |
| ---------------------------------- | ---------------------------------- |
| Study region                       | Bengaluru, Karnataka, India        |
| Satellite                          | Sentinel-2                         |
| Data collection                    | COPERNICUS/S2_SR_HARMONIZED        |
| Observation period                 | 2020–2025                          |
| Seasonal windows                   | 18                                 |
| Seasonal periods                   | Pre-monsoon, monsoon, post-monsoon |
| Qualifying satellite scenes        | 330                                |
| Analysed lake polygons             | 933                                |
| Spatial resolution                 | 20 metres                          |
| Seasonal records                   | 16,794                             |
| Complete records                   | 16,646                             |
| Records with missing spectral data | 148                                |

### Seasonal Classification

| Season       | Months           |
| ------------ | ---------------- |
| Pre-monsoon  | March–May        |
| Monsoon      | June–September   |
| Post-monsoon | October–December |

January and February were not included in the seasonal analysis.

---

## 4. Data Sources

### Satellite Imagery

**Copernicus Sentinel-2 Surface Reflectance Harmonized**

Sentinel-2 provides multispectral satellite observations used to calculate water-related and vegetation-related indices.

- Google Earth Engine dataset: `COPERNICUS/S2_SR_HARMONIZED`
- [Google Earth Engine Data Catalog](https://developers.google.com/earth-engine/datasets/catalog/COPERNICUS_S2_SR_HARMONIZED)
- [Copernicus Data Space Ecosystem](https://dataspace.copernicus.eu/)

### Geospatial Lake Polygons

The analysis uses a mapped lake-polygon dataset uploaded to Google Earth Engine for spatial screening.

Google Earth Engine asset:

`projects/winged-polygon-470717-b8/assets/LakeTwin_AI_Bengaluru_Lake_Polygons_Shapefile`

The polygon dataset is used to define the geographical units for the analysis. Its completeness, uniqueness, and current lake status should not be assumed without independent verification.

---

## 5. Methodology

The project follows a multi-stage geospatial and analytical workflow.

### Stage 1 – Satellite Data Processing

Google Earth Engine was used to access and process Sentinel-2 imagery.

The processing workflow includes:

- Filtering imagery by date and geographical region.
- Selecting qualifying satellite scenes.
- Masking clouds, cloud shadows, cirrus, and snow using the Scene Classification Layer.
- Generating seasonal median composites.
- Extracting spectral information for the mapped lake polygons.

A total of 330 qualifying Sentinel-2 scenes were processed across 18 seasonal windows.

### Stage 2 – Spectral Index Calculation

Three spectral indices were used.

**1. NDWI – Normalized Difference Water Index**

Used to identify water-related surface characteristics.

[
NDWI = \frac{Green-NIR}{Green+NIR}
]

Sentinel-2 bands: B3 and B8.

**2. MNDWI – Modified Normalized Difference Water Index**

Used to enhance water-feature identification, particularly in built-up environments.

[
MNDWI = \frac{Green-SWIR}{Green+SWIR}
]

Sentinel-2 bands: B3 and B11.

**3. NDVI – Normalized Difference Vegetation Index**

Used to identify vegetation-related characteristics.

[
NDVI = \frac{NIR-Red}{NIR+Red}
]

Sentinel-2 bands: B8 and B4.

### Stage 3 – Candidate-Water Fraction

For this study, candidate-water fraction is defined as the proportion of valid 20-metre pixels within a polygon having:

`MNDWI > 0`

This is an exploratory satellite-derived indicator. It is not a direct measurement of actual water area, water quality, water level, or lake health.

### Stage 4 – Data Cleaning and Statistical Analysis

Python was used for data-quality checks and statistical analysis.

The analysis included:

- Checking missing observations.
- Examining repeated lake names and polygon records.
- Checking spectral-value ranges.
- Comparing seasonal and yearly medians.
- Performing matched-polygon comparisons.
- Calculating bootstrap confidence intervals.
- Conducting sensitivity checks.

Missing spectral observations were retained as missing values rather than being automatically treated as zero.

### Stage 5 – Exploratory Machine Learning

K-Means clustering was used to explore recurring spectral patterns.

**Input features:**

- NDWI
- MNDWI
- NDVI

**Configuration:**

- Algorithm: K-Means
- Number of clusters: 3
- Standardisation: StandardScaler
- Complete observations: 16,646

The resulting groups represent different spectral characteristics. They are exploratory groups, not validated water classifications or lake-health categories.

The three-cluster solution achieved a silhouette score of approximately 0.41.

### Stage 6 – Big Data and Computational Analysis

The project uses:

- **Google Earth Engine:** Cloud-based satellite data access and geospatial processing.
- **Python and Pandas:** Data cleaning, statistical analysis, and visualisation.
- **PySpark:** DataFrame operations and aggregation.
- **Scikit-learn:** Feature standardisation and K-Means clustering.

A Pandas-versus-PySpark computational comparison was also performed using synthetic replicas of the original dataset at increasing data volumes.

This was a local Spark benchmark, not a multi-node distributed-cluster experiment.

---

## 6. Key Findings

### Seasonal Variation

The median candidate-water fraction across the study polygons was:

| Season       | Median candidate-water fraction |
| ------------ | ------------------------------: |
| Pre-monsoon  |                           0.008 |
| Monsoon      |                           0.017 |
| Post-monsoon |                           0.204 |

The post-monsoon median was the highest in all six study years.

Its 95% confidence interval was 0.189–0.220.

### Consistency Across Years

The post-monsoon median exceeded the monsoon median in all six years.

However, only **32% of the 796 polygons with complete observations across all six years** showed an increase over both other seasons in at least five of the six years.

This indicates that an overall seasonal trend does not necessarily represent the behaviour of every individual lake polygon.

### Missing Data

Out of 16,794 seasonal records:

- 16,646 had complete spectral observations.
- 148 records (approximately 0.9%) had missing spectral data.
- All 148 missing records occurred during the monsoon season.

### K-Means Clustering

The exploratory clustering identified three spectral groups:

| Group   | Approximate share | Spectral profile                            |
| ------- | ----------------: | ------------------------------------------- |
| Group 2 |               20% | Stronger water-related characteristics      |
| Group 1 |               38% | Stronger vegetation-related characteristics |
| Group 0 |               41% | Intermediate spectral characteristics       |

The clustering provides an exploratory view of spectral variation across the observations.

---

## 7. Repository Contents

The repository provides the research notebook and combined seasonal observation dataset.

| File                                                       | Description                                            |
| ---------------------------------------------------------- | ------------------------------------------------------ |
| `LakeTwin_AI_Complete_Analysis_Colab.ipynb`                | Google Colab notebook containing the analysis workflow |
| `LakeTwin_AI_Combined_Seasonal_Observations_2020_2025.csv` | Combined seasonal observation dataset                  |

Additional scripts, documentation, and reproducibility resources may be added as the project repository is expanded.

---

## 8. How to Access and Run the Notebook

The analysis notebook is designed for Google Colab.

### Step 1 – Open the Notebook

Open the notebook from this repository:

`LakeTwin_AI_Complete_Analysis_Colab.ipynb`

Alternatively, download it and upload it to Google Colab.

### Step 2 – Set Up Google Earth Engine

Some geospatial processing steps require Google Earth Engine access.

1. Sign in using a Google account.
2. Ensure that your account has access to Google Earth Engine.
3. Authenticate Earth Engine in the Colab environment when prompted.
4. Make sure the required lake-polygon asset is accessible to your account.

The referenced private or access-restricted Earth Engine assets may not be accessible to other users automatically. Appropriate asset access or an alternative copy of the polygon dataset is required to reproduce the complete workflow.

### Step 3 – Install and Import Dependencies

The notebook uses Python-based geospatial and data-analysis libraries. Depending on the notebook environment, required packages include:

- Earth Engine Python API
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- PySpark

Use the notebook's installation and import cells to configure the environment.

### Step 4 – Execute the Notebook

Run the notebook cells in their intended order.

The workflow covers satellite-data processing, seasonal analysis, data cleaning, statistical comparisons, clustering, and visualisation.

**Note:** Exact reproducibility depends on access to the required Google Earth Engine assets and the original input data.

---

## 9. Limitations

This research is an exploratory satellite-based screening study. The following limitations should be considered:

1. **No independent ground validation:** The candidate-water indicator has not been independently validated against field measurements or reference water masks.
2. **Threshold selection:** The MNDWI > 0 threshold is an operational screening threshold and has not been independently optimised.
3. **Mixed pixels:** Small lake polygons may contain relatively few 20-metre pixels, which can affect spectral estimates.
4. **Polygon quality:** Mapped polygons are not necessarily unique, current, or verified active lakes.
5. **Observation gaps:** Missing satellite observations are concentrated in the monsoon season.
6. **Study duration:** The analysis covers six years, from 2020 to 2025.
7. **No predictive model:** K-Means is used for exploratory clustering, not future lake-condition prediction.
8. **Computational benchmark:** The PySpark comparison was conducted locally and does not demonstrate multi-node distributed scalability.

The findings should therefore be interpreted as satellite-derived screening indicators rather than definitive environmental measurements.

---

## 10. Future Work

Potential extensions include:

- Validation using independent water masks and field observations.
- Integration of rainfall and other environmental datasets.
- Evaluation of alternative water-index thresholds.
- Improved handling of mixed pixels and small lake polygons.
- Extension to additional urban lake datasets and geographical regions.
- Development of a more comprehensive lake-monitoring framework.
- Exploration of time-series machine learning and predictive modelling after suitable validation data become available.

---

## 11. Research Relevance

LakeTwin-AI contributes to the exploration of technology-assisted urban lake monitoring through:

- Remote sensing and GIS-based spatial analysis.
- Seasonal satellite observation and environmental screening.
- Cloud-based geospatial data processing.
- Big Data-oriented analytical workflows.
- Exploratory machine learning for spectral pattern analysis.

The project aims to support the prioritisation of lakes for further investigation and field assessment.

---

## 12. References

1. McFeeters, S. K. (1996). The use of the Normalized Difference Water Index (NDWI) in the delineation of open water features. *International Journal of Remote Sensing*, 17(7), 1425–1432.

2. Xu, H. (2006). Modification of normalised difference water index (NDWI) to enhance open water features in remotely sensed imagery. *International Journal of Remote Sensing*, 27(14), 3025–3033.

3. Rouse, J. W. Jr., Haas, R. H., Schell, J. A., & Deering, D. W. (1974). Monitoring vegetation systems in the Great Plains with ERTS. *Proceedings of the Third Earth Resources Technology Satellite-1 Symposium*, NASA.

4. Drusch, M., et al. (2012). Sentinel-2: ESA's optical high-resolution mission for GMES operational services. *Remote Sensing of Environment*, 120, 25–36.

5. Gorelick, N., et al. (2017). Google Earth Engine: Planetary-scale geospatial analysis for everyone. *Remote Sensing of Environment*, 202, 18–27.

---

## 13. Acknowledgements

The author sincerely acknowledges the **School of Engineering, Chanakya University**, for providing the opportunity, academic support, and encouragement to present this research at LAKE 2026.

The author also acknowledges the Copernicus Sentinel-2 programme and Google Earth Engine for providing access to satellite observations and geospatial processing resources.

---

## 14. Author

**Sonali Gupta**
BCA, School of Engineering
Chanakya University, Bengaluru, India

Research presentation: LAKE 2026 – 15th Biennial Lake Symposium

---

\<p align="center">
&#x20; \<strong>LakeTwin-AI\</strong>\<br>
&#x20; Satellite-based seasonal monitoring of urban lake environments.
\</p>
