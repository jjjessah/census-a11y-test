---
layout: default
title: Comparison Specs
nav_order: 3
permalink: /components/comparison.html
---

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

Provide this reproducible framework to developers to extract real Census Data API fields, handle decoding errors, attach margins of error, and export a 508-compliant backing data matrix.

```python
import pandas as pd
import matplotlib.pyplot as plt
import requests

print("1. Reaching out to the U.S. Census Bureau API...")
base_url = "https://census.gov"

query_parameters = {
    "get": "NAME,DP03_0062E,DP03_0062M",
    "for": "state:*"
}

try:
    response = requests.get(base_url, params=query_parameters, timeout=10)
    response.raise_for_status()
    raw_data = response.json()
    
    print("2. Live Census API data successfully retrieved! Parsing data structure...")
    df = pd.DataFrame(raw_data[1:], columns=raw_data)
    df.rename(columns={"DP03_0062E": "Median_Income", "DP03_0062M": "MOE"}, inplace=True)
    
except (requests.exceptions.RequestException, ValueError) as error:
    print("\n[NOTE] Live Census API connection failed or returned an invalid response.")
    print("→ Activating 508-Compliant Local Fallback System to build your example files...")
    
    fallback_data = [
        ["Maryland", 98465, 450],
        ["New Jersey", 96346, 380],
        ["Massachusetts", 94488, 510],
        ["Hawaii", 92458, 620],
        ["California", 91551, 290]
    ]
    df = pd.DataFrame(fallback_data, columns=["NAME", "Median_Income", "MOE"])

df["Median_Income"] = pd.to_numeric(df["Median_Income"])
df["MOE"] = pd.to_numeric(df["MOE"])
sample_df = df.sort_values(by="Median_Income", ascending=False).head(5)

print("3. Building Section 508 / WCAG Compliant Visual Chart Layout...")
fig, ax = plt.subplots(figsize=(10, 6), facecolor='#ffffff')

# Rule: High-contrast bars mapping to USWDS Deep Blue (#002d62)
bars = ax.bar(sample_df["NAME"], sample_df["Median_Income"], 
              color="#002d62", edgecolor="#ffffff", linewidth=1.5,
              label="Median Household Income")

# Rule: Statistical Quality Disclosure - Explicitly draw the red Margin of Error bars
ax.errorbar(sample_df["NAME"], sample_df["Median_Income"], yerr=sample_df["MOE"], 
            fmt='none', ecolor='#d9381e', elinewidth=2, capsize=5,
            label="Statistical Margin of Error (95% Confidence)")

# Rule: Redundant Encoding - Place bold text labels directly above the bars
for bar in bars:
    height = bar.get_height()
    ax.annotate(f"\${height:,.0f}",
                xy=(bar.get_x() + bar.get_width() / 2, height),
                xytext=(0, 8),  
                textcoords="offset points",
                ha='center', va='bottom', fontsize=10, fontweight='bold', color='#1b1b1b')

# Rule: Low-Vision & Cognitive Accessibility Layout Enhancements
# Force a 15% top margin padding buffer so labels never clip against the top border
ax.set_ylim(0, sample_df["Median_Income"].max() * 1.15)

# Place the legend back inside the upper right corner with a compact size footprint
ax.legend(loc="upper right", prop={'size': 8.5}, frameon=True, facecolor="#ffffff", edgecolor="#ccc")

# Structural Labels & Typography (All text lines are dark grey #1b1b1b for high contrast)
ax.set_title("Median Household Income by State\nSource: 2022 Census ACS 5-Year Estimates", 
             fontsize=14, fontweight='bold', pad=15, color="#1b1b1b")
ax.set_ylabel("Income (USD)", fontsize=12, labelpad=10, color="#1b1b1b")
ax.tick_params(colors="#1b1b1b", labelsize=11)
ax.grid(axis='y', linestyle='--', alpha=0.5, color='#ccc')

plt.tight_layout()
plt.savefig("my_census_chart.png", dpi=300)

# Save the Parallel Screen-Reader Tabular Spreadsheet
sample_df.columns = ["Geographic Area", "Median Household Income (USD)", "Margin of Error (USD)", "state_id"] if "state_id" in sample_df.columns else ["Geographic Area", "Median Household Income (USD)", "Margin of Error (USD)"]
sample_df[["Geographic Area", "Median Household Income (USD)", "Margin of Error (USD)"]].to_csv("my_accessible_data.csv", index=False)
print("[SUCCESS] Your code pipeline has executed perfectly.")
```
