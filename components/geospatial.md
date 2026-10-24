---
layout: default
title: Geospatial Visualizations
nav_order: 4
permalink: /components/geospatial.html
---


# Geospatial Visualizations (Thematic & Choropleth Maps)

Geospatial visualizations rely heavily on color mapping across geographic boundaries to display data intensity. Because color-blind and screen-reader users cannot interpret raw graphic fill values alone, all map components must integrate strict behavioral and structural guardrails.

---

## 1. Compliance & Statistical Guardrails

*   **Categorical Binning Limits:** Maps must restrict data classifications to a maximum of **5 to 7 logical bins** or data intervals. Too many shades degrade visual contrast and cause cognitive fatigue.
*   **Redundant Boundaries:** Data boundaries (like borders between counties or states) must use distinct, high-contrast stroke outlines (minimum **3:1** contrast against map fills) so shapes remain clear to low-vision users.
*   **Interactive Focus:** If a map uses hover tooltips, the geographic regions must be keyboard-focusable. Users tabbing through the map must move sequentially through regions (e.g., alphabetically or by numeric FIPS code), triggering an audible data announcement via screen readers.

---

## 2. Accessible Alternative Data Matrix

Every thematic map asset must provide a parallel, screen-reader-accessible table grouped strictly by geographic region. Visual map layers (`<svg>`) must use `aria-hidden="true"` to prompt assistive technologies straight to this readable alternative matrix.

```html
<div class="usa-table-container">
  <table class="usa-table">
    <caption>Percent Population Change by State (2020 to 2022 Census Metrics)</caption>
    <thead>
      <tr>
        <th scope="col">FIPS Code</th>
        <th scope="col">Geographic Name</th>
        <th scope="col">Percentage Change (%)</th>
        <th scope="col">Margin of Error (MOE)</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th scope="row">01</th>
        <td>Sample State Alpha</td>
        <td>+ 1.4%</td>
        <td>± 0.1%</td>
      </tr>
    </tbody>
  </table>
</div>
```

---

## 3. R Reference Blueprint

Provide this reproducible tidycensus template to developers to cleanly query geospatial fields and generate accessible parallel text layers.

```R
library(tidycensus)
library(dplyr)

# Query geographic attributes from Census API
map_data <- get_acs(
  geography = "state",
  variables = "B01003_001", # Total Population
  year = 2022
)

# Export structured fallback data file matching the graphic layout
write.csv(map_data, "accessible_geospatial_matrix.csv", row.names = False)
print("Geospatial backing matrix successfully generated.")
```
