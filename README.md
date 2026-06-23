# Archipelagos_vessel_tracking_tool
Application that allows users to download data from Global Fishing Watch and create interactive maps. 

## Features

### 1. Automated Data Retrieval (`VP_gfw.py`)
* Asynchronously interfaces with the Global Fishing Watch API.
* Handles data partitioning into chunks to prevent memory overflows.
* Queries regional tracking data across a custom-defined maritime boundary envelope (covering areas spanning from Malta and the Ionian Sea up to the Aegean and Antalya regions).

### 2. Vessel Position & AIS Gap Analysis (`VP_map.py`, `VP_bulk_map.py`, `VP_report.py`)
* **Tracking Activity Evaluation:** Groups, chronologically sorts, and computes timestamps between successive coordinate pings.
* **Suspicious Gap Flagging:** Pinpoints where AIS transponders might have been intentionally deactivated based on distance limits relative to a reference shore/AIS buffer layer (`ais_buffer_Xnm.geojson`).
* **Encounter Event Isolation:** Utilizes high-performance metric spatial joins (`GeoPandas` vector processing) to identify when separate tracking vessels are suspiciously close to one another in time and space (suggesting transshipment or unauthorized contact).
* **Interactive Visualization:** Yields detailed Leaflet/Folium track maps distinguishing between normal paths, gaps, encounter events, and localized environmental restrictions.

### 3. Apparent Fishing Effort Matrix (`AFE_map.py`, `AFE_bulk_map.py`, `AFE_report.py`)
* **Grid-Based Mapping:** Aggregates fishing hours onto a localized spatial vector meshgrid.
* **Interactive Heatmaps:** Renders high-fidelity Folium heatmaps layered under an interactive matrix that reveals ship breakdown configurations and country metrics on hover or click.
* **National Border Analysis:** Automatically groups and intersects operational activities against territorial borders for Greece (GRC), Turkey (TUR), Italy (ITA), and Malta (MLT).

### 4. Cross-Platform Desktop GUI (`main.py`)
* Built using a modern, hardware-accelerated dark-themed workspace driven by `CustomTkinter`.
* Fully handles background execution using Python multithreading (`threading` + `asyncio`) to ensure the graphical user interface stays responsive during large API syncs and map generation.
* Integrates native drag-and-drop file ingestion via `tkinterdnd2`.

---

## Repository Structure

```text
├── main.py                  # Core GUI Application (Layout, Threading, and File Routing)
├── VP_gfw.py                # Asynchronous API Engine handling Global Fishing Watch queries
├── VP_map.py                # Visual Mapper specializing in singular vessel AIS paths & gap segments
├── VP_bulk_map.py           # Bulk processing script for fleet tracking map compiles and encounters
├── VP_report.py             # Analytical compiler yielding spatial statistics to structured CSV sheets
├── AFE_map.py               # Renders targeted fishing effort heatmaps & transparent info-grids
├── AFE_bulk_map.py          # Aggregates wide-scale multi-vessel fishing grid matrices
├── AFE_report.py            # Generates a horizontal regional comparison grid across territorial boundaries
├── requirements.txt         # Project software dependency manifests
└── map_files/               # Critical reference spatial vector configurations
    ├── ais_buffer_3nm.geojson         # Coastal buffer layer to filter legitimate near-shore pings
    ├── grc_territorial_waters.geojson # Greece maritime boundary polygon
    ├── tur_national_waters.geojson    # Turkey maritime boundary polygon
    ├── ita_national_waters.geojson    # Italy maritime boundary polygon
    ├── mlt_FMZ_waters.geojson         # Malta Fisheries Management Zone vector bounds
    └── mlt_national_waters.geojson    # Malta baseline territorial waters (12NM)
