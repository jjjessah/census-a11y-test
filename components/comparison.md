---
layout: default
title: Comparison Visualizations
permalink: /components/comparison.html
---
**[← Home](/census-a11y-test/)** | **[Section 508 Requirements](/census-a11y-test/foundations/508.html)** | **Comparison Specs** | **[Geospatial Specs](/census-a11y-test/components/geospatial.html)**

***

# Comparison Visualizations (Bar Charts & Line Graphs)

Comparison visualizations are used to show data variations across distinct categories or tracking trends over a period of time. When displaying Census profiles, these elements must strictly integrate both visual and structural data guardrails to comply with Section 508 and Statistical Quality Standards.

---

## 1. Compliance Guardrails

*   **No Color-Only Data:** You must use distinct geometric markers (like solid circles or open squares) on line trends, or explicit text labels directly on bar charts. Color cannot be the only way to tell data series apart.
*   **Sequential Headings:** Ensure your code description layouts follow absolute hierarchy. This page uses a primary `# H1` title, followed by sequential `## H2` and `### H3` subheadings so screen readers can crawl sections flawlessly.
*   **Contrast Safeguards:** Any visual marker or text layer must maintain a minimum **4.5:1** contrast ratio against the background canvas.

---

## 2. Accessible Alternative Data Matrix

Every bar chart or line trend must be immediately followed by a matching, structured HTML table. This ensures users navigating via assistive technology can read data points linearly using native keyboard commands.

```html
<div class="usa-table-container">
  <table class="usa-table">
    <caption>Median Household Income by Target State (2022 ACS 5-Year Profiles)</caption>
    <thead>
      <tr>
        <th scope="col">Geographic Region</th>
        <th scope="col">Median Household Income (USD)</th>
        <th scope="col">Margin of Error (MOE)</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th scope="row">Sample State Alpha</th>
        <td>$85,000</td>
        <td>± $450</td>
      </tr>
    </tbody>
  </table>
</div>
```

---

## 3. Python Reference Blueprint

Provide this reproducible framework to developers to extract real Census Data API fields, attach margins of error, and export a 508-compliant backing data matrix.

```python
import pandas as pd
import matplotlib.pyplot as plt

# Programmatic data array mapping 
data = {
    "Geographic Area": ["State A", "State B", "State C"],
    "Median_Income":,
    "MOE": [350, 410, 290]
}
df = pd.DataFrame(data)

# Export clean parallel data file for screen-reader matrices
df.to_csv("accessible_comparison_data.csv", index=False)
print("Data matrix successfully exported for compliance verification.")
```
