🌊 Where Did the Land Go?

Char can't see the river changing. Satellites can. Now everyone can.



Team BUET_SIRIUS · Bangladesh University of Engineering and Technology (BUET)

</div>---

🌍 The Idea

Every year, Bangladesh's rivers and sea take land in one place and build it in another, and satellites have been watching for 30+ years.

We turn NASA/USGS Landsat images into a tide-aware map that shows, for every approximately 1 km grid cell along selected coastlines and riverbanks, what is changing, where, how fast, and whether the trend is statistically supported.

Our Opposite Trends view puts a shrinking shore next to a growing sandbar, so the same river can tell two stories at once. Char, a nearly blind Ganges river dolphin (shushuk), guides people through the map, their village's land story, and an optional kids' game.

«“Char can't see the river changing. Satellites can.”»

Every claim will be tested against the data and checked against published studies where a comparable validation source is available. We will communicate uncertainty and will not present planned analysis or sample values as verified findings.

«Project Status: This repository describes the project plan and prototype design. Scientific results, hotspot rankings, and validation claims will be published only after the analysis has been run and checked.»

---

🎯 Challenge Connection

The project is designed around the NASA Space Apps Challenge theme "Be An Earth System Trend Detective!" by investigating long-term changes visible in satellite observations.

Question| What the project aims to show
Where is it changing?| A map of land-loss and land-gain trends across selected coastal and riverine sites
What is changing?| Land fraction within approximately 1 km grid cells
How much is changing?| Estimated change over time, expressed in interpretable units where supported by the analysis
Is the trend statistically meaningful?| Trend-test results and significance after multiple-testing correction
What might explain the pattern?| Carefully sourced context about erosion, sediment deposition, tides, waves, and river dynamics

A central feature is Opposite Trends: a comparison of a location losing land with another location gaining land. The locations will be selected from actual results rather than assumed in advance.

---

✨ Key Features

1. Explore
   Explore a time series of land-change trends across selected study areas. A year selector and map-cell details will help users inspect changes over time.

2. Opposite Trends
   Compare land-loss and land-gain locations side by side, with trend charts and a plain-language explanation of the possible physical processes.

3. Hotspot Village
   Explore selected upazilas and view measured land-change summaries where the analysis supports them. Locations and rankings will be finalised only after the study area and results are verified.

4. Guess the Trend (Stretch Goal)
   An optional educational activity where users guess whether a location is gaining land, losing land, or showing no clear trend. It will use the same data definitions as the main application.

5. Char, the Science Guide
   Char provides short, accessible explanations in English and Bangla. The character is a storytelling guide, not a substitute for evidence or scientific uncertainty.

---

🔬 Planned Scientific Workflow

The analysis is designed to reduce common sources of error in satellite-based land and water mapping.

flowchart TD
    A["Landsat 5, 7, 8 and 9 observations"] --> B["Cloud and shadow masking"]
    B --> C["Dry-season image selection"]
    C --> D["MNDWI water detection with per-image thresholding"]
    D --> E["Water-frequency composite"]
    E --> F["Land fraction per year in ~1 km grid cells"]
    F --> G["Mann–Kendall trend test and Sen's slope"]
    G --> H["Benjamini–Hochberg FDR correction"]
    H --> I["Optional breakpoint analysis"]
    I --> J["Validation and uncertainty checks"]
    J --> K["Export documented results as JSON"]
    K --> L["Interactive web application"]

1. Image preparation

- Use Landsat Collection 2 Level-2 surface-reflectance imagery.
- Mask clouds, cloud shadows, and other unsuitable pixels using quality bands.
- Prioritise consistent seasonal windows to reduce seasonal variation.
- Document observation counts and data gaps for each year and location.

2. Water detection

Use the Modified Normalized Difference Water Index (MNDWI) as a candidate water-detection method:

[
MNDWI = \frac{Green - SWIR}{Green + SWIR}
]

Thresholds will be evaluated rather than assumed to work equally well across all landscapes. Water-frequency composites will help represent how often a pixel is classified as water during the selected observation period.

Important: Water frequency is not a complete tide correction. Tidal stage, seasonal river width, cloud cover, and image availability can still affect the results. These limitations will be documented.

3. Approximately 1 km grid

For each grid cell and year, estimate the fraction of valid observed area classified as land. Track valid observation coverage so that cells with insufficient data are not treated as reliable measurements.

4. Trend analysis

- Mann–Kendall test: Assess evidence of a monotonic trend.
- Sen's slope: Estimate the magnitude and direction of change over time.
- Benjamini–Hochberg false-discovery-rate correction: Account for testing many grid cells.
- Optional breakpoint analysis: Investigate possible changes in trend when sufficient observations are available.

A statistically significant trend does not automatically establish its cause. Results will distinguish measured patterns from possible explanations.

5. Validation

Compare the derived water maps with suitable reference products, including JRC Global Surface Water where appropriate. Consult published studies and CEGIS reports for relevant Bangladesh locations and methods.

Validation will document the reference period, spatial resolution, comparison method, and limitations. No accuracy percentage will be reported until it has been calculated from actual comparisons.

---

🛰️ Data Sources

Primary data: Landsat

Landsat is the scientific backbone of the project. MNDWI, water frequency, the approximately 1 km grid, trend tests, and optional breakpoints will be derived from Landsat observations.

Dataset| Period / purpose| Earth Engine collection ID
Landsat 5 TM, Collection 2 Level-2| Historical surface reflectance| "LANDSAT/LT05/C02/T1_L2"
Landsat 7 ETM+, Collection 2 Level-2| Historical surface reflectance| "LANDSAT/LE07/C02/T1_L2"
Landsat 8 OLI, Collection 2 Level-2| Surface reflectance from 2013 onward| "LANDSAT/LC08/C02/T1_L2"
Landsat 9 OLI-2, Collection 2 Level-2| Recent surface reflectance| "LANDSAT/LC09/C02/T1_L2"

Useful references:

- "NASA Landsat Level-2 Surface Reflectance Collection 2" (https://data.nasa.gov/dataset/landsat-level-2-surface-reflectance-collection-2)
- "USGS Landsat Collection 2 Level-2 STAC API" (https://landsatlook.usgs.gov/stac-server/collections/landsat-c2l2-sr/items)

Optional NASA context datasets

These datasets may help explain environmental context, but they will not replace Landsat-derived shoreline and riverbank measurements.

Dataset| Intended use| Resolution / limitation
"MODIS Terra MOD44W v6.1" (https://doi.org/10.5067/MODIS/MOD44W.061)| Independent land/water sanity check| 250 m; may not resolve narrow channels or small changes
"MODIS Aqua Kd490" (https://oceancolor.gsfc.nasa.gov/PRODUCTS/k490.html)| Qualitative water-clarity context for sediment-related discussion| Approximately 4–5 km pixels; not a direct erosion measurement
"NASA-SSH gridded sea-surface height" (https://podaac.jpl.nasa.gov/dataset/NASA_SSH_REF_SIMPLE_GRID_V1)| Broad sea-level context chart| Approximately 55 km grid; not suitable for local shoreline-change measurement

Google Earth Engine will serve as a processing platform, not as a dataset. JRC Global Surface Water is an additional cross-check product, not a substitute for independent ground truth.

Candidate study areas

Potential study areas include the Bay of Bengal coast, the Meghna estuary, Hatiya, and Bhola. Final areas will be selected after assessing imagery availability, spatial coverage, computational feasibility, and relevant validation sources.

---

🗺️ Prototype and User Experience

The prototype is planned as an interactive web application that turns geospatial analysis into an understandable public-facing story.

Screen| Purpose| Priority
Explore| Map, year selector, grid-cell details, trend and uncertainty information| Core
Opposite Trends| Side-by-side land-loss and land-gain comparison| Core
Hotspot Village| Location-level summaries for supported areas| Next
Guess the Trend| Educational activity for public engagement| Stretch

The interface will distinguish confirmed analysis outputs from illustrative prototype content. Sample values, if used during development, will be labelled clearly and will not be presented as observed results.

---

📦 Planned Repository Structure

.
├── README.md
├── assets/
│   ├── banner.png
│   ├── char.png
│   ├── char-and-screens.png
│   ├── method.png
│   ├── method-choices.png
│   ├── scope-roles.png
│   └── story.png
├── analysis/
│   ├── landsat_processing.js
│   ├── trend_analysis.py
│   └── validation.py
├── data-contract/
│   └── cell-trend.schema.json
├── docs/
│   ├── methodology.md
│   ├── validation.md
│   └── limitations.md
├── video/
└── web/

This is the intended structure. Only files that have actually been created should be treated as implemented components.

---

🧾 Data Contract

The web application will consume documented, versioned JSON outputs. A grid-cell record may contain fields like these:

{
  "cell_id": "example-cell-id",
  "study_area": "example-study-area",
  "period_start": 1990,
  "period_end": 2025,
  "grid_resolution_m": 1000,
  "trend_direction": "loss",
  "sen_slope_land_fraction_per_year": null,
  "p_value": null,
  "adjusted_p_value": null,
  "valid_year_count": 0,
  "observation_coverage": null,
  "validation_status": "pending",
  "uncertainty_notes": [
    "Illustrative schema only; not an observed result."
  ]
}

The record above is a schema illustration, not real data. Numeric result fields must remain unavailable or null until calculated. The final schema will document units, coordinate reference system, time period, missing-data conventions, and trend classification rules.

---

👥 Team and Responsibilities

Team BUET_SIRIUS · Bangladesh University of Engineering and Technology (BUET)

Responsibility| Main tasks
Earth Engine pipeline| Image selection, masking, water-frequency composites, annual land fraction, exports
Statistics| Trend testing, Sen's slope, multiple-testing correction, optional breakpoints
Science and validation| Literature review, validation checks, limitations, review of scientific claims
Frontend and mapping| Explore screen, map display, year selector, data integration
Storytelling and design| Char, English/Bangla explanations, visual design, video and presentation

Responsibilities may be shared or combined depending on the team's size and project timeline.

---

🗓️ Planned Milestones

1. Scope and data audit: Select candidate areas, verify data availability, and establish the observation period.
2. Processing prototype: Build and test cloud masking, seasonal filtering, MNDWI, and water-frequency composites.
3. Grid and statistics: Produce annual land-fraction series and implement trend testing.
4. Validation: Compare suitable reference products, inspect questionable locations, and document uncertainty.
5. Web integration: Connect verified outputs to the interactive map and Opposite Trends view.
6. Storytelling and delivery: Finalise Char's explanations, accessibility, demonstration video, and documentation.

Milestones describe the intended workflow and do not imply that each stage is already complete.

---

⚠️ Scientific Integrity and Limitations

- Landsat observations do not provide continuous monitoring; clouds, revisit timing, and missing observations affect coverage.
- Tides and seasonal water-level changes can influence apparent shoreline position.
- Sediment deposition, erosion, river migration, and coastal change are related but distinct processes.
- A change in water frequency is not automatically equivalent to a measured change in land area.
- A 1 km grid provides a useful regional overview but can hide local changes and narrow channels.
- Statistical significance does not prove a physical cause.
- Coarse-resolution context datasets must not be used to claim local shoreline measurements.
- Hotspot rankings, trend magnitudes, validation metrics, and village-level impact claims will only be published after calculation and review.

The project aims to communicate both what the data reveal and what they cannot yet establish.

---

📚 References and Resources

- "NASA Space Apps Challenge" (https://www.spaceappschallenge.org/)
- "NASA Landsat Level-2 Surface Reflectance Collection 2" (https://data.nasa.gov/dataset/landsat-level-2-surface-reflectance-collection-2)
- "USGS Landsat Collection 2 Level-2 STAC API" (https://landsatlook.usgs.gov/stac-server/collections/landsat-c2l2-sr/items)
- "Google Earth Engine Data Catalog" (https://developers.google.com/earth-engine/datasets)
- "JRC Global Surface Water" (https://global-surface-water.appspot.com/)
- Pekel, J.-F. et al. (2016). High-resolution mapping of global surface water and its long-term changes. Nature. "https://doi.org/10.1038/nature20584" (https://doi.org/10.1038/nature20584)
- "pyMannKendall documentation" (https://github.com/mmhs013/pyMannKendall)
- "MODIS MOD44W v6.1" (https://doi.org/10.5067/MODIS/MOD44W.061)
- "NASA Ocean Color Kd490" (https://oceancolor.gsfc.nasa.gov/PRODUCTS/k490.html)
- "NASA-SSH dataset" (https://podaac.jpl.nasa.gov/dataset/NASA_SSH_REF_SIMPLE_GRID_V1)

Additional Bangladesh-specific studies and CEGIS reports will be cited when they directly support the selected study areas or validation method.

---

💙 Our Goal

We want to make long-term land change easier to explore, understand, and discuss—connecting satellite observations with the places and communities affected by erosion and accretion.

Char can't see the river changing. Satellites can. Now everyone can.
