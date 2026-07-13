# Shetland Islands Dominant Marine Biotope Map (SI-DMBM)

Automated MaxEnt ecological niche modelling workflow for marine benthic biotope habitat suitability mapping in Shetland's waters (12nm).

## Overview
The Shetland Islands Dominant Marine Biotope Map (SI-DMBM) is a predictive ecological niche model covering 28 biotopes, first developed in 2023. An adaptive management approach ensures annual refinements incorporating the latest environmental and biotope occurrence data.
**2025 Update:** Transitioned from MaxEnt GUI (v3.4.4) to a fully scripted R workflow for enhanced reproducibility, automation, and integration with modern spatial analysis frameworks (Published July 2026).
**Project Website:** [UHI Shetland Marine Spatial Planning] (https://www.shetland.uhi.ac.uk/research/marine-spatial-planning/research/seabed-habitats/shetland-islands-dominant-marine-biotope-map/)

## Features
- **Automated MaxEnt modeling** with bootstrap replicates
- **Comprehensive validation metrics**: AUC, TSS, Kappa, Boyce Index
- **Ensemble predictions** from multiple model runs
- **Standardized outputs**: Habitat suitability rasters (.tif, .asc), validation statistics, variable importance
- **Reproducible workflow** with automatic citation generation

## Installation

### Prerequisites
- R ≥ 4.0.0
- Java Runtime Environment (JRE) for MaxEnt

### Required R Packages
```r
install.packages(c("terra", "sf", "ecospat", "predicts", "pROC", "dismo", "rJava"))
```

## Quick Start

### 1. Directory Structure
Organize your data following this structure:
```
project/
├── data/
│   ├── species/          # Biotope occurrence CSV files
│   ├── environmental/    # Environmental raster layers (.tif)
│   └── spatial/          # Land boundary shapefiles
├── output/               # Model outputs (auto-created)
├── scripts/              # R scripts
└── docs/                 # Documentation
```

### 2. Input Data Format
**Species CSV** must contain:
- `Biotope`: Biotope classification code - This can be species instead of biotope
- `LATITUDE`: Decimal degrees (WGS84)
- `LONGITUDE`: Decimal degrees (WGS84)
  
**Environmental layers**: GeoTIFF format, matching CRS and extent

### 3. Configure and Run
Edit the configuration section (lines 28-50) in `maxent_sdm.R`:
```r
CONFIG <- list(
  base_dir = "C:/Users/YourUsername/Projects/SDM",  # Your project path
  species_csv = "your_biotope.csv",                  # Your species file
  n_replicates = 10,                                 # Number of replicates
  # ... other parameters
)
```

Then run:
```r
source("scripts/maxent_sdm.R")
```

### 4. Outputs
Results are saved to `output/[species_name]/`:
- `*_prediction.tif/asc` - Habitat suitability rasters
- `*_occurrence_map.png` - Species occurrence visualization
- `*_variable_importance.csv` - Environmental variable contributions
- `*_validation_summary.csv` - Model performance metrics
- `*_citations.txt` - Software citations for methods section

## Model Configuration
Key parameters (adjust in configuration section):
| Parameter | Default | Description |
|-----------|---------|-------------|
| `n_replicates` | 10 | Bootstrap replicates (≥10 recommended) |
| `n_background` | 10000 | Pseudo-absence points |
| `train_test_split` | 0.75 | Training data proportion |
| `max_iterations` | 5000 | MaxEnt convergence iterations |
| `beta_multiplier` | 1 | Regularization parameter |

## Validation Metrics
Models are evaluated using four complementary metrics:
- **AUC** (Area Under ROC Curve): 0.7-0.9 = good, >0.9 = excellent
- **TSS** (True Skill Statistic): >0.6 = good performance
- **Kappa**: >0.6 = acceptable
- **Boyce Index**: Presence-only validation, positive values indicate consistency

## Methodology

### Previous Approach (2023-2024)
MaxEnt models executed through GUI (v3.4.4)
### Current Approach (2025+)
Scripted workflow in R with:
- Programmatic MaxEnt implementation (`predicts` package)
- Automated data quality control (NA removal, duplicate filtering)
- Ensemble predictions from multiple replicates
- Enhanced validation including Boyce Index
- Full reproducibility and version control

See associated report citation for detailed methods.

## Citation
If you use this code, please cite:
Riley, T.G., Gillanders, S. & Shucksmith, R. (2026). Shetland Islands Dominant Marine Biotope Map (2025 update). UHI Shetland report. p31. 

https://github.com/ShetlandScientist/SI-DomMarBioMap

## Acknowledgments
- NatureScot for biotope occurrence data (GeMS database)
- Bio-ORACLE for environmental oceanographic data
- University of Aberdeen, funding internship to transition to R code

## Contact
**Tanya Riley**  
UHI Shetland  
Email: tanya.riley@uhi.ac.uk 
Project Website: [Marine Spatial Planning] (https://www.shetland.uhi.ac.uk/research/marine-spatial-planning/)

---

**Last Updated:** July 2026
**Version:** 1.0.0
