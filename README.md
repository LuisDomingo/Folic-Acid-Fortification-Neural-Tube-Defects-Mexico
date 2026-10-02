# Folic-Acid-Fortification-Neural-Tube-Defects-Mexico
Code and analytical pipeline for the study: "Folic Acid Fortification and Neural Tube Defect Prevalence in Mexico: A Nationwide Population-Based Study, 2005-2021"

# Folic Acid Fortification and Neural Tube Defect Prevalence in Mexico (2005-2021)

This repository contains the R and Quarto scripts, along with the analytical pipeline used to evaluate the long-term impact of mandatory folic acid fortification on the prevalence of neural tube defects (NTDs) in Mexico. The analysis spans nearly two decades of national surveillance data.

## Overview

In Mexico, mandatory folic acid fortification of wheat and corn flour was implemented in 2008. This project processes nationwide data to calculate NTD prevalence across three policy periods: pre-fortification (2005–2007), partial fortification (2008–2009), and post-fortification (2010–2021). The code evaluates subtype-specific trends, geographic heterogeneity across 32 states and 4 major regions, and models the relationship between baseline prevalence and post-intervention reduction.

## Data Sources

The analysis integrates two official, publicly available Mexican health databases:

* **DGIS (General Directorate of Health Information):** Hospital discharges and vital statistics used to identify NTD cases (ICD-9 and ICD-10 codes), including hospital-documented pregnancy terminations.
* **INEGI (National Institute of Statistics and Geography):** Official statistics on registered live births and fetal deaths (stillbirths).

> **Epidemiological Note:** Nationwide comprehensive registries for early undocumented miscarriages do not exist in Mexico. Therefore, the denominator for prevalence calculations consists of the most robust baseline available: **total live births and fetal deaths** recorded by INEGI. To prevent underestimating the true NTD burden, the numerator includes all diagnosed NTD cases from live births, fetal deaths, and hospital-documented pregnancy terminations (legally coded as therapeutic abortions within the DGIS fetal death registry).

## Key Methodologies

* **Prevalence Calculation:** Rates are expressed per 10,000 births.
* **Confidence Intervals (Bootstrapping):** 95% Confidence Intervals for regional estimates and prevalence ratios were calculated using 5,000 bootstrap resamples to account for temporal variability within each policy period.
* **Trend Analysis:** The Cochran-Armitage test for trend in proportions (`prop.trend.test`) was applied to the pre-fortification period to assess underlying baseline trends prior to the mandatory policy implementation.
* **Regression Modeling & Selection:** Multiple models (Linear, Log-transformed Exponential, and Explicit Non-Linear Least Squares [NLS]) were compared to assess the relationship between baseline state-level prevalence and the absolute magnitude of reduction. The explicit exponential model ($y = a \cdot e^{bx}$) via `nls` was selected for its superior fit (highest Pseudo-R²) and convergence.
* **Spatial Visualization:** Regional and state-level prevalence ratios were mapped using spatial features (`sf`) and natural earth polygons.

## Repository Structure

* `09_Prevalencia_DTN_por_cada_diez_mil_nacimientos_barras_apiladas.Rmd`: Core script for calculating annual NTD prevalence by diagnostic category and generating stacked bar plots and connected scatter plots.
* `11_Pruebas_de_tendencia_lineal.Rmd`: Applies the Cochran-Armitage chi-square test to evaluate pre-fortification trends.
* `13_Mapeo_regional_de_prevalencia_1.0.qmd`: Calculates regional prevalence, applies 5,000-resample bootstrapping for Confidence Intervals, and maps regional prevalence ratios.
* `014_Tendencia_lineal_por_estado_1.1.qmd`: Compares and fits regression models (Linear vs. Exponential NLS) to evaluate the magnitude of reduction relative to baseline prevalence.
* `tb.DTN.curado.2021-2005.csv`: Curated dataset of NTD cases.
* `Nacimientos_en_Mexico_2005-2021_INEGI.csv` & `Base.de.datos.Obito.1985-2021.csv`: Annual live births and fetal deaths datasets.
* `tabla_razon_prevalencia_por_estados.csv`: Generated output of state-level metrics.

## Dependencies

The analysis was conducted using R. Required packages include:

* **Data Manipulation & Stats:** `tidyverse`, `plyr`, `dplyr`
* **Visualization:** `ggplot2`, `ggrepel`, `viridis`, `hrbrthemes`
* **Spatial Mapping:** `sf`, `rnaturalearth`, `rnaturalearthdata`, `ggspatial`
* **Formatting:** `kableExtra`, `rmarkdown`, `knitr`
