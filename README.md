# ClearSky: AI-Powered Methane Plume Detection

**Team:** Geo-Intelligence  
**Project for:** AI Co-Innovation Program

<div align="center">

[![Status](https://img.shields.io/badge/Status-Proof--of--Concept-green)](https://www.rdia.gov.sa/)
[![Technology](https://img.shields.io/badge/Data-EMIT%20Hyperspectral-blue)](https://earth.jpl.nasa.gov/emit/)
[![Models](https://img.shields.io/badge/Models-STARCOP%20%7C%20Project--Eucalyptus-orange)]()

</div>

---

## 1. Project Overview

**ClearSky** is an advanced methane detection system that leverages pre-trained AI models to identify and map methane plumes from NASA's EMIT hyperspectral satellite imagery. Our mission is to *"make the invisible visible for a safer, cleaner future."*

A primary application for ClearSky is the proactive monitoring of Saudi Arabia's industrial heartlands, such as the city of **Al Jubail**. By providing regular, wide-area surveillance of critical energy and chemical infrastructure, our solution enables rapid leak detection, enhancing both operational safety and environmental stewardship in line with the Kingdom's strategic goals.

This project demonstrates a robust, resource-efficient approach by creating an **ensemble** of specialized AI models to achieve higher confidence and more reliable detection than any single model could alone.

## 2. The Challenge

Methane (CH₄) is a potent greenhouse gas, invisible and odorless, making it notoriously difficult to detect at low concentrations. Traditional monitoring methods are often localized, costly, or lack the scalability required for wide-area surveillance over dense industrial zones. The challenge is to develop a system that can:
-   Detect methane plumes over large, complex geographical areas.
-   Operate cost-effectively without extensive ground-based infrastructure.
-   Provide actionable data for rapid response to leaks.

## 3. Our Solution: An Ensemble Approach

Given the significant hardware constraints of working with hyperspectral data—where a single satellite image can exceed 2GB—fine-tuning or training new models from scratch is often infeasible without access to a large-scale GPU cluster and terabytes of storage.

To overcome this, we adopted a **robust and resource-efficient strategy**: leveraging existing, state-of-the-art, pre-trained models in an ensemble. This "zero-shot" approach allows us to apply powerful, specialized algorithms directly to new EMIT data, combining their strengths to produce a superior, high-confidence result.

Our pipeline utilizes two primary models:

1.  **STARCOP (Semantic Tracking of Atmospheric Methane Plumes):** A model developed by the University of Valencia, trained on AVIRIS data. It excels at semantic segmentation and is highly effective at identifying the characteristic shapes and spectral signatures of methane plumes.
2.  **Project-Eucalyptus:** An open-source model designed for probabilistic methane detection. It provides a likelihood score, offering a different, complementary perspective on potential emissions.

By aggregating the outputs from these models across multiple satellite passes, we build a final confidence map that is more reliable and less prone to false positives.

## 4. Workflow & Methodology

Our process is designed for scalability and is implemented entirely within a single, reproducible notebook.

*(See the diagram below for a visual representation of the workflow.)*

1.  **Data Ingestion:** Download specified EMIT L1B radiance granules.
2.  **Parallel Inference:** Process each granule independently through both the STARCOP and Project-Eucalyptus models to generate initial prediction masks.
3.  **Georeferencing:** Align all prediction masks to a common Coordinate Reference System (CRS) to ensure spatial consistency.
4.  **Aggregation & Ensembling:** Combine the aligned masks from all models and all satellite passes. The aggregated map reflects both the average predicted signal and the number of observations (flyovers).
5.  **Confidence Scoring:** Generate a final confidence score by weighting the mean signal by the observation count. Areas with consistent detections across multiple passes and models receive the highest confidence scores.
6.  **Visualization & Export:** Produce an interactive HTML map (using Folium) and a compressed GeoTIFF file, providing an intuitive and shareable final product for analysis.

## 5. How to Use This Notebook

1.  **Setup:** The initial cells install all required dependencies, including `STARCOP` and `georeader`.
2.  **Configuration:** The `granule_names` list contains the EMIT scenes to be processed. You can modify this list to analyze different areas or dates.
3.  **Execution:** Run the notebook cells sequentially. The notebook will:
    -   Download each granule.
    -   Run predictions using STARCOP.
    -   Run predictions using Project-Eucalyptus.
    -   (Final Cells) Aggregate the results into a single confidence map.
4.  **Output:** The final output will be an interactive map (`methane_detection_map_popup.html`) and a GeoTIFF file (`final_methane_confidence_map_compressed.tif`) saved in the notebook's output directory.

## 6. Technology Stack

-   **Core Libraries:** PyTorch, Rasterio, Geopandas, Xarray
-   **Modeling:** STARCOP, Project-Eucalyptus
-   **Geospatial Tools:** Georeader, Folium, Pyproj
-   **Environment:** Kaggle Notebooks

## 7. Future Work

-   **Expand the Ensemble:** Integrate additional pre-trained models (e.g., from other research institutions) to further improve detection robustness.
-   **Automated Alerting:** Develop a system to automatically notify stakeholders when a high-confidence plume is detected in a new satellite pass.
-   **Quantitative Estimation:** Extend the models to estimate the volume or flow rate of detected methane leaks.
